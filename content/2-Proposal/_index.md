---
title: "Proposal"
date: 2026-07-27
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# MLOps Platform for Telco Customer Churn Prediction
## An automated MLOps system for training, evaluating, and deploying telecommunications churn prediction models on AWS

### 1. Executive Summary  
The Telco Customer Churn MLOps Platform project is designed to build an End-to-End MLOps Pipeline that automates the entire lifecycle of a Machine Learning model. The platform handles everything from data preprocessing, model training/hyperparameter optimization (HPO), and model quality evaluation, to registering models in the Model Registry and automatically deploying them to a Serverless Endpoint once approved. The entire real-time API interface is protected and optimized using Amazon CloudFront combined with the AWS WAF firewall, enabling telecom enterprises to proactively identify customers at risk of churning in a secure, robust, and cost-optimized manner.

### 2. Problem Statement  
#### Current Issues  
Traditional churn prediction models are typically developed manually in local environments (Local Notebooks), leading to Model Drift as production data evolves over time. Manual deployment from Notebooks to Production is time-consuming, prone to operational errors, and lacks an automated retraining mechanism when model performance degrades. Furthermore, exposing public APIs directly to the Internet without protective firewall layers leaves the system vulnerable to DoS/DDoS attacks or unauthorized exploitation.

#### Solution  
Build an MLOps platform on AWS SageMaker Workflow (Pipelines) combined with Event-Driven Automation architecture. When an Administrator uploads new data to Amazon S3, an AWS Lambda function checks for Data Drift and sends notifications via SNS. If retraining is required, the SageMaker Pipeline executes a 4-step workflow (Processing, HPO, Evaluation, Condition Check with AUC $\ge$ 0.80). Once the model meets the criteria and reaches the Approved state in the SageMaker Model Registry, Amazon EventBridge triggers the Lambda Deployer to automatically update the Serverless Endpoint without service interruption (Zero-downtime Deployment). The system uses **Amazon CloudFront** as an Edge Location to optimize API latency and integrates **AWS WAF** with Rate Limiting rules to mitigate malicious traffic.

#### Benefits & Return on Investment (ROI)  
- **Technical:** Reduces time-to-market from training to deployment from days to hours. Enhances security and reduces API latency for clients via CloudFront & AWS WAF. The Serverless Endpoint architecture optimizes infrastructure costs by billing only during active inference requests.  
- **Business:** Early detection of churning customers helps retain revenue streams while ensuring high system availability against potential cyber threats.
- **Estimated Cost:** ~$2.57 - $4.00 USD/month for pipeline retraining runs and serverless inference (including baseline WAF protection).

### 3. Solution Architecture  

![Architecture](/images/2-Proposal/architecture.png)

#### AWS Services Used  
- **Amazon S3:** Stores raw data, processed datasets, and Model Artifacts.
- **Amazon CloudFront & AWS WAF:** Acts as a public Edge Location CDN to accelerate API delivery and apply Web Application Firewall rules (Rate Limiting) to protect against DDoS/Layer 7 attacks.
- **Amazon API Gateway & AWS Lambda (Inference):** Receives prediction requests from CloudFront via REST API and executes real-time data preprocessing.
- **AWS SageMaker Serverless Endpoint:** Provides a real-time inference API hosting the XGBoost model, auto-scaling seamlessly based on incoming traffic.
- **AWS Lambda (Drift Checker & Trigger):** Inspects Data Drift upon new data uploads to S3 and triggers the SageMaker Pipeline.
- **AWS SageMaker Pipelines:** Orchestrates the 4-step MLOps workflow (Processing, Tuning, Evaluation, Condition & Register).
- **AWS SageMaker Model Registry:** Manages model versions and approval statuses.
- **Amazon EventBridge & AWS Lambda (Deployer):** Listens for Model Approved events to automatically update the Serverless Endpoint.  
- **Amazon CloudWatch & Amazon SNS:** Stores logs, sets alarm thresholds for API/Endpoint errors, and sends automated email notifications to Gmail.

### 4. Technical Implementation  
#### Implementation Phases  

1. **Data Exploration & Baseline Training:** Analyze the Telco Customer Churn dataset. Preprocess data using SKLearnProcessor. Train a single baseline XGBoost model and evaluate its AUC metric.  
2. **Pipeline Automation & MLOps Workflow:** Configure Hyperparameter Tuning Jobs for XGBoost. Write an evaluation script that outputs an evaluation.json file containing AUC metrics. Build a complete SageMaker Pipeline with a ConditionStep (registers the model only if AUC >= 0.80).    
3. **Event-Driven Automated Deployment & Public API Security:** Develop the AWS Lambda Deployer to handle flexible Endpoint updates. Configure EventBridge Rules to capture events from the Model Registry. Set up an Amazon CloudFront Distribution pointing to API Gateway and enable AWS WAF (Rate Limiting set to 100 requests per 5 minutes). Integrate CloudWatch Alarms and SNS Email Alerts.

### 5. Roadmap & Key Milestones  
- Week 4: The team explores the MLOps concept, studies SageMaker components, and analyzes the Telco Churn dataset.  
- Week 5: Design the full solution architecture, draw the architecture diagram, and write the Proposal.  
- Week 6: Build the complete SageMaker Pipeline (Processing, HPO, Evaluation, ConditionStep AUC >= 0.80, Register).  
- Week 7: Implement EventBridge + Lambda Auto-Deploy, build the Inference API, and perform End-to-End testing.  
- Week 8: Clean up AWS resources, write technical blogs, finalize the internship report.   


### 6. Budget Estimation  
- **AWS Lambda & Amazon EventBridge:** $0.00 USD/month (Covered by AWS Free Tier).  
- **Amazon S3:** ~$0.12/month (~5 GB storage including artifacts & datasets).  
- **AWS SageMaker Processing & Training:** ~$0.35/month (using ml.m5.large instances).  
- **AWS SageMaker Hyperparameter Tuning:** ~$0.80/month (6 parallel tuning jobs on ml.m5.large).  
- **AWS SageMaker Serverless Endpoint:** ~$1.20/month (2048 MB Memory, ~10,000 requests/month).  
- **Amazon CloudFront & AWS WAF:** Costs depend on actual traffic (Free Tier covers 1TB CloudFront data transfer; short-term testing WAF costs ~$0.00 - $15.00/month depending on scope).  
- **Amazon CloudWatch & SNS:** ~$0.10/month.  

**Total:** ~$2.57 - $4.00 USD/month (Baseline operational cost excluding large-scale WAF scaling)

### 7. Risk Assessment  
#### Risk Matrix  
- Model Performance Drift: High impact, medium probability.  
- Distributed Denial of Service (DDoS) or spam requests overloading the API: High impact, medium probability.  
- Uncontrolled resource cost spikes: Medium impact, low probability.  

#### Mitigation Strategies  
- Model Performance: Enforce a condition check step (ConditionStep) within the Pipeline. If a new model yields AUC < 0.80, the Pipeline triggers a FailStep and immediately halts registration into the Model Registry.  
- API Security: Deploy AWS WAF with Rate Limiting (blocking IPs exceeding 100 requests/5 minutes) behind CloudFront to protect the backend service from malicious or spam requests.  
- Cost Control: Utilize Serverless Endpoints (incurring costs strictly per invocation with zero idle fees). Set up AWS Budgets Alarms to alert when monthly spend exceeds $10.00 USD.  

### 8. Expected Outcomes  
- Technical: Successfully deploy a 100% automated, closed-loop MLOps pipeline: Data Upload => Drift Check => Pipeline => Model Registry => Auto-Deploy => Serverless Endpoint => CloudFront (WAF).
- Operational & Security: Reduce manual operational effort for Data/MLOps Engineers by 95% during new model releases, while guaranteeing that the real-time API remains secure, reliable, and performant for client requests.