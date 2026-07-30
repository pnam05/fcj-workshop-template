---
title: "Week 7 Worklog"
date: 2026-07-20
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Collaborate with team members to code Lambda functions, configure API Gateway, and test Real-time Inference APIs.
* Individual learning on Serverless inference architecture on AWS Cloud with SageMaker Serverless Inference.
* Individual learning on Amazon API Gateway (HTTP API vs REST API), Amazon EventBridge Rules, and alerting systems with CloudWatch Alarms & Amazon SNS.

### Tasks to implement this week:

| Day | Task                                                                                                                                                                                                                                                   | Start Date | Completion Date | Reference Documentation                                                                                   |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | --------------------------------------------------------------------------------------------------------- |
| Mon | - Individual learning on SageMaker Serverless Inference: <br>&emsp; + Cost optimization advantages, automatic scale-to-zero when idle <br>&emsp; + Configure memory allocation MemorySizeInMB (1024MB - 6144MB) and MaxConcurrency limits              | 20/07/2026 | 20/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/serverless-endpoints.html>                               |
| Tue | - Deep dive into Amazon API Gateway: <br>&emsp; + Compare HTTP API vs REST API (HTTP API >70% more cost-effective for Serverless workloads) <br>&emsp; + Payload Format Version (v1.0 vs v2.0) in Lambda Proxy Integration, Throttling & CORS Security | 21/07/2026 | 21/07/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api.html>                              |
| Wed | - Study advanced EventBridge Rules filtering: <br>&emsp; + Write JSON Event Patterns capturing AWS resource state transitions <br>&emsp; + Assign Targets triggering AWS Lambda functions or sending SNS notifications                                 | 22/07/2026 | 22/07/2026      | <https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html>                         |
| Thu | - Research Amazon CloudWatch & Amazon SNS: <br>&emsp; + Configure CloudWatch Alarms based on performance metrics (Latency, 5XX Invocation Errors) <br>&emsp; + Integrate Amazon SNS Topics to automatically send operational alert emails to engineers | 23/07/2026 | 23/07/2026      | <https://docs.aws.amazon.com/cloudwatch/latest/monitoring/AlarmThatSendsEmail.html>                       |
| Fri | End-to-End Testing with team: upload new data => DriftChecker trigger => Pipeline run => Model Registered => Deployer update Endpoint => Predict API returns result <br> - Analyze CloudWatch logs to verify each step                                 | 24/07/2026 | 24/07/2026      | [CloudWatch Log Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html) |

### Week 7 Achievements:

* Completed End-to-End Testing of all 5 stages with team - everything working accurately from data ingestion to inference via API.
* Mastered real-time API communication interfaces and monitoring architecture concepts on AWS Cloud.
* Mastered SageMaker Serverless Inference concepts and memory/concurrency tuning for cost optimization.
* Understood HTTP API Gateway integration with Lambda Proxy Integration and alerting system setup using CloudWatch Alarms & Amazon SNS.
