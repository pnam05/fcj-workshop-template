---
title: "Week 4 Worklog"
date: 2026-06-29
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

* Collaborate with team members to explore MLOps direction and perform in-depth analysis of the Telco Customer Churn dataset.
* Individual learning on data preprocessing and encoding methods (One-Hot Encoding, Label Encoding) in Cloud Notebook environments.
* Individual learning on XGBoost algorithm on AWS SageMaker and classification model evaluation metrics (ROC-AUC, Precision, Recall).

### Tasks to implement this week:

| Day | Task                                                                                                                                                                                                                                                        | Start Date | Completion Date | Reference Documentation                                                                        |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ---------------------------------------------------------------------------------------------- |
| Mon | - Learn data preprocessing techniques and feature encoding (One-Hot Encoding, Label Encoding) in Cloud Notebooks <br> - Learn CSV data formatting requirements for SageMaker Built-in Algorithms                                                            | 29/06/2026 | 29/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/numpytensormulticlass.html>                   |
| Tue | - Research XGBoost Algorithm on SageMaker: <br>&emsp; + Analyze core training parameters: objective='binary:logistic', eval_metric='auc' <br>&emsp; + Understand hyperparameter tuning parameters max_depth, eta, min_child_weight, subsample               | 30/06/2026 | 30/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html>                                 |
| Wed | - Explore MLOps with team: End-to-End workflow, SageMaker components (Studio, Processing Jobs, Training Jobs, Pipelines) <br> - Analyze Telco Churn dataset with team: feature structures, statistical distribution, defining binary classification problem | 01/07/2026 | 01/07/2026      | [SageMaker Developer Guide](https://docs.aws.amazon.com/sagemaker/latest/dg/)                  |
| Thu | - Study dataset splitting principles (Train/Validation/Test Split) for binary classification on Cloud <br> - Study data compression and input formatting compatible with SageMaker Estimators                                                               | 02/07/2026 | 02/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-training.html>                            |
| Fri | - Learn binary classification performance evaluation: <br>&emsp; + ROC-AUC Score, Confusion Matrix, Accuracy, Precision, Recall metrics <br>&emsp; + Establish standard Quality Gate thresholds for Machine Learning models on Cloud                        | 03/07/2026 | 03/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-model-monitor-model-performance.html> |

### Week 4 Achievements:

* Understood End-to-End MLOps workflow with team: from data ingestion => preprocessing => training => evaluation => model registry => deployment => monitoring. Completed Telco Churn dataset analysis.

* Mastered data preprocessing & One-Hot Encoding techniques standardized for SageMaker.
* Mastered core training parameters and training methodologies for XGBoost on AWS.
* Understood binary classification performance evaluation principles via ROC-AUC score and Quality Gate setup.
