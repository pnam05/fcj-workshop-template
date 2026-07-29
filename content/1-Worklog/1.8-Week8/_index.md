---
title: "Worklog Week 8"
date: 2026-08-03
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* Deepen AWS Cloud knowledge in Cost Optimization & Operational Excellence (AWS Well-Architected Framework & Cost Optimization): Learn to analyze costs via AWS Cost Explorer, set AWS Budgets, and optimize Serverless resources.
* Perform comprehensive End-to-End testing and validation matrix for both Auto-Retrain flow and Real-time Inference API.
* Finalize bilingual Workshop documentation (Vietnamese - English), present final internship project acceptance report to Mentor, and perform clean-up of all AWS Cloud resources.

### Tasks to implement this week:

| Day | Task | Start Date | Completion Date | Reference Documentation |
| --- | --- | --- | --- | --- |
| Mon | - Advanced learning on **AWS Well-Architected Framework** (5 pillars: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization) <br> - Analyze system operation costs via **AWS Cost Explorer** and set up **AWS Budgets Alarm** warning when cost exceeds $10 USD/month <br> - Evaluate Serverless cost optimization solutions (Auto-scaling, Concurrency limits) | 03/08/2026 | 03/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - **End-to-End Testing Scenario 1 (Automated Retrain Flow):** <br>&emsp; + Simulate Admin uploading new data file to `s3://.../raw/` <br>&emsp; + Confirm Lambda Drift Checker triggers SageMaker Pipeline <br>&emsp; + Verify 4-step Pipeline executes green, sending SNS Email notifying `Succeeded` <br>&emsp; + Verify EventBridge triggers Lambda Deployer automatically updating Serverless Endpoint | 04/08/2026 | 04/08/2026 | AWS Console Testing |
| Wed | - **End-to-End Testing Scenario 2 (Real-time Inference Flow):** <br>&emsp; + Send multiple test JSON payload samples via API Gateway HTTP Endpoint using cURL/Postman <br>&emsp; + Test Churn prediction cases (`Yes`/`No`) and evaluate Latency <br>&emsp; + Build complete Test Results Summary Table (**Validation Matrix**) covering all criteria | 05/08/2026 | 05/08/2026 | Postman / cURL Tests |
| Thu | - Review and finalize all internship report documentation & Workshop documentation on Hugo website: <br>&emsp; + Check completeness of both languages (**Vietnamese** and **English**) <br>&emsp; + Add architecture diagrams, proof of work images from CloudWatch Logs and cURL responses <br> - Present project acceptance results to Mentor | 06/08/2026 | 06/08/2026 | FCAJ Website Template |
| Fri | - **Perform Resource Clean-up:** <br>&emsp; + Delete SageMaker Serverless Endpoint & Configurations <br>&emsp; + Empty and delete S3 Data Lake Bucket <br>&emsp; + Delete AWS Lambda functions, API Gateway, EventBridge Rules & SNS Topic <br> - Wrap up internship period and close Worklog | 07/08/2026 | 07/08/2026 | Workshop Clean-up Guide |

### Week 8 Accomplishments:

* Mastered core principles of **AWS Well-Architected Framework**, learned analyzing and optimizing Cloud infrastructure costs using AWS Budgets and Cost Explorer (maintaining total project cost under $5 USD/month).
* Excellent 100% completion of End-to-End testing scenarios:
  * Automated Retrain and Auto-Deploy event chain operated accurately without errors.
  * API Gateway returned Real-time inference results with low latency (~45ms - 80ms post Cold Start) and accurate responses.
* Fully published bilingual Workshop documentation (Vietnamese - English) on the personal internship reporting website with complete proof images and step-by-step guidance.
* Successfully presented project acceptance report to Mentor and performed Clean-up of all resources on AWS, ensuring no unintended charges incur on the account.
