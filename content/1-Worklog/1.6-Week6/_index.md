---
title: "Week 6 Worklog"
date: 2026-07-13
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Collaborate with team members to build and connect the complete SageMaker Pipeline on AWS environment.
* Individual learning on Event-Driven Architecture on AWS with Amazon EventBridge, S3 Event Notifications, and AWS Lambda Serverless Compute.
* Study Data Drift / Model Drift monitoring mechanics and automated retraining architectures on Cloud.

### Tasks to implement this week:

| Day | Task                                                                                                                                                                                                                                           | Start Date | Completion Date | Reference Documentation                                                               |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------------------- |
| Mon | - Advanced study of Event-Driven Architecture on AWS: <br>&emsp; + Publish/Subscribe pattern and EventBridge as central Router <br>&emsp; + Learn writing Event Patterns using JSON for precise Cloud resource event filtering                 | 13/07/2026 | 13/07/2026      | <https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html>            |
| Tue | - Study Amazon S3 Event Notifications mechanics: <br>&emsp; + Configure s3:ObjectCreated:* events on S3 Bucket Prefixes <br>&emsp; + Connect S3 Event Triggers sending notification payloads to AWS Lambda / Amazon SQS / SNS                  | 14/07/2026 | 14/07/2026      | <https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventNotifications.html>       |
| Wed | - Learn AWS Lambda Serverless Compute: <br>&emsp; + Lambda function lifecycle, Execution Role configuration, Memory Allocation & Timeout limits <br>&emsp; + Writing Lambda functions in Python using boto3 SDK to interact with AWS resources | 15/07/2026 | 15/07/2026      | <https://docs.aws.amazon.com/lambda/latest/dg/welcome.html>                           |
| Thu | - Study Data Drift & Model Drift concepts on Cloud: <br>&emsp; + Concept of input data distribution shifts over time <br>&emsp; + Data quality monitoring solutions and automated retraining workflow triggers                                 | 16/07/2026 | 16/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-data-quality.html>     |
| Fri | Build SageMaker Pipeline with team - ProcessingStep => TuningStep => EvalStep => ConditionStep (AUC >= 0.80) => ModelStep (Register) <br> - Test Pipeline execution                                                                            | 17/07/2026 | 17/07/2026      | [SageMaker Pipelines](https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines.html) |

### Week 6 Achievements:

* Successfully built 4-step SageMaker Pipeline with team: Processing (data preparation) => HPO (tuning XGBoost) => Evaluation (checking AUC) => Condition/Register. Pipeline automatically approves model when AUC >= 0.80.
* Mastered Event-Driven automation mindset using EventBridge, AWS Lambda, and S3 Event Triggers.
* Understood event messaging mechanics from Amazon S3 to AWS Lambda for triggering Serverless tasks.
* Mastered Data Drift monitoring principles and automated model retraining pipeline configurations.
