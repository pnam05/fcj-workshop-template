---
title: "Prerequisites"
date: 2026-07-27
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# Prerequisites for the Workshop

Before proceeding with the implementation of the MLOps system on AWS, please prepare the required account, IAM access permissions, S3 storage structure, and execution environment according to the instructions below.

---

## Account & Permissions Requirements (AWS Account & IAM)
- **AWS Account:** Have access to AWS Management Console.
- **IAM Permissions:** The IAM user account needs AdministratorAccess or minimum policies including:
  - AmazonSageMakerFullAccess
  - AWSLambda_FullAccess
  - AmazonS3FullAccess
  - AmazonEventBridgeFullAccess
  - AmazonSNSFullAccess
  - AmazonAPIGatewayAdministrator
  - IAMFullAccess (to configure PassRole)

---

## Initialize IAM Roles for Services
The system uses the following IAM Roles to delegate permissions between services (Principle of Least Privilege):

1. **SageMaker-Telco-Churn-Role:**
   - **Service Trust:** sagemaker.amazonaws.com
   - **Attached Policies:** AmazonSageMakerFullAccess, AmazonS3FullAccess.
   - **Purpose:** Grants permissions for SageMaker Processing Job, HPO Job, Evaluation, and Serverless Endpoint to access S3 data.

![telco-churn-role](/images/3-Prerequiste/telco-churn-role.png)

2. **Lambda-Execution-Role (used for Lambda Trigger & Lambda Deployer):**
   - **Service Trust:** lambda.amazonaws.com
   - **Attached Policies:** AWSLambdaBasicExecutionRole, AmazonSageMakerFullAccess, AmazonS3FullAccess.
   - **Inline Policy (PassRolePolicy):** Allows Lambda to execute iam:PassRole to SageMaker-Execution-Role:
     ```json
     {
         "Version": "2012-10-17",
         "Statement": [
             {
                 "Effect": "Allow",
                 "Action": "iam:PassRole",
                 "Resource": "arn:aws:iam::<YOUR_ACCOUNT_ID>:role/<SageMaker-Execution-Role-Name>"
             }
         ]
     }
     ```

---

## Create S3 Bucket & Data Lake Directory Structure
Create 1 single S3 Bucket with a globally unique name (e.g., telco-churn-mlops-<account-id>).

Create directory structure (Prefixes) inside the Bucket as follows:
```text
telco-churn-mlops-<account-id>/
├── raw/                 # Contains raw data (.csv)
├── processed/           # Contains preprocessed data
│   ├── train/
│   ├── validation/
│   └── test/
└── models/              # Contains model.tar.gz compressed files
```
![s3](/images/3-Prerequiste/S3.png)


## Prepare Data & Coding Environment (Local / SageMaker Studio)
- **Dataset:** Download initial training data file WA_Fn-UseC_-Telco-Customer-Churn.csv.
- **SageMaker Studio / Jupyter Notebook:** Initialize Python 3.10+ environment on SageMaker Studio.