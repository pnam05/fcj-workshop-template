---
title: "Worklog Week 1"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Week 1 Objectives:

* Connect with members of the First Cloud AI Journey (FCAJ) and understand the internship rules.
* Master basic AWS Cloud services (IAM, S3, EC2) and get familiar with the AWS Console & AWS CLI interfaces.
* Survey the Telco Customer Churn prediction problem and shape the objectives for the MLOps project.

### Tasks to implement this week:

| Day | Task | Start Date | Completion Date | Reference Documentation |
| --- | --- | --- | --- | --- |
| Mon | - Attend Onboarding session, meet FCAJ members <br> - Read and note rules, regulations, and discipline at the internship unit | 15/06/2026 | 15/06/2026 | FCAJ Internship Regulations |
| Tue | - Learn overview of AWS Cloud & foundational service groups: <br>&emsp; + Identity & Access Management (IAM) <br>&emsp; + Compute (EC2) <br>&emsp; + Storage (S3) <br>&emsp; + Networking (VPC, Security Group) | 16/06/2026 | 16/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Wed | - Set up AWS Free Tier account & basic security configuration (MFA) <br> - Install AWS CLI v2 on local machine <br> - **Hands-on:** Create IAM User, create Access Key, configure `aws configure` (Region `ap-southeast-1`) and check connection via terminal | 17/06/2026 | 17/06/2026 | AWS Documentation |
| Thu | - Research Amazon S3 storage service & Amazon EC2 virtualization service <br> - Survey the Telco Customer Churn dataset (`WA_Fn-UseC_-Telco-Customer-Churn.csv`) <br> - Identify technical requirements and technologies needed for the MLOps system | 18/06/2026 | 18/06/2026 | Kaggle / AWS SageMaker Docs |
| Fri | - **Hands-on:** <br>&emsp; + Create test S3 Bucket via AWS CLI <br>&emsp; + Launch EC2 Instance (Amazon Linux 2), connect SSH via Terminal <br>&emsp; + Discuss with Mentor on the direction for building an automated MLOps Platform | 19/06/2026 | 19/06/2026 | AWS Hands-on Labs |

### Week 1 Accomplishments:

* Understood internship rules, workflows, and connected well with FCAJ group members.
* Successfully installed and configured AWS CLI v2 on personal machine with IAM Access Key (Region `ap-southeast-1`).
* Mastered basic concepts and operations on core services:
  * **IAM:** Create Role, grant Least Privilege permissions, and understand `PassRole`.
  * **S3:** Concepts of Bucket, Prefix, Object, and access permissions.
  * **EC2:** Launch instance, configure Security Group (Inbound/Outbound rules), and secure SSH connection.
* Completed survey of the Telco Customer Churn dataset and finalized the architectural direction of the automated MLOps Platform on AWS for the personal project.
