---
title: "Week 2 Worklog"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

* Explore and clean data (EDA) and build an experimental baseline model (XGBoost Baseline Model) for the Telco Customer Churn problem.
* Write a standardized data preprocessing script (preprocessing.py) to prepare for integration into SageMaker Processing Step.
* Finalize a detailed Project Proposal on the MLOps Platform architecture for Mentor approval.

### Tasks to implement this week:

| Day | Task | Start Date | Completion Date | Reference Documentation |
| --- | --- | --- | --- | --- |
| Mon | - Download Telco Customer Churn dataset (WA_Fn-UseC_-Telco-Customer-Churn.csv) to local Jupyter Notebook/Colab <br> - Perform Exploratory Data Analysis (EDA): Analyze Churn target variable distribution, handle missing values in TotalCharges column, and encode categorical features | 22/06/2026 | 22/06/2026 | Pandas / Scikit-Learn Docs |
| Tue | - Experiment with training churn prediction model using XGBoost algorithm <br> - Evaluate baseline model performance using Machine Learning metrics: ROC-AUC, Accuracy, Precision, Recall <br> - Determine minimum AUC threshold for Quality Gate ($AUC \ge 0.80$) | 23/06/2026 | 23/06/2026 | XGBoost Documentation |
| Wed | - Package data preprocessing logic into an independent Python script preprocessing.py <br> - Handle data type casting, One-Hot Encoding, move target column Churn to the first column (following XGBoost input format standard), and split dataset into Train (70%), Validation (15%), Test (15%) | 24/06/2026 | 24/06/2026 | SageMaker Python SDK |
| Thu | - Draft Project Proposal document in both Vietnamese and English: <br>&emsp; + Executive Summary & Problem Statement <br>&emsp; + Event-Driven MLOps Solution Architecture on AWS <br>&emsp; + List of AWS services used (SageMaker, S3, Lambda, EventBridge, API Gateway, SNS, CloudWatch) <br>&emsp; + Budget Estimation & 8-Week Implementation Roadmap | 25/06/2026 | 25/06/2026 | AWS MLOps Framework |
| Fri | - Test running preprocessing.py script in local environment to ensure output CSV data is compatible with SageMaker <br> - Present project Proposal to Mentor, incorporate feedback, and update MLOps system architecture diagram | 26/06/2026 | 26/06/2026 | Mentor Feedback |

### Week 2 Accomplishments:

* Completed EDA data analysis for Telco Customer Churn problem: Successfully identified and handled 11 missing values in TotalCharges column, converted Churn variable from string (Yes/No) to binary format (1/0).
* Successfully trained Baseline XGBoost model on cleaned dataset, achieving **ROC-AUC ~0.84** (exceeding target threshold of 0.80).
* Built a complete Python script file preprocessing.py, supporting arguments from SageMaker's SKLearnProcessor to automatically split data into 3 sets: train.csv, validation.csv, and test.csv.
* Finalized and published Proposal file for **MLOps Platform for Telco Customer Churn Prediction** project on the internship reporting system, ready for the automated packaging stage on AWS Cloud.
