---
title: "Week 6 Worklog"
date: 2026-07-20
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Deepen AWS Cloud knowledge in Event-Driven Architecture: Deep dive into **Amazon EventBridge** (Bus, Rules, Event Patterns), **S3 Event Notifications**, and **AWS Lambda Serverless Compute**.
* Write and configure AWS Lambda function TelcoChurnDriftChecker attached to S3 Event Notification to automatically check Data Drift and launch SageMaker Pipeline when new data arrives.
* Write and deploy AWS Lambda function TelcoChurnAutoDeployer combined with EventBridge Rule capturing Approved events from Model Registry to automatically create/update **SageMaker Serverless Endpoint** (Continuous Deployment - CD).

### Tasks to implement this week:

| Day | Task | Start Date | Completion Date | Reference Documentation |
| --- | --- | --- | --- | --- |
| Mon | - Advanced learning on **Event-Driven Architecture on AWS**: <br>&emsp; + Learn Publish/Subscribe pattern and how EventBridge acts as central Router <br>&emsp; + Learn writing Event Patterns in JSON for precise event filtering <br>&emsp; + Optimize AWS Lambda (Execution Role, Timeouts, Memory Allocation, Error Handling) | 20/07/2026 | 20/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - Program AWS Lambda function TelcoChurnDriftChecker in Python 3.11: <br>&emsp; + Read newly uploaded CSV file from S3 via boto3 <br>&emsp; + Evaluate data distribution drift risk (Data Drift / Missing values) <br>&emsp; + Send warning email via SNS and call sagemaker_client.start_pipeline_execution() <br> - **Hands-on:** Create S3 Event Notification on raw/ folder connecting directly to Lambda | 21/07/2026 | 21/07/2026 | AWS Lambda & S3 Developer Guide |
| Wed | - Learn **SageMaker Serverless Inference**: <br>&emsp; + Advantages over Real-time Provisioned Endpoints (Cost optimization, automatic scale to 0 when no traffic) <br>&emsp; + Configure MemorySizeInMB (2048 MB) and MaxConcurrency <br> - Write AWS Lambda function TelcoChurnAutoDeployer using boto3 library to automatically create Model, Endpoint Configuration and call update_endpoint() / create_endpoint() | 22/07/2026 | 22/07/2026 | SageMaker Serverless Inference Docs |
| Thu | - Configure **Amazon EventBridge Rule** (TelcoChurnModelApprovedRule): <br>&emsp; + Set up Event Pattern filtering SageMaker Model Package State Change events <br>&emsp; + Filter specifically ModelPackageGroupName: TelcoChurnModelGroup and ModelApprovalStatus: Approved <br>&emsp; + Assign Target routing to Lambda function TelcoChurnAutoDeployer | 23/07/2026 | 23/07/2026 | EventBridge User Guide |
| Fri | - Test integration of complete Automation Chain: <br>&emsp; 1. Upload new CSV file to s3://.../raw/ $\rightarrow$ Lambda Drift Checker triggers Pipeline <br>&emsp; 2. Pipeline finishes execution $\rightarrow$ Registers Approved Model into Registry <br>&emsp; 3. EventBridge detects Approved Model $\rightarrow$ Triggers Lambda Deployer <br>&emsp; 4. Endpoint telco-churn-serverless-endpoint transitions state to Updating $\rightarrow$ InService successfully | 24/07/2026 | 24/07/2026 | AWS CloudWatch Logs |

### Week 6 Accomplishments:

* Mastered mindset of setting up event-driven automation systems on Cloud, mastering AWS Lambda, EventBridge Rules, and S3 Event Triggers services.
* Successfully built and integrated **Lambda Drift Checker**: Automatically detects newly uploaded data in s3://.../raw/, validates quality, and automatically triggers SageMaker Pipeline execution.
* Successfully built and deployed fully automated **Continuous Deployment (CD)** flow:
  * EventBridge Rule listens accurately for model approval (Approved) events.
  * Lambda TelcoChurnAutoDeployer automatically initializes Model, Endpoint Config, and updates new model to **SageMaker Serverless Endpoint** with Zero-Downtime Deployment.
