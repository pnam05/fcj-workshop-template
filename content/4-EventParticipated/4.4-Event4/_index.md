---
title: "Event 4"
date: 2026-07-25
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

# EVENT SUMMARY REPORT: AGENTIC AI BUILD WEEK (AABW) HACKATHON

**Event Title:** Agentic AI Build Week (AABW) Hackathon  
**Date:** July 25th 2026
**Location:** 26th & 36th Floor, Bitexco Financial Tower, Ho Chi Minh City  
**Main Theme:** Real-world AI/AWS Solution Showcase & Practical 24-Hour Hackathon Insights  

---

### PART I: PROJECT SHOWCASE & TECHNICAL SOLUTIONS

#### 1. Team Plan V – Project: Solution Architect Professional AI Native App
* **Team Members:** Pham Tien Thuan, Phat Huynh Hoang Long, Le Minh Nghia, Tran Dai Vi, Nguyen An.
* **Problem Statement:**
  * Solution Architects (SAs) spend extensive manual effort analyzing customer requirements from documents (BRD/PRD).
  * Designing architecture diagrams, writing Infrastructure as Code (IaC - Terraform), and estimating cloud costs are often started from scratch and heavily depend on individual experience.
* **Solution:**
  * Built an AI-Native application tailor-made for Solution Architects:
    * Analyzes natural language documents and extracts a Requirements Catalogue in minutes.
    * Automatically generates enterprise-compliant cloud architecture drafts (supporting Hybrid-Cloud).
    * Generates editable architecture diagrams (`Draw.io` and standard AWS icons).
    * Estimates AWS costs grouped by region (e.g., `ap-southeast-1`).
    * Interactive refinement via a Chatbot Sidebar.
* **Technical Architecture & Tech Stack:**
  * **AI & Retrieval Services:** Amazon Bedrock, Knowledge Base, Draw.io MCP, AWS Pricing MCP.
  * **AWS Infrastructure:** ECS Fargate (for Backend & Agent Services), Amazon EFS, S3 Buckets, PostgreSQL, Amazon CloudFront, Application Load Balancer (ALB), AWS Cognito, CloudWatch, ECR, Terraform.

---

#### 2. Team Dream AI Team – Project: Signal Scout
* **Team Members:** Le Tan Luc, Do Hoang Hieu, Trieu Quoc Hao, Nguyen Van Duy Khiem, Nguyen Cong Minh, Nguyen Tran Minh Quan.
* **Value Proposition:**
  * Helps strategy, risk management, and competitive analysis teams detect early signals of corporate strategic shifts (restructuring, operational/financial metric changes).
  * Aggregates scattered data into transparent reports with cited evidence, empowering executives to decide whether to *Maintain*, *Adapt*, or *Accelerate*.
* **Architecture & Cost Optimization:**
  * **Tech Stack:** Amazon Bedrock, AgentCore Runtime & Short-Term Memory, AWS Amplify, WAF, DynamoDB, AWS Lambda, API Gateway, Route53, S3 Intelligent-Tiering, alongside tools like Langfuse, Apify, and TinyFish.
  * **Cost Optimization:** Provided a flexible budget breakdown based on usage tier (ranging from ~$81/month at baseline to ~$359/month at high scale).

---

#### 3. Team 3KA – Project: S.H.E.P.H.E.R.D.
*(Smart Human-flow Evaluation, Prediction, Hazard Detection, Response, and Dispatch)*
* **Team Members:** Huynh An Khuong, Nguyen Quoc Huy, Ngo Quang Khoi, Hoang Le Thanh Duc, Dang Nguyen Phuoc Loc, Dang Truong Hung.
* **Background & Core Solution:**
  * Originally planned as a Capstone project, the team brought it to the Hackathon to build a working MVP under high time pressure.
  * Real-time monitoring of crowded venues and events to detect congestion early rather than reacting passively.
* **Key Features & System Architecture:**
  * Live camera stream video analytics: Person counting/tracking, crowd density estimation, queue length evaluation, congestion risk alert, and dispatch recommendations.
  * **Computer Vision:** YOLO + ByteTrack (detection & object tracking).
  * **AI & Platform:** Amazon SageMaker, Amazon Bedrock AgentCore + Strands Agent (Agentic AI Layer featuring *Autonomous Monitor* for auto-tracking and *Operator Copilot* for real-time data Q&A), React Dashboard.

---

### PART II: HACKATHON INSIGHTS & EXPERIENCE (HACKATHON JOURNEY)

Beyond technical architectures, participating teams shared valuable lessons from their intense 24-hour hackathon journey:

#### 1. Challenges & Technical Hurdles
* **Learning Curve & Time Constraints:** Several team members were new to AI and AWS services, facing the challenge of shipping a functional Minimum Viable Product (MVP) within 24 hours.
* **Technical Incidents:** Experiencing video processing lag, frame tracking loss, staying up late debugging until 3 AM, and accidental secrets commits (`.env` file pushed to GitHub).

#### 2. Memorable Experiences
* Experiencing the high-energy 24-hour overnight atmosphere, team brainstorming sessions, late-night walks to recharge, and strong team bonding.
* Networking with fellow engineers, judges, and experienced AWS Mentors.

#### 3. Essential Advice for First-Time Hackathon Participants
1. **Preparation:** Define a clear "Definition of Done", prepare environment templates/accounts in advance, and assign clear roles (coding, UI/UX design, pitching).
2. **Scope It Tiny:** Focus on executing **one core feature extremely well**. A small, polished, bug-free MVP always beats an ambitious but broken project.
3. **Leverage Mentors:** Proactively seek guidance and feedback from AWS Mentors throughout the competition.
4. **Just Sign Up:** Don't wait until you feel "ready". Stepping into the competition is already a huge milestone!

---

### PART III: KEY TAKEAWAYS

* **Stepping Out of Comfort Zone:** Participating in the competition is half the victory.
* **Practicality Matters:** A smooth, working product holds higher value than sheer concept size.
* **Community & Networking:** Hackathons build rapid problem-solving resilience and connect like-minded cloud and AI enthusiasts.

---

### PART IV: PERSONAL TAKEAWAYS & IMPRESSIONS AS AN ATTENDEE

Attending the **Agentic AI Build Week (AABW)** project showcase and hackathon experience-sharing session as an audience member provided me with fresh perspectives and valuable professional insights:

#### 1. Technical Perspectives & Cloud / AI Architecture Mindset
* **Practical Agentic AI Design:** By watching the project showcases from Team Plan V, Dream AI, and 3KA, I gained a much clearer picture of how Generative AI models (Amazon Bedrock, AgentCore) seamlessly integrate with AWS infrastructure (ECS Fargate, Lambda, SageMaker, DynamoDB) to solve real enterprise automation challenges.
* **Cost Strategy & Infrastructure Efficiency:** Learned how competing teams conducted detailed cost analyses and leveraged Serverless architectures, S3 Intelligent-Tiering, and DynamoDB on-demand to optimize cloud expenditures.
* **Computer Vision & Real-Time ML Pipelines:** Understood how YOLO + ByteTrack models can be orchestrated alongside Amazon SageMaker and Bedrock Agents for real-time video stream analytics and autonomous operator assistance.

#### 2. Key Insights & Personal Takeaways from Team Sharing
* **Importance of Scope Management:** Listening to the participants' journey reinforced the vital lesson of prioritizing a polished, fully functional **Minimum Viable Product (MVP)** focused on one core capability over an overly ambitious, incomplete concept.
* **Inspiration from Team Resilience:** Although I did not compete directly in the 24-hour contest, hearing authentic stories of how teams collaborated, solved overnight technical bugs, and persevered under time pressure inspired me greatly.
* **Community Networking & Growth:** The event offered a great opportunity to broaden my industry perspective, listen to expert guidance from AWS Mentors, and learn problem-solving mindsets from experienced cloud engineers.
#### 3. Event Photos
![FCAJ Community Day - AABW Showcase](/images/event4.png)
