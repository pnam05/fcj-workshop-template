---
title : "Test Retrain & Automated Deployment Flow"
date : 2026-07-29
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

#### 1. Simulate Uploading New Data File to S3
- Go to S3 Console $\rightarrow$ Buckets $\rightarrow$ telco-churn-mlops-fcaj $\rightarrow$
raw/ -> Upload.
- Upload new data file
![new](/images/5-Workshop/5.4-Test-Validation/up-newdata.png)

#### 2. Check Data Drift Alert & Trigger Pipeline (Lambda Drift Checker)
- Open AWS Lambda $\rightarrow$ select function TelcoChurnDriftChecker $\rightarrow$ select Monitor tab $\rightarrow$ View CloudWatch logs.
- Inspect the latest execution log:
  - Expected log: Lambda detects new file raw/new_telecom_data.csv, performs drift check, shoots email via SNS and calls start_pipeline_execution().
  ![log](/images/5-Workshop/5.4-Test-Validation/newdata-log.png)
- Open Gmail inbox to check warning Email from Amazon SNS.
  ![newdata-gmail](/images/5-Workshop/5.4-Test-Validation/newdata-gmail.png)

#### 3. Monitor Execution Progress of SageMaker Pipeline
- Access Amazon SageMaker $\rightarrow$ select Pipelines $\rightarrow$ select TelcoChurnPipeline.
- Click on the latest execution run (Execution ID) to observe the 4-step execution graph:
  - TelcoChurnProcessStep (Succeeded) $\rightarrow$ TelcoChurnHpoStep (Succeeded) $\rightarrow$ TelcoChurnEvalStep (Succeeded) $\rightarrow$ TelcoChurnCheckAUCThreshold (True) $\rightarrow$ TelcoChurnRegisterModelStep (Approved).
  
![pipeline](/images/5-Workshop/5.4-Test-Validation/pipeline.png)
- Confirm Pipeline result Email: When Pipeline completes, EventBridge Rule (TelcoChurnPipelineStatusRule) captures event and sends confirmation email:
 ![done-gmail](/images/5-Workshop/5.4-Test-Validation/done-gmail.png)

#### 4. Confirm Automated Deployment to Serverless Endpoint (Lambda Deployer)
- Access Amazon SageMaker $\rightarrow$ select Model Registry $\rightarrow$ TelcoChurnModelGroup.
  - You will see the new model registered with the latest version (Version) and Approved status.
   ![model-registry](/images/5-Workshop/5.4-Test-Validation/model-registry.png)
- EventBridge Rule (TelcoChurnModelApprovedRule) captures Approved event and automatically triggers TelcoChurnAutoDeployer.
- Check logs of Lambda Deployer (TelcoChurnAutoDeployer):
    ![deploy-logs](/images/5-Workshop/5.4-Test-Validation/deploy-logs.png)
- Access Amazon SageMaker $\rightarrow$ Endpoints $\rightarrow$ telco-churn-serverless-endpoint:
  - Endpoint status will transition from Updating to InService with the new EndpointConfig pointing to the retrained model Version!
  
![inservice](/images/5-Workshop/5.4-Test-Validation/inservice.png)
