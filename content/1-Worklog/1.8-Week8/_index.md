---
title: "Week 8 Worklog"
date: 2026-07-27
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* Write Blog 1 (RDS Proxy) and Blog 2 (AWS Security) with team.
* Individual learning on AWS Well-Architected Framework (5 pillars of standardized Cloud system design).
* Individual learning on Cost Optimization via AWS Cost Explorer, AWS Budgets, and safe Cloud resource clean-up practices.

### Tasks to implement this week:

| Day | Task                                                                                                                                                                                                                                                                                                          | Start Date | Completion Date | Reference Documentation                                                                              |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ---------------------------------------------------------------------------------------------------- |
| Mon | - Advanced study of AWS Well-Architected Framework (5 pillars: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization) <br> - Analyze operational costs via AWS Cost Explorer and set AWS Budgets Alarm for cost management                                                 | 27/07/2026 | 27/07/2026      | <https://aws.amazon.com/architecture/well-architected/>                                              |
| Tue | Learn about RDS Proxy with team: Connection Pooling, Multiplexing, Graceful Failover, IAM Authentication <br> - Write Blog 1 with team: "Connection Exhaustion Problem with RDS Proxy"                                                                                                                        | 28/07/2026 | 28/07/2026      | [RDS Proxy](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-proxy.html)                   |
| Wed | Learn about AWS Security with team: IAM Least Privilege, WAF, GuardDuty, Security Hub, Public/Private Subnet <br> - Write Blog 2 with team: "Security in Software Development on AWS"                                                                                                                         | 29/07/2026 | 29/07/2026      | [AWS Well-Architected Security](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/) |
| Thu | - Study integration testing methodologies and build performance criteria matrices (Validation Matrix) for Serverless systems on Cloud                                                                                                                                                                         | 30/07/2026 | 30/07/2026      | <https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/welcome.html>                 |
| Fri | - Study Cloud resource clean-up best practices (Resource Clean-up Best Practices): <br>&emsp; + Steps to delete Endpoints, empty S3 Buckets, delete Lambda functions, API Gateway & EventBridge Rules <br>&emsp; + Ensure AWS account incurs no unintended ongoing resource costs after internship completion | 31/07/2026 | 31/07/2026      | <https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html>                       |

### Week 8 Achievements:

* Authored and published Blog 1 with team - detailed analysis of Connection Exhaustion when combining Lambda + RDS, and how RDS Proxy solves it via Multiplexing, Graceful Failover, and IAM Authentication.
* Authored and published Blog 2 with team - summary of 5 practical security lessons when developing on AWS: no hardcoded Access Keys, Least Privilege, Public/Private Subnet separation, WAF protection, GuardDuty/Inspector/Security Hub monitoring.
* Mastered core principles of AWS Well-Architected Framework, analyzing and optimizing Cloud infrastructure costs using AWS Budgets and Cost Explorer.
* Understood integration testing principles and Validation Matrix construction for evaluating Cloud system reliability.
* Mastered standard resource clean-up procedures on AWS Cloud, protecting accounts from unintended charges.
