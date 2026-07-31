---
title: "Prerequisites"
date: 2026-07-27
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# Prerequisites for the Workshop

Before proceeding with the implementation of the MLOps system on AWS, please prepare the required account, IAM access permissions, supporting IAM Roles, S3 storage structure, and execution environment according to the instructions below.

---

## 1. Account & Permissions Requirements (AWS Account & IAM)

- **AWS Account:** Registered and with access to **AWS Management Console**.
- **AWS Region:** Recommended to use **us-east-1 (N. Virginia)** or **ap-southeast-1 (Singapore)** to ensure full service availability (SageMaker Serverless Inference, CloudFront, AWS WAF, EventBridge).
- **IAM Permissions:** The IAM user account needs **AdministratorAccess** or a minimum set of Managed Policies including:
  - AmazonSageMakerFullAccess
  - AWSLambda_FullAccess
  - AmazonS3FullAccess
  - AmazonEventBridgeFullAccess
  - AmazonSNSFullAccess
  - AmazonAPIGatewayAdministrator
  - CloudWatchLogsFullAccess
  - AWSCloudFrontFullAccess
  - AWSWAFv2FullAccess
  - IAMFullAccess (to create and attach the `iam:PassRole` inline policy)

---

## 2. Initialize IAM Roles for Services (Service Roles)

The system adheres to the **Principle of Least Privilege**, using separate IAM Roles to delegate permissions across AWS services:

### 2.1. SageMaker Execution Role
- **Service Trust:** sagemaker.amazonaws.com
- **Attached Policies:** AmazonSageMakerFullAccess, AmazonS3FullAccess
- **Purpose:** Grants permissions for SageMaker Processing Job, HPO Job, Evaluation, SageMaker Pipeline, and Serverless Endpoint to read/write S3 data.

![telco-churn-role](/images/3-Prerequiste/telco-churn-role.png)

### 2.2. Lambda Execution Role
- **Service Trust:** lambda.amazonaws.com
- **Attached Policies:** AWSLambdaBasicExecutionRole, AmazonSageMakerFullAccess, AmazonS3FullAccess, AmazonSNSFullAccess.
- **Inline Policy (PassRolePolicy):** Required configuration allowing Lambda to pass roles (`iam:PassRole`) to **SageMaker-Telco-Churn-Role** when launching Pipelines and deploying Endpoints:
  ```json
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": "iam:PassRole",
              "Resource": "arn:aws:iam::<YOUR_ACCOUNT_ID>:role/SageMaker-Telco-Churn-Role"
          }
      ]
  }
  ```

---

## 3. Prepare Data & Coding Environment

- **Telco Customer Churn Dataset:** Download the initial training data file `WA_Fn-UseC_-Telco-Customer-Churn.csv` from standard IBM / Kaggle datasets.
- **Coding Environment:** Initialize a Python 3.10+ environment on **SageMaker Studio / JupyterLab Notebook** or local environment with necessary libraries:
  ```bash
  pip install boto3 sagemaker pandas numpy scikit-learn xgboost
  ```