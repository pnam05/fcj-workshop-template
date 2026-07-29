---
title : "Clean-up Resources"
date : 2026-07-29
weight : 6
chapter : false
pre : " <b> 5.5. </b> "
---

To avoid incurring unnecessary charges on your AWS account after completing workshop testing, perform resource clean-up following the sequence of steps below.

#### Delete SageMaker Serverless Endpoint & Configurations
- Access Amazon SageMaker Studio $\rightarrow$ Deployments $\rightarrow$ Endpoints: Select telco-churn-serverless-endpoint $\rightarrow$ Delete.
![clean-server](../../../static/images/5-Workshop/5.4-Test-Validation/clean-server.png)
- Select JumpStart / Models: Select TelcoChurnModelGroup $\rightarrow$ Delete.
![clean-group](../../../static/images/5-Workshop/5.4-Test-Validation/clean-group.png)

#### Delete SageMaker Pipeline
- In SageMaker Console, open Pipelines section.
- Select TelcoChurnPipeline (or your Pipeline name).
- Click Delete and confirm Pipeline deletion.
![clean-pipeline](../../../static/images/5-Workshop/5.5-Cleanup/clean-pipeline.png)