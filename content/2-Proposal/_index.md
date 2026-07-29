---
title: "Proposal"
date: 2026-07-27
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# MLOps Platform for Telco Customer Churn Prediction
## Automated MLOps system for training, evaluating, and deploying telco customer churn prediction models on AWS

### 1. Executive Summary  
The Telco Customer Churn MLOps Platform project is designed to build a closed End-to-End MLOps Pipeline automating the entire lifecycle of Machine Learning models. The platform handles data processing, training/hyperparameter optimization (HPO), model quality evaluation, registration into Model Registry, and automated deployment to a Serverless Endpoint as soon as a model is Approved. The system helps telecom enterprises proactively detect customers at risk of churning, thereby enabling timely retention policies at optimal operational costs.

### 2. Problem Statement  
*Current Issue*  
Traditional Churn prediction models are often developed manually in local environments (Local Notebooks), causing Model Drift when real-world data changes over time. Manual deployment workflows from Notebooks to Production take a long time, are prone to operational errors, and lack an automated retraining mechanism when model performance degrades.

*Solution*  
Build an MLOps system on the AWS SageMaker Workflow (Pipeline) platform combined with Event-Driven Automation architecture. When an Admin uploads new data to Amazon S3, AWS Lambda checks for Data Drift and sends notifications via SNS. If retraining is needed, SageMaker Pipeline runs a 4-step workflow (Processing, HPO, Evaluation, Condition Check with AUC >= 0.80). When the model qualifies and transitions to Approved status in SageMaker Model Registry, Amazon EventBridge triggers Lambda Deployer to automatically update the Serverless Endpoint without service interruption (Zero-downtime Deployment).

*Benefits & Return on Investment (ROI)*  
- **Technical:** Reduces time from Training to Deployment from several days down to a few hours. The Serverless Endpoint mechanism optimizes infrastructure costs as it only charges per inference request.  
- **Business:** Early detection of churning customers, protecting enterprise revenue.  
- **Estimated Cost:** ~$2.5 - $4.0 USD/month for pipeline retrain runs and serverless inference.  

### 3. Solution Architecture  

![Architecture](/images/2-Proposal/architecture.png)

*AWS Services Used*  
- *Amazon S3*: Stores raw data, processed data, and Model Artifacts.  
- *Amazon API Gateway & AWS Lambda (Inference)*: Receives prediction requests from Clients via REST API, performing real-time data preprocessing.  
- *AWS SageMaker Serverless Endpoint*: Provides real-time inference API with XGBoost model, auto-scaling based on traffic.  
- *AWS Lambda (Drift Checker & Trigger)*: Checks Data Drift when new data lands on S3 and triggers SageMaker Pipeline.  
- *AWS SageMaker Pipelines*: Orchestrates 4-step MLOps workflow (Processing, Tuning, Evaluation, Condition & Register).  
- *AWS SageMaker Model Registry*: Manages model versions and approval statuses.  
- *Amazon EventBridge & AWS Lambda (Deployer)*: Listens for Model Approved events to automatically update Serverless Endpoint.  
- *Amazon CloudWatch & Amazon SNS*: Stores Logs, sets Alarms for API/Endpoint errors, and sends automated Email notifications to Gmail.  

### 4. Technical Implementation  
*Implementation Phases*  

1. *Data Exploration & Trial Training*: Analyze Telco Customer Churn dataset. Preprocess data using SKLearnProcessor. Trial train standalone XGBoost model and evaluate AUC metric.  
2. *Pipeline Automation & MLOps Workflow*: Configure Hyperparameter Tuning Job for XGBoost. Write evaluation script exporting evaluation.json file containing AUC metric. Build complete SageMaker Pipeline with ConditionStep (only register model if AUC $\ge$ 0.80).  
3. *Event-Driven Automated Deployment*: Program AWS Lambda Deployer handling flexible Endpoint updates. Configure EventBridge Rule capturing events from Model Registry. Integrate CloudWatch Alarm and SNS Email Alert.  

### 5. Roadmap & Implementation Milestones  
- Week 1 – Week 2: Survey problem context, process Telco Churn data, and build XGBoost Baseline Model.  
- Week 3 – Week 4: Package data processing and training workflow into SageMaker Pipelines.  
- Week 5 – Week 6: Automate model evaluation phase and integrate SageMaker Model Registry.  
- Week 7: Set up EventBridge, Lambda Function to realize Auto-Deploy feature to Serverless Endpoint.  
- Week 8: End-to-End system testing, cost optimization, Endpoint latency evaluation, and report finalization.  

### 6. Budget Estimation  
- AWS Lambda & Amazon EventBridge: $0.00 USD/month (Within Free Tier).  
- Amazon S3: ~$0.12/month (~5 GB including Artifacts & Data).  
- AWS SageMaker Processing & Training: ~$0.35/month (ml.m5.large instance).  
- AWS SageMaker Hyperparameter Tuning: ~$0.80/month (6 parallel Tuning Jobs on ml.m5.large).  
- AWS SageMaker Serverless Endpoint: ~$1.20/month (2048 MB Memory, ~10,000 requests/month).  
- Amazon CloudWatch & SNS: ~$0.10/month.  

*Total*: ~$2.57 - $4.00 USD/month  

### 7. Risk Assessment  
*Risk Matrix*  
- Model Performance Drift: High impact, medium probability.  
- Uncontrolled resource cost occurrence: Medium impact, low probability.  

*Mitigation Strategies*  
- Model performance: Place condition check step (ConditionStep) in Pipeline. If new model AUC $< 0.80$, Pipeline triggers FailStep and immediately stops registration to Registry.  
- Cost control: Use Serverless Endpoint (only incurs charges during invocations, zero idle waiting fee). Set AWS Budgets Alarm warning when cost exceeds $10 USD/month.  

### 8. Expected Outcomes  
- Technical: Successfully deploy 100% automated closed MLOps workflow: Data Upload -> Drift Check -> Pipeline -> Model Registry -> Auto-Deploy -> Serverless Endpoint.  
- Operational: Reduce manual effort by 95% for Data/MLOps Engineers when deploying new model versions.  