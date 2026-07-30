---
title: "Event 3"
date: 2026-07-11
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# EVENT SUMMARY REPORT: AWS COMMUNITY DAY & TECH WORKSHOP

**Event Name:** AWS Community Day & Tech Workshop  
**Date:** July 11th 2026
**Location:** 26th Floor, Bitexco Financial Tower, Ho Chi Minh City  
**Main Topics:** Web App Security with AWS Security Agent, AWS CLF-C02 Certification Roadmap, & Customer-Centric SLA Monitoring  

---

### PART I: DETAILED TOPIC PRESENTATIONS

#### 1. Topic 1: Securing Your Web Apps With AWS Security Agent
* **Speaker:** Thinh Nguyen (DevOps/DevSecOps/Cloud Engineer – Styl Solutions)
* **The Security Bottleneck:**
  * Manual penetration testing (pentesting) takes weeks, incurs high costs ($5,000 – $20,000/round), and yields inconsistent results depending on individual tester mindset and skill.
* **AWS Security Agent Solution (Frontier Agent):**
  * Automates reasoning powered by **Amazon Bedrock** to plan and execute security reviews.
  * **Full Lifecycle Coverage:**
    * **Design Security Review:** Evaluates architecture documents (Markdown/Terraform) against PCI DSS, NIST CSF, and AWS Well-Architected frameworks (Free Tier: 200 tasks/month).
    * **Code Security Review:** Scans Pull Requests (PRs) on GitHub/GitLab, comments directly on code lines, and suggests automated fixes (Free Tier: 1,000 PRs/month).
    * **Automated Pentesting:** Simulates real-world exploit chains (e.g., IDOR => XSS) and validates vulnerabilities.
* **Pricing Reality:**
  * 2-month free trial (400 task-hours/month); pay-as-you-go rate at $50/task-hour.
  * In real projects, total pentesting cost ranges from **$1,500 – $2,500**, offering massive cost savings compared to traditional $10,000 manual pentesting engagements.
* **Limitations to Note:**
  * Blocked by advanced authentication mechanisms (MFA, Biometrics, mTLS).
  * Struggles with complex business logic flaws without deep context.
  * Requires strict task-hour tracking to prevent unexpected cost overruns.

---

#### 2. Topic 2: Inside The Exam: AWS Cloud Practitioner (CLF-C02)
* **Speaker:** Ngo Le Tan Huy
* **Exam Overview:**
  * Foundational-level exam suitable for beginners, focusing on high-level cloud concepts without requiring deep coding or technical configurations.
  * **Format:** 65 multiple-choice questions, 90-minute duration (+30 minutes ESL accommodation), passing score of 700/1000, 3-year validity.
* **4 Content Domains:**
  * **Cloud Concepts (24%):** Digital transformation mindset, 6 Cloud benefits, AWS WAF, AWS CAF.
  * **Security and Compliance (30%):** Shared Responsibility Model, IAM, Security Groups, NACLs, AWS Shield, AWS WAF, AWS Artifact.
  * **Cloud Technology and Services (34%):** Global Infrastructure (Regions, AZs, Edge Locations) and core services for Compute (EC2, Lambda), Storage/DB (S3, EBS, RDS, DynamoDB), Networking (VPC, Route 53).
  * **Billing, Pricing, and Support (12%):** EC2 Pricing Models, AWS Cost Explorer, AWS Budgets, Support Plans.
* **Study Insights & Exam Tips:**
  * **Strategy:** Learn via *Keyword Thinking*, thoroughly analyze mock test explanations, and gain hands-on experience using the AWS Free Tier.
  * **Exam Tactics:** Use elimination (2 options are usually distractors), keep solutions simple, watch for trap keywords (`NOT`, `Least cost`), and use *Flag for review* to revisit tough questions.

---

#### 3. Topic 3: SLA and Monitoring: From SLA to Monitoring What Really Matters
* **Speaker:** Nguyen Huynh Son (Ex-Infrastructure Reliability Engineer)
* **Role of Service Level Agreements (SLA):**
  * Sets clear service expectations with customers, assigns operational accountability, and manages system risk.
* **Monitoring Reality:**
  * **Healthy infrastructure $\neq$ Happy users.**
  * *Real-world example:* `/health` returns 200 OK and EC2 CPU runs at a cool 18%, but RDS database connection issues prevent users from logging in (`/login` fails).
* **The Monitoring Pyramid:**
  * Monitor top-down rather than focusing solely on lower-level infrastructure:
    1. **Customer Experience:** Can users log in / complete purchases?
    2. **Business Metrics:** Login success rate, order count, revenue.
    3. **Application:** Latency spikes, error rates.
    4. **Infrastructure:** CPU, Memory, Disk, Network.
    5. **Cloud Provider:** AWS service status (EC2, RDS, ALB).
* **Core Message & Operational Philosophy:**
  * Monitor what users actually do (Login, Checkout, Payment), rather than relying only on server metrics.
  * **Shared Responsibility SLA Model:** AWS ensures Cloud infrastructure availability, while you remain accountable for Customer Experience.
  * Philosophy from Dr. Werner Vogels (CTO Amazon): *"Everything fails all the time, so plan for failure and nothing fails"*.

---

### PART II: PERSONAL TAKEAWAYS & IMPRESSIONS AS AN ATTENDEE

As an audience member attending the event presentations, I gained practical operational insights and valuable lessons:

#### 1. Security & DevSecOps Automation Takeaways
* Understood how Generative AI tools (AWS Security Agent) automate architecture and code reviews, lowering traditional pentesting costs while upholding security standards.

#### 2. Certification Preparation Strategy
* Mastered the CLF-C02 exam structure, keyword thinking approach, and test-taking strategies to build a solid foundation for acquiring the AWS Cloud Practitioner certification.

#### 3. Customer-Centric Monitoring Mindset
* Shifted from pure infrastructure monitoring to **user-experience-centric monitoring**, realizing that green infrastructure metrics are only meaningful when core user journeys (Login, Payment) function seamlessly.
#### 4. Event Photos
![FCAJ Community Day](/images/event3.png)

