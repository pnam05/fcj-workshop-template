---
title: "Week 3 Worklog"
date: 2026-06-22
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Deep dive into AWS Cloud Machine Learning Managed Services ecosystem: Study Amazon ECR, Amazon SageMaker Estimators, and SageMaker Training Jobs operational mechanics.
* Research automated hyperparameter optimization mechanisms with SageMaker Hyperparameter Tuning (HPO).
* Study data preprocessing mechanisms with SageMaker Processing Jobs and centralized log management via Amazon CloudWatch Logs.

### Tasks to implement this week:

| Day | Task                                                                                                                                                                                                                                                          | Start Date | Completion Date | Reference Documentation                                                       |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------------------------------------------- |
| Mon | - Study Amazon Elastic Container Registry (ECR) service: <br>&emsp; + Manage Docker Container Images, ECR Public vs Private Registries <br>&emsp; + Understand SageMaker mechanism for pulling Built-in Containers from ECR for standard algorithms           | 22/06/2026 | 22/06/2026      | <https://docs.aws.amazon.com/ecr/>                                            |
| Tue | - Research Amazon SageMaker Training Jobs architecture: <br>&emsp; + Configure Training Instances, S3 Data Input/Output Channels <br>&emsp; + Understand SageMaker Estimator SDK configuration for classification and prediction tasks                        | 23/06/2026 | 23/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-training.html>  |
| Wed | - Study SageMaker Hyperparameter Tuning (HPO) mechanics: <br>&emsp; + Hyperparameter search strategies: Bayesian Optimization, Random Search, Hyperband <br>&emsp; + Configure HyperparameterRanges (Continuous, Integer, Categorical Ranges)                 | 24/06/2026 | 24/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning.html> |
| Thu | - Study SageMaker Processing Jobs concepts: <br>&emsp; + SKLearnProcessor, ScriptProcessor concepts in SageMaker Python SDK <br>&emsp; + Configure ProcessingInput (pointing to S3) and ProcessingOutput (pointing to S3) for large data processing workloads | 25/06/2026 | 25/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/processing-job.html>         |
| Fri | - Learn how to integrate Amazon CloudWatch Logs for SageMaker Jobs: <br>&emsp; + Understand how SageMaker automatically pushes stdout/stderr from containers to CloudWatch Logs <br>&emsp; + Inspect and analyze logs to troubleshoot job execution failures  | 26/06/2026 | 26/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/logging-cloudwatch.html>     |

### Week 3 Achievements:

* Mastered container image storage and pulling mechanics from Amazon ECR to SageMaker Managed environments.
* Understood SageMaker Training Jobs execution architecture and data streaming from Amazon S3 into container instances.
* Mastered automated hyperparameter tuning principles and HyperparameterRanges search space configurations.
* Understood how to package data processing workloads with SKLearnProcessor and monitor logs centrally via CloudWatch Logs.
