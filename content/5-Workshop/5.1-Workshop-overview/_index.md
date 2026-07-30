---
title: "Overview"
date: 2026-07-27
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Workshop Overview: MLOps Platform for Telco Customer Churn Prediction

## Problem Statement
In the telecommunications industry (Telco), acquiring a new customer typically costs 5 to 25 times more than retaining an existing one. Predicting early churn risks enables customer care teams to proactively launch promotional campaigns and timely support to retain subscribers.

However, real-world Machine Learning models frequently suffer from **Data Drift / Model Drift** — where prediction accuracy degrades over time due to changing consumer habits. Furthermore, manual training and deployment from Jupyter Notebooks to Production environments are time-consuming, error-prone, and often lack essential firewall protections for public-facing APIs.

This workshop walks you through building an **End-to-End Automated MLOps Platform** on **AWS Cloud**, fully secured and optimized using **Amazon CloudFront** and **AWS WAF**, to address these operational and infrastructure challenges.

---

## Workshop Objectives
Upon completing this lab, you will master and deploy:
1. **API Security & Acceleration (CloudFront & AWS WAF):** Configure Amazon CloudFront as a public Edge Location in front of API Gateway to accelerate API response times, combined with AWS WAF firewall rules (Rate Limiting) to mitigate malicious traffic and protect backend infrastructure.
2. **Real-time Inference:** Integrate Amazon API Gateway, AWS Lambda, and AWS SageMaker Serverless Endpoint to process requests and deliver instantaneous churn predictions with cost-efficient infrastructure ($0 when idle).
3. **Event-Driven Automation:** Automatically detect new dataset uploads to Amazon S3, evaluate Data Drift, and trigger retraining workflows.
4. **MLOps Workflow (4-Step SageMaker Pipeline):**
   - **TelcoChurnProcessStep**: Data preprocessing and Train/Validation/Test splitting (SKLearnProcessor).
   - **TelcoChurnHpoStep**: Automatic training & Hyperparameter Optimization using XGBoost (HyperparameterTuner).
   - **TelcoChurnEvalStep**: Model evaluation on the Test set (ScriptProcessor).
   - **ConditionStep**: Quality threshold evaluation ($AUC \ge 0.80$). If met, automatically registers the model in the SageMaker Model Registry under the Approved state.
5. **Continuous Deployment (CD):** Use Amazon EventBridge to capture Approved events from the Model Registry, triggering an AWS Lambda Deployer to update the Serverless Endpoint with Zero-Downtime Deployment.
6. **Monitoring & Alerting:** Centralized log storage via CloudWatch Logs, CloudWatch Alarms setup, and automated SNS email notifications.

---

## System Architecture Diagram

![AWS MLOps Architecture Diagram](/images/5-Workshop/5.1-Workshop-overview/architecture.png)

### AWS Services Used:
- **Amazon S3:** Stores raw data, processed datasets, and Model Artifacts.
- **Amazon CloudFront & AWS WAF:** Serves as a public Edge Location CDN to accelerate API response latency and enforce Web Application Firewall rules (Rate Limiting) against DoS/DDoS threats.
- **Amazon API Gateway & AWS Lambda:** Provides REST API endpoints and performs real-time request preprocessing.
- **AWS SageMaker Serverless Endpoint:** Hosts the XGBoost model in a serverless, auto-scaling configuration.
- **AWS SageMaker Pipelines:** Manages and orchestrates the automated 4-step ML pipeline.
- **AWS SageMaker Model Registry:** Centralized model versioning and approval management.
- **Amazon EventBridge:** Listens for state transition events from Pipelines and Model Registry.
- **Amazon SNS:** Sends automated email alerts for retraining status and error notifications.
- **Amazon CloudWatch:** System logging, metric tracking, and operational alarm dispatching.

---

## Estimated Time & Cost
- **Duration:** ~60 - 90 minutes.
- **Infrastructure Cost:** ~$0.50 - $1.00 USD (Provided you clean up resources according to the Clean-up step at the end of the lab, most services remain within the AWS Free Tier).