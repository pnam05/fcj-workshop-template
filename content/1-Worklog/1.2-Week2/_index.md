---
title: "Week 2 Worklog"
date: 2026-06-15
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

* Advanced study of object storage management with Amazon S3 (Storage Classes, Versioning, Lifecycle Rules, Bucket Policies & CORS).
* Deep dive into AWS IAM security mechanisms (IAM Roles, Service Trust, Least Privilege principle & iam:PassRole authorization).
* Study secure networking infrastructure with Amazon VPC Endpoints (Gateway Endpoints vs Interface Endpoints).

### Tasks to implement this week:

| Day | Task                                                                                                                                                                                                                                                         | Start Date | Completion Date | Reference Documentation                                                          |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | -------------------------------------------------------------------------------- |
| Mon | - Advanced study of Amazon S3 service: <br>&emsp; + Compare S3 Storage Classes (Standard, Intelligent-Tiering, Glacier, Deep Archive) <br>&emsp; + Configure S3 Versioning & automated S3 Lifecycle Rules for tiering                                        | 15/06/2026 | 15/06/2026      | <https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html> |
| Tue | - Study data protection & encryption mechanisms on Amazon S3: <br>&emsp; + Server-side encryption SSE-S3 & SSE-KMS using AWS Key Management Service <br>&emsp; + Write and configure S3 Bucket Policies & Cross-Origin Resource Sharing (CORS)               | 16/06/2026 | 16/06/2026      | <https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-policies.html>     |
| Wed | - Deep dive into AWS IAM Security: <br>&emsp; + Distinguish IAM User vs IAM Role, Assume Role mechanics & Service Trust Relationships <br>&emsp; + Enforce Principle of Least Privilege using Customer Managed Policies                                      | 17/06/2026 | 17/06/2026      | <https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html>                 |
| Thu | - Research iam:PassRole service authorization permission: <br>&emsp; + Mechanism allowing AWS services to pass a Role to another service to execute tasks <br>&emsp; + Configure PassRole to limit service invocation scope following AWS security standards | 18/06/2026 | 18/06/2026      | <https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html>    |
| Fri | - Research VPC Endpoints (AWS PrivateLink) infrastructure: <br>&emsp; + Compare Gateway Endpoints (Amazon S3, DynamoDB) vs Interface Endpoints <br>&emsp; + Private access to AWS services from VPC without going through public Internet                    | 19/06/2026 | 19/06/2026      | <https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints.html>          |

### Week 2 Achievements:

* Mastered cost-optimal S3 Storage Class selection and S3 Lifecycle data tiering rules.
* Understood SSE-KMS encryption mechanisms and access control using S3 Bucket Policies.
* Mastered IAM Role security mindset, iam:PassRole authorization, and Least Privilege principles.
* Mastered VPC Endpoints concepts for establishing private and secure internal connections to Amazon S3.
