---
title : "Tạo AWS Lambda Auto-Deployer và Amazon EventBridge Rule"
date : 2026-07-27 
weight : 5
chapter : false
pre : " <b> 5.3.5 </b> "
---

#### Tạo AWS Lambda Auto-Deployer
Hàm Lambda này nhận sự kiện từ Model Registry khi mô hình đạt nhãn Approved để tự động tạo EndpointConfig và cập nhật Serverless Endpoint.
- Vào AWS Lambda => chọn Create function.
- Name: TelcoChurnAutoDeployer | Runtime: Python 3.11.
- Trong tab Permissions, gán Role có quyền AmazonSageMakerFullAccess và đính kèm Inline Policy iam:PassRole cho SageMaker-Telco-Churn-Role.
![inline-policy](/images/5-Workshop/5.3-Implementation/inline-policy.png)
- Dán đoạn code dưới đây vào lambda_function.py và bấm Deploy:

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
#### Cấu hình Amazon EventBridge Rule
Thiết lập "mắt thần" bắt sự kiện phê duyệt mô hình để gọi Lambda Deployer tự động.
- Vào Amazon EventBridge => Rules => Bấm Create rule.
- Name: TelcoChurnModelApprovedRule.
![bridge-name](/images/5-Workshop/5.3-Implementation/bridge-name.png)
- Event pattern: Chọn Custom pattern (JSON editor) và dán:

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
- Select Target: Chọn Lambda function => Chọn TelcoChurnAutoDeployer.
- ![target](/images/5-Workshop/5.3-Implementation/target.png)
- Bấm Create rule.

#### Cấu hình EventBridge Rule thông báo kết quả SageMaker Pipeline qua Email
Khi SageMaker Pipeline kết thúc (thành công, thất bại, hoặc bị dừng), EventBridge Rule này sẽ lập tức bắt sự kiện và gửi Email thông qua Amazon SNS.
##### Tạo EventBridge Rule (TelcoChurnPipelineStatusRule)
- Truy cập Amazon EventBridge => Rules => Bấm Create rule.
- Name: TelcoChurnPipelineSuccessRule.
![success-event-name](/images/5-Workshop/5.3-Implementation/success-event-name.png)
- Tại mục Event pattern, chọn Custom pattern (JSON editor) và dán chính xác đoạn JSON của bạn:

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
##### Cấu hình Target gửi sang SNS Topic
- Tại mục Select a target:
  - Target 1: Chọn AWS service.
  - Select a target: Chọn SNS topic.
  - Topic: Chọn SNS Topic đã tạo trước đó (TelcoChurnAlerts).
    ![success-target](/images/5-Workshop/5.3-Implementation/success-target.png)
- Mở phần Additional settings => Target input transformer nếu muốn định dạng lại nội dung Email cho dễ đọc (Option):
  - Target input: Chọn Input transformer => Bấm Configure input transformer.
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
        "Cảnh báo SageMaker Pipeline Execution:"
        "Pipeline: <pipelineName>"
        "Trạng thái: <status>"
        "Thời gian: <time>"
        "Execution ARN: <executionArn>"
    ```
    ![target-template](/images/5-Workshop/5.3-Implementation/target-template.png)
- Bấm Next => Next => Create rule.