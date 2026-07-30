---
title: "Week 3 Worklog"
date: 2026-06-29
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Deepen advanced AWS Cloud knowledge in Storage Management (Amazon S3 Storage Classes, Bucket Policies), IAM Security (Assume Role, Principle of Least Privilege), and Networking (VPC Endpoints).
* Initialize Data Lake storage architecture on Amazon S3 with a standard directory structure hierarchy (raw/, processed/, models/).
* Package data preprocessing step into SageMaker ProcessingStep using SKLearnProcessor and successfully connect to S3 Bucket.

### Tasks to implement this week:

| Day | Task | Start Date | Completion Date | Reference Documentation |
| --- | --- | --- | --- | --- |
| Mon | - Advanced learning on **Amazon S3** service: <br>&emsp; + Differentiate Storage Classes (Standard, Intelligent-Tiering, Glacier) <br>&emsp; + Configure S3 Versioning, Lifecycle Rules & Encryption (SSE-S3, SSE-KMS) <br>&emsp; + Learn S3 Bucket Policy & CORS <br> - **Hands-on:** Initialize main S3 Bucket telco-churn-mlops-fcaj and create folder structure (raw/, processed/, models/) | 29/06/2026 | 29/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - In-depth research on **AWS IAM Security**: <br>&emsp; + IAM Roles vs IAM Users, Service Trust concepts <br>&emsp; + iam:PassRole mechanism and assigning permissions to AWS services <br>&emsp; + Learn VPC Endpoints concept (Gateway vs Interface) for secure internal S3 access <br> - **Hands-on:** Create SageMaker-Execution-Role with minimal policies adhering to Least Privilege standard | 30/06/2026 | 30/06/2026 | AWS Security Best Practices |
| Wed | - Upload raw dataset WA_Fn-UseC_-Telco-Customer-Churn.csv to folder s3://telco-churn-mlops-fcaj/raw/ using AWS CLI <br> - Study SageMaker Python SDK documentation regarding SKLearnProcessor and ProcessingStep objects <br> - Deploy preprocessing.py script to SageMaker Studio environment to prepare for packaging | 01/07/2026 | 01/07/2026 | SageMaker SDK Documentation |
| Thu | - Configure ProcessingInput (pointing to raw data file on S3) and ProcessingOutput (pointing to train/, validation/, test/ folders) <br> - Write source code to launch trial independent ProcessingStep on ml.m5.large instance | 02/07/2026 | 02/07/2026 | AWS Hands-on Labs |
| Fri | - Inspect execution log of Processing Job on Amazon CloudWatch Logs <br> - Verify output results on S3: Confirm 3 files train.csv, validation.csv, test.csv are automatically generated and uploaded correctly to s3://telco-churn-mlops-fcaj/processed/ <br> - Package complete ProcessingStep code to prepare for integration into SageMaker Pipeline | 03/07/2026 | 03/07/2026 | CloudWatch Logs Console |

### Week 3 Accomplishments:

* Mastered AWS security knowledge: Deeply understood IAM Role delegation mechanisms, Least Privilege policy, and setting up secure connections to Amazon S3 via VPC Endpoints.
* Successfully set up S3 Data Lake telco-churn-mlops-fcaj with clear hierarchical directory structure, enabling default Encryption to protect customer data.
* Initialized and assigned exact permissions for SageMaker-Execution-Role, allowing SageMaker to access S3 and execute container jobs.
* Successfully packaged and executed TelcoChurnProcessStep using SKLearnProcessor on SageMaker:
  * Automatically reads raw data from s3://.../raw/.
  * Performs data cleaning, encoding, and splitting.
  * Automatically writes output to 3 corresponding S3 directories (processed/train, processed/validation, processed/test) without manual intervention.
