---
title: "Workshop"
date: 2026-07-29
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Automating MLOps Workflow and Deploying Telco Customer Churn Prediction Models on AWS

#### Overview

In real-world enterprise environments, Machine Learning models often face **Data Drift** (degradation of prediction quality over time) and require significant manual effort to operate and update.

This Workshop will guide you step-by-step to build a complete **End-to-End Automated MLOps Platform** on AWS Cloud for the Telco Customer Churn Prediction problem.

The system combines the power of Event-Driven Automation architecture and AWS Serverless Services:
+ **Automated Retrain Trigger:** Checks Data Drift and launches SageMaker Pipeline as soon as an Admin uploads new data to Amazon S3.
+ **Standard 4-step MLOps Workflow (SageMaker Pipeline):** Automates from Data Preprocessing (SKLearnProcessor), Training & Hyperparameter Optimization (HyperparameterTuner), Model Quality Evaluation (ScriptProcessor), to Quality Gate checking ($AUC \ge 0.80$).
+ **Automated Deployment (Continuous Deployment - CD):** Uses Amazon EventBridge to listen for Approved status events in Model Registry, triggering AWS Lambda to automatically create configurations and update to **SageMaker Serverless Endpoint** with Zero-Downtime Deployment.
+ **Real-time Inference API:** Integrates Amazon API Gateway (HTTP API) and Lambda Inference Handler to receive HTTPS requests and return immediate Churn probabilities.
+ **Monitoring & Alerts:** Centralized Log management via CloudWatch Logs, establishing CloudWatch Alarms, and sending automated Email notifications via Amazon SNS.

#### Detailed Workshop Contents

1. [1. Overview (Workshop Overview)](5.1-Workshop-overview/)
2. [2. Prerequisites](5.2-Prerequiste/)
3. [3. Step-by-Step Implementation](5.3-Implementation/)
4. [4. Test & Validation](5.4-Test-Validation/)
5. [5. Resource Clean-up](5.5-Cleanup/)