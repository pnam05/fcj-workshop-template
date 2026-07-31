---
title: "Blog 2"
date: 2026-07-10
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# SECURITY IN SOFTWARE DEVELOPMENT — BEYOND SECURE CODING

### 1. Introduction

Throughout the journey of approaching and deploying solutions on AWS, an essential reality becomes clear: in early development stages, engineers often prioritize **"ensuring the application functions correctly"**. However, when the system is released to the Internet, the core challenge shifts: **How to guarantee absolute system security?**

An application with high functional performance can still be vulnerable if a single misconfiguration or small security flaw exists, potentially leading to data leaks or infrastructure compromise. The AWS ecosystem not only provides compute infrastructure but also equips a suite of specialized security services, enabling secure architecture design right from the start rather than reactively fixing incidents.

---

### 2. 5 Core Security Lessons on AWS Infrastructure

#### 2.1. Strict Management of Access Keys

Hardcoding credentials directly into source code is one of the most common mistakes in cloud infrastructure management:
```
AWS_ACCESS_KEY="AKIAxxxxxxxx"
AWS_SECRET_KEY="xxxxxxxxxxxxxxxx"
```
When source code containing access keys is pushed to public repositories, attackers can instantly exploit these key pairs to hijack AWS resources. Numerous real-world incidents show accounts being abused for cryptocurrency mining or spawning massive fleets of EC2 instances, causing severe financial damage within hours.

To eliminate credential leak risks, AWS recommends adopting standard identity management mechanisms:

- **IAM Roles**
- **Environment Variables**
- **AWS Secrets Manager**
- **AWS Systems Manager Parameter Store**

#### 2.2. Principle of Least Privilege

In IAM access management, the core principle is to **grant only the minimum permissions required to perform a task, and nothing more**.

*Example:* An EC2 instance only needs to read data from Amazon S3. Instead of assigning the **AmazonS3FullAccess** policy, the optimal configuration should only grant **s3:GetObject** permission on the specific target bucket. This approach minimizes the blast radius if an account or service is compromised — a mandatory standard in enterprise architecture.

#### 2.3. Subnet Segmentation Configuration

A common architectural flaw is placing all services into a Public Subnet — creating direct network connectivity from the Internet to Backend applications and Databases. If a Database has a Public IP paired with loose Security Group rules, port scanning and network attack risks increase dramatically.

A secure standard architectural model requires a clear separation across tiers:

> **Internet → Load Balancer → Backend (Public Subnet) → Database (Private Subnet)**

Accordingly, the Database is isolated in a Private Subnet and only accepts internal access from the Backend tier. This is a standard architecture pattern recommended in the **AWS Well-Architected Framework**.

#### 2.4. Layered Protection for Web Applications

Even when source code is optimized, applications still face application-layer attacks such as SQL Injection, Cross-Site Scripting (XSS), automated bot attacks, or DDoS.

**AWS WAF (Web Application Firewall)** acts as a traffic filtering layer before requests reach the application. The service provides capabilities to:

- Block access from malicious IP addresses
- Rate limit incoming requests per IP
- Identify and block attack patterns according to OWASP standards
- Inspect and filter out payloads containing malicious code

Consequently, the system reduces processing overhead on the Backend while boosting defense capabilities against Internet threats.

#### 2.5. Continuous Security & Behavior Monitoring

Infrastructure security does not stop at initial configuration but requires **continuous monitoring**. AWS provides a comprehensive suite of security monitoring services:

- **Amazon GuardDuty:** Analyzes log data from AWS CloudTrail, VPC Flow Logs, and DNS Logs to detect anomalous behavior — such as logins from unusual locations, EC2 instances generating abnormal outbound traffic, or automated botnet activity. GuardDuty synthesizes findings and issues real-time alerts for proactive mitigation.

- **Amazon Inspector:** Focuses on detecting internal software and infrastructure vulnerabilities. It automatically scans EC2 instances, Container Images, and software libraries to discover end-of-life dependencies, CVE vulnerabilities, or security misconfigurations.

- **AWS Security Hub:** Functions as a centralized security management hub. It aggregates assessment results from GuardDuty, Inspector, IAM Access Analyzer, AWS Config, and Macie into a single pane of glass, giving operation teams a holistic view of the system's security posture.

---

### 3. Solving Real-World Operational Challenges

#### 3.1. Performance Optimization During Traffic Spikes

Network congestion or system overload often occurs when user traffic exceeds the serving capacity of single-instance infrastructure, leading to increased latency or request timeouts.

By combining **Amazon EC2 Auto Scaling** and **Elastic Load Balancer (ELB)**, the system can automatically launch additional compute instances and evenly distribute incoming traffic. This ensures high availability and stable performance even during sudden traffic spikes.

#### 3.2. Stateless Architecture & Independent Data Management

Storing user uploads (media, files) directly on local server storage (e.g., **uploads/** directory on EC2) creates a massive bottleneck when scaling out or replacing instances, increasing data loss risks.

Adopting **Amazon S3** as a dedicated object storage service for images, documents, and backups completely decouples Storage from Compute. This model not only secures data but also optimizes overall system scalability.

---

### 4. Conclusion

Cloud infrastructure implementation proves that: **Security is not an afterthought, but must be a core principle in system architecture design from day one**. The majority of security incidents stem not from code syntax bugs, but from misconfigurations and operational oversights.

Core governance rules to follow:

- Never store access keys in source code
- Strictly enforce the Principle of Least Privilege
- Thoroughly isolate Public and Private subnets
- Establish continuous security monitoring and logging systems
- Periodically scan for vulnerabilities and update dependencies
- Ensure fault tolerance and scalability for infrastructure

For real-world engineering problems, understanding the technical nature and problems solved by AWS security services shapes a standardized, secure, and production-ready system mindset.

---

**Authors:** Thành Nhân, Nguyễn Cảnh Nguyên, Nguyễn Trọng Nhân, Nam Phan, Nguyễn Bá Nam.

**Link Blog:** [Security in Software Development on AWS](https://www.facebook.com/groups/awsstudygroupfcj/?multi_permalinks=2228803837884576&notif_id=1785383944402087&notif_t=feedback_reaction_generic_tagged)

**References:**
- [IAM Best Practices (AWS Docs)](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [Well-Architected – Security – IAM](https://docs.aws.amazon.com/wellarchitected/latest/framework/sec-iam.html)
- [AWS WAF](https://docs.aws.amazon.com/waf/latest/developerguide/what-is-aws-waf.html)
- [Amazon GuardDuty](https://docs.aws.amazon.com/guardduty/latest/ug/what-is-guardduty.html)
- [Amazon Inspector](https://aws.amazon.com/inspector/)