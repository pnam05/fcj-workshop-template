---
title: "Overview"
date: 2026-07-27
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

# Workshop Overview: End-to-End MLOps Platform for Telco Customer Churn Prediction

## Problem Statement
In the telecommunications industry, acquiring a new customer is 5 to 25 times more expensive than retaining an existing one. Predicting customer churn early enables customer retention teams to proactively offer personalized promotions and timely support.

However, real-world Machine Learning models frequently suffer from **Data Drift and Model Drift** — prediction performance degrades over time as customer behavior changes. Furthermore, manually training and deploying models from local Jupyter Notebooks to Production is time-consuming and prone to operational errors.

This workshop guides you through building an **End-to-End Automated MLOps Platform** on **AWS Cloud** to completely automate and resolve these operational challenges.

---

## Workshop Objectives
By completing this workshop, you will learn how to build and deploy:
1. **Real-time Inference API:** Integrate **Amazon API Gateway**, **AWS Lambda**, and **AWS SageMaker Serverless Endpoint** for real-time predictions with zero idle costs.
2. **Event-Driven Retrain Trigger:** Automatically detect new dataset uploads on **Amazon S3**, check for **Data Drift**, and trigger the pipeline.
3. **4-Step SageMaker Pipeline:**
   - `TelcoChurnProcessStep`: Data preprocessing and splitting using `SKLearnProcessor`.
   - `TelcoChurnHpoStep`: Automated XGBoost hyperparameter tuning via `HyperparameterTuner`.
   - `TelcoChurnEvalStep`: Model evaluation on the test set using `ScriptProcessor`.
   - `ConditionStep`: Quality gate evaluation ($AUC \ge 0.80$). Automatically registers passing models to **SageMaker Model Registry** as `Approved`.
4. **Continuous Deployment (CD):** Utilize **Amazon EventBridge** to detect `Approved` model packages and trigger an **AWS Lambda Deployer** for zero-downtime endpoint updates.
5. **Monitoring & Alerting:** Centralized logging with **CloudWatch Logs**, metric tracking via **CloudWatch Alarms**, and automated email notifications through **Amazon SNS**.

---

## Architecture Diagram

![AWS MLOps Architecture Diagram](/images/1-Overview/mlops_architecture.png)

### AWS Services Utilized:
- **Amazon S3:** Stores raw data, processed datasets, and model artifacts .
- **Amazon API Gateway & AWS Lambda:** Handles HTTPS REST API requests and real-time data transformation.
- **AWS SageMaker Serverless Endpoint:** Hosts the XGBoost model with auto-scaling capabilities.
- **AWS SageMaker Pipelines:** Automates and orchestrates the 4-step ML workflow.
- **AWS SageMaker Model Registry:** Manages model versions and approval statuses.
- **Amazon EventBridge:** Listens for pipeline and model package state changes.
- **Amazon SNS:** Sends automated email alerts for pipeline results and drift notifications.
- **Amazon CloudWatch:** Provides centralized logging, operational metrics, and alarms.

---

## Estimated Time & Cost
- **Duration:** ~60 - 90 minutes.
- **Estimated Cost:** ~$0.50 - $1.00 USD (Most resources qualify under the AWS Free Tier if cleaned up properly).