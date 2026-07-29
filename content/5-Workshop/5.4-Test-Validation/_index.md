---
title : "End-to-End System Testing"
date : 2026-07-29 
weight : 4 
chapter : false
pre : " <b> 5.4. </b> "
---

#### Overview
This section guides End-to-End testing of the entire MLOps system across 2 main scenarios:

- Automated Retrain & Continuous Deployment Flow Testing (Automated Retrain & CD Flow): Simulate Admin uploading a new data file to S3 to trigger the event chain S3 -> Lambda Drift Checker -> SageMaker Pipeline -> Model Registry -> EventBridge -> Lambda Deployer -> Serverless Endpoint.

- Real-time Inference Flow Testing (Real-time Inference Flow): Send HTTP POST Request from Client via API Gateway to test processing capability and return of Churn Prediction results.
