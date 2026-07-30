---
title: "Week 7 Worklog"
date: 2026-07-27
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Deepen AWS Cloud knowledge in API Management & Operational Excellence: Deep dive into **Amazon API Gateway** (REST API vs HTTP API), **Amazon CloudWatch Alarms**, and **Amazon SNS Notifications**.
* Deploy public REST API gateway for Real-time Inference using **API Gateway (HTTP API)** combined with AWS Lambda function TelcoChurnPredictHandler connecting directly to SageMaker Serverless Endpoint.
* Configure automated monitoring and incident alert system: Capture Pipeline completion events via EventBridge Rule and send status notification emails via Amazon SNS, setting up CloudWatch Alarms for 5XX errors.

### Tasks to implement this week:

| Day | Task | Start Date | Completion Date | Reference Documentation |
| --- | --- | --- | --- | --- |
| Mon | - Advanced learning on **Amazon API Gateway**: <br>&emsp; + Compare HTTP API vs REST API (HTTP API is >70% more cost-effective and reduces latency for Serverless workloads) <br>&emsp; + Payload Format Version types (v1.0 vs v2.0) in Lambda Proxy Integration <br>&emsp; + Manage Stages, Throttling, and CORS Security | 27/07/2026 | 27/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - Program AWS Lambda Predict Handler function (telco-churn-api-handler): <br>&emsp; + Extract features array from HTTP request body <br>&emsp; + Call sagemaker_runtime.invoke_endpoint() sending CSV payload <br>&emsp; + Receive Churn probability, format JSON result (churn_probability, prediction: "CHURN"/"RETAIN") <br> - **Hands-on:** Create Amazon API Gateway (HTTP API), create route POST /predict linked to Lambda Predict Handler | 28/07/2026 | 28/07/2026 | AWS API Gateway & Lambda Docs |
| Wed | - Learn **Operational Monitoring & Alerting** on Cloud: <br>&emsp; + Manage Log Groups, Metric Filters in Amazon CloudWatch Logs <br>&emsp; + Set up CloudWatch Alarms based on performance metrics (Latency, Invocation Errors) <br>&emsp; + Integrate Amazon SNS Topic to send automated email alerts to operations team | 29/07/2026 | 29/07/2026 | AWS CloudWatch User Guide |
| Thu | - Configure second EventBridge Rule (TelcoChurnPipelineStatusRule): <br>&emsp; + Capture SageMaker Model Building Pipeline Execution Status Change events <br>&emsp; + Filter terminal execution states (Succeeded, Failed, Stopped) <br>&emsp; + Configure Target sending directly to Amazon SNS Topic TelcoChurnAlerts to shoot automated Retrain result emails to Gmail | 30/07/2026 | 30/07/2026 | EventBridge Target Options |
| Fri | - Configure **CloudWatch Alarm** (Invocation5XXErrors > 0) on SageMaker Endpoint connected to SNS Topic <br> - **Hands-on testing:** <br>&emsp; + Send cURL request via PowerShell to HTTP API Gateway URL (https://c6kbjaktj9.execute-api.ap-southeast-1.amazonaws.com/predict) <br>&emsp; + Inspect execution logs on CloudWatch Logs to measure processing latency and confirm successful response return | 31/07/2026 | 31/07/2026 | CloudWatch Logs Console |

### Week 7 Accomplishments:

* Mastered knowledge of setting up real-time API communication gateways and operational monitoring architecture on AWS Cloud, understanding reasons for choosing HTTP API to optimize costs and performance.
* Successfully built and deployed **Real-time Inference API**:
  * Seamlessly integrated **API Gateway (HTTP API)** $\rightarrow$ **Lambda Predict Handler** $\rightarrow$ **SageMaker Serverless Endpoint**.
  * Successfully received prediction requests and returned accurate results (churn_probability: 0.0406, prediction: "RETAIN").
* Finalized **Monitoring & Notification System**:
  * Automatically sent Email notifications of SageMaker Pipeline Retrain execution results (Succeeded/Failed) to Gmail inbox via Amazon SNS.
  * Established CloudWatch Alarms ready to trigger alerts when the system encounters 5XX error incidents or abnormal latency increases.
