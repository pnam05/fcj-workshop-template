---
title : "Create AWS Lambda Auto-Deployer & Amazon EventBridge Rule"
date : 2026-07-27 
weight : 5
chapter : false
pre : " <b> 5.3.5 </b> "
---

#### Create AWS Lambda Auto-Deployer
This Lambda function receives events from Model Registry when the model reaches Approved status to automatically create EndpointConfig and update Serverless Endpoint.
- Go to AWS Lambda $\rightarrow$ select Create function.
- Name: TelcoChurnAutoDeployer | Runtime: Python 3.11.
- In Permissions tab, assign Role with AmazonSageMakerFullAccess permissions and attach Inline Policy iam:PassRole for SageMaker-Telco-Churn-Role.
![inline-policy](/images/5-Workshop/5.3-Implementation/inline-policy.png)
- Paste the code below into lambda_function.py and click Deploy:

```python
import json
import boto3
import time
from botocore.exceptions import ClientError

sagemaker = boto3.client('sagemaker')

ENDPOINT_NAME = "telco-churn-serverless-endpoint"
ROLE_ARN = "arn:aws:iam::606754308917:role/SageMaker-Telco-Churn-Role"

def lambda_handler(event, context):
    print("Received event:", json.dumps(event))
    
    try:
        detail = event.get('detail', {})
        
        approval_status = detail.get('ModelApprovalStatus') or detail.get('modelApprovalStatus')
        
        model_package_arn = detail.get('ModelPackageArn') or detail.get('modelPackageArn')
        
        if approval_status != "Approved":
            print(f"Model status is '{approval_status}'. Skipping deployment.")
            return {
                'statusCode': 200, 
                'body': json.dumps(f"Status is {approval_status}, skipped deployment.")
            }
        
        if not model_package_arn:
            raise ValueError("ModelPackageArn is missing in event payload.")
            
        timestamp = int(time.time())
        version = model_package_arn.split('/')[-1]
        
        model_name = f"telco-churn-model-v{version}-{timestamp}"
        endpoint_config_name = f"telco-churn-config-v{version}-{timestamp}"
        
        print(f"Creating model: {model_name}...")
        sagemaker.create_model(
            ModelName=model_name,
            ExecutionRoleArn=ROLE_ARN,
            Containers=[{
                'ModelPackageName': model_package_arn
            }]
        )

        print(f"Creating endpoint config: {endpoint_config_name}...")
        sagemaker.create_endpoint_config(
            EndpointConfigName=endpoint_config_name,
            ProductionVariants=[{
                'VariantName': 'AllTraffic',
                'ModelName': model_name,
                'ServerlessConfig': {
                    'MemorySizeInMB': 2048,
                    'MaxConcurrency': 10
                }
            }]
        )
        
        try:
            print(f"Updating existing endpoint: {ENDPOINT_NAME}...")
            sagemaker.update_endpoint(
                EndpointName=ENDPOINT_NAME,
                EndpointConfigName=endpoint_config_name
            )
            msg = f"Successfully triggered UPDATE for endpoint {ENDPOINT_NAME} with model {model_name}"
        except ClientError as e:
            error_code = e.response['Error']['Code']
            error_msg = e.response['Error']['Message']
            
            if error_code == 'ValidationException' or "Could not find endpoint" in error_msg:
                print(f"Endpoint '{ENDPOINT_NAME}' not found. Creating new endpoint...")
                sagemaker.create_endpoint(
                    EndpointName=ENDPOINT_NAME,
                    EndpointConfigName=endpoint_config_name
                )
                msg = f"Successfully triggered CREATE for new endpoint {ENDPOINT_NAME} with model {model_name}"
            else:
                raise e
        
        print(msg)
        return {
            'statusCode': 200,
            'body': json.dumps(msg)
        }
        
    except Exception as e:
        print(f"[ERROR] Deployment failed: {str(e)}")
        raise e
```
#### Configure Amazon EventBridge Rule
Set up "watchdog" capturing model approval events to call Lambda Deployer automatically.
- Go to Amazon EventBridge $\rightarrow$ Rules $\rightarrow$ Click Create rule.
- Name: TelcoChurnModelApprovedRule.
![bridge-name](/images/5-Workshop/5.3-Implementation/bridge-name.png)
- Event pattern: Select Custom pattern (JSON editor) and paste:

```json
{
  "source": ["aws.sagemaker"],
  "detail-type": ["SageMaker Model Package State Change"],
  "detail": {
    "ModelPackageGroupName": ["TelcoChurnModelGroup"],
    "ModelApprovalStatus": ["Approved"]
  }
}
```
- ![event-name](/images/5-Workshop/5.3-Implementation/event-pattern.png)
- Select Target: Select Lambda function $\rightarrow$ Select TelcoChurnAutoDeployer.
- ![target](/images/5-Workshop/5.3-Implementation/target.png)
- Click Create rule.

#### Configure EventBridge Rule notifying SageMaker Pipeline results via Email
When SageMaker Pipeline finishes (succeeded, failed, or stopped), this EventBridge Rule will immediately capture the event and send an Email via Amazon SNS.
##### Create EventBridge Rule (TelcoChurnPipelineStatusRule)
- Access Amazon EventBridge $\rightarrow$ Rules $\rightarrow$ Click Create rule.
- Name: TelcoChurnPipelineSuccessRule.
![success-event-name](/images/5-Workshop/5.3-Implementation/success-event-name.png)
- Under Event pattern, select Custom pattern (JSON editor) and paste your exact JSON snippet:

```json
{
  "source": ["aws.sagemaker"],
  "detail-type": ["SageMaker Model Building Pipeline Execution Status Change"],
  "detail": {
    "pipelineArn": ["arn:aws:sagemaker:ap-southeast-1:606754308917:pipeline/TelcoChurnPipeline"],
    "currentPipelineExecutionStatus": ["Succeeded", "Failed", "Stopped"]
  }
}
```
- ![success-json](/images/5-Workshop/5.3-Implementation/success-json.png)
##### Configure Target sending to SNS Topic
- Under Select a target:
  - Target 1: Select AWS service.
  - Select a target: Select SNS topic.
  - Topic: Select SNS Topic created previously (TelcoChurnAlerts).
    ![success-target](/images/5-Workshop/5.3-Implementation/success-target.png)
- Open Additional settings $\rightarrow$ Target input transformer if you wish to reformat the Email content for readability (Optional):
  - Target input: Select Input transformer $\rightarrow$ Click Configure input transformer.
  - Input path:
   ```json
    {
        "pipeline": "$.detail.pipelineExecutionArn",
        "status": "$.detail.currentPipelineExecutionStatus",
        "time": "$.time"
    }
    ```
    ![target-input](/images/5-Workshop/5.3-Implementation/target-input.png)
    - Template:
    ```
        "SageMaker Pipeline Execution Alert:"
        "Pipeline: <pipelineName>"
        "Status: <status>"
        "Time: <time>"
        "Execution ARN: <executionArn>"
    ```
    ![target-template](/images/5-Workshop/5.3-Implementation/target-template.png)
- Click Next $\rightarrow$ Next $\rightarrow$ Create rule.