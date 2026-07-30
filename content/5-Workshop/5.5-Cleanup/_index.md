---
title : "Clean-up Resources"
date : 2026-07-29
weight : 6
chapter : false
pre : " <b> 5.5. </b> "
---

To avoid incurring unnecessary charges on your AWS account after completing workshop testing, perform resource clean-up following the sequence of steps below.

#### Delete SageMaker Serverless Endpoint & Configurations
- Access Amazon SageMaker Studio => Deployments => Endpoints: Select telco-churn-serverless-endpoint => Delete.
![clean-server](/images/5-Workshop/5.5-Cleanup/clean-server.png)
- Select JumpStart / Models: Select TelcoChurnModelGroup => Delete.
![clean-group](/images/5-Workshop/5.5-Cleanup/clean-group.png)

#### Delete SageMaker Pipeline
- In SageMaker Console, open Pipelines section.
- Select TelcoChurnPipeline (or your Pipeline name).
- Click Delete and confirm Pipeline deletion.
![clean-pipeline](/images/5-Workshop/5.5-Cleanup/clean-pipeline.png)

#### Delete Amazon API Gateway & AWS Lambda Functions
##### Delete API Gateway
- Access Amazon API Gateway.
- Find API telco-churn-api => Click Delete => Confirm deletion.
![clean-api](/images/5-Workshop/5.5-Cleanup/clean-api.png)

##### Delete Lambda Functions
- Access AWS Lambda => Functions.
- Check the 3 Lambda functions created during the workshop:
  - TelcoChurnDriftChecker  
  - TelcoChurnAutoDeployer
  - telco-churn-api-handler 
- Click Actions => Delete => Type delete to confirm.
![clean-lambda](/images/5-Workshop/5.5-Cleanup/clean-lambda.png)

#### Delete Amazon EventBridge Rules & Amazon SNS Topic
##### Delete EventBridge Rules
- Access Amazon EventBridge => Rules.
- Check the Rules:
  - TelcoChurnModelApprovedRule
  - TelcoChurnPipelineSuccessRule
- Click Delete.
![clean-event](/images/5-Workshop/5.5-Cleanup/clean-event.png)

##### Delete SNS Topic
- Access Amazon SNS => Topics.
- Select Topic TelcoChurnAlerts => Click Delete.
![clean-sns](/images/5-Workshop/5.5-Cleanup/clean-sns.png)
- Go to Subscriptions => Delete the Subscription associated with your email.
![clean-email](/images/5-Workshop/5.5-Cleanup/clean-email.png)

##### Delete S3 Bucket & Data
- Access Amazon S3.
- Select Bucket telco-churn-mlops-fcaj => Click Empty => Type permanently delete to clear all raw data, processed data, and model artifacts.
![empty-s3](/images/5-Workshop/5.5-Cleanup/empty-s3.png)
- After emptying, select the Bucket again and click Delete to remove the Bucket from the system.
![delete-s3](/images/5-Workshop/5.5-Cleanup/delete-s3.png)

#### Delete Amazon CloudFront Distribution & AWS WAF
##### Disable and Delete CloudFront Distribution
- Access **Amazon CloudFront** => **Distributions**.
- Select Distribution telco-churn-cloudfront-waf.
- Click **Disable** and wait until status changes to Disabled.
![disable](/images/5-Workshop/5.5-Cleanup/disable.png)
- Once disabled, select the distribution again and click **Delete** to remove completely.

##### Delete Web ACLs on AWS WAF
- Access **AWS WAF & Shield** => **Web ACLs**.
- Select region **Global (CloudFront)**.
- Select the Web ACL associated with your CloudFront Distribution => Click **Action** => **Delete**.
![acls](/images/5-Workshop/5.5-Cleanup/acls.png)