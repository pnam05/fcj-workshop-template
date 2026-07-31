---
title: "Published Tech Blogs"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

During our internship at FCAJ, our team conducted technical research, engaged in architectural discussions, and published three in-depth technical blogs on AWS Cloud solutions for the [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj) community. Each article synthesizes theoretical cloud concepts and practical insights gained from resolving real-world operational challenges.

---

### [Blog 1 - RESOLVING CONNECTION EXHAUSTION WITH RDS PROXY](3.1-Blog1/)

An in-depth analysis of the **Connection Exhaustion** issue when combining Serverless architectures (AWS Lambda) with traditional relational databases (Amazon RDS). The article details root causes of connection bottlenecks when thousands of Lambda instances scale concurrently, and presents **Amazon RDS Proxy** as the solution built on three core pillars: Multiplexing, Graceful Failover, and IAM Authentication.


---

### [Blog 2 - SOFTWARE DEVELOPMENT SECURITY ON AWS](3.2-Blog2/)

A comprehensive guide covering 5 essential security principles for developing and deploying applications on AWS: Secure credential management, enforcing **Least Privilege**, network segmentation via Public/Private Subnets, application-layer defense with **AWS WAF**, and continuous threat detection using **GuardDuty, Inspector, and Security Hub**. Includes practical solutions for real-world challenges such as traffic spikes and stateless storage design.


---

### [Blog 3 - INFRASTRUCTURE MANAGEMENT WITH TERRAFORM](3.3-Blog3/)

An exploration of **Infrastructure as Code (IaC)** using HashiCorp Terraform to manage AWS cloud infrastructure. The article details the core transition from manual console operations ("ClickOps") to code-driven automation, covering essential mechanisms like plan/apply execution workflows, state management via **Remote Backend (S3 & DynamoDB)**, code reusability through **Terraform Modules**, and handling **Infrastructure Drift**.
