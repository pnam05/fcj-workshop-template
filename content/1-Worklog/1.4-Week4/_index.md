---
title: "Worklog Week 4"
date: 2026-07-06
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

* Deepen AWS Cloud knowledge in Machine Learning Managed Services ecosystem: Learn container image management mechanisms on ECR, SageMaker Estimators, Hyperparameter Tuning Jobs, and integration with Amazon CloudWatch Logs for debugging jobs.
* Build and package Automated Training & Hyperparameter Optimization step (`TelcoChurnHpoStep`) using `HyperparameterTuner` with XGBoost algorithm.
* Write model evaluation script `evaluate.py` and package Performance Evaluation step (`TelcoChurnEvalStep`), extracting ROC-AUC metric into `evaluation.json`.

### Tasks to implement this week:

| Day | Task | Start Date | Completion Date | Reference Documentation |
| --- | --- | --- | --- | --- |
| Mon | - Advanced learning on Machine Learning & Analytics services on AWS: <br>&emsp; + SageMaker Training Jobs architecture & pulling built-in containers from Amazon ECR mechanism <br>&emsp; + Hyperparameter Optimization strategies (Random, Bayesian, Hyperband Search) <br>&emsp; + Centralized Log Management for Training Jobs with Amazon CloudWatch Logs <br> - **Hands-on:** Check XGBoost v1.5-1 ECR Image URI in region `ap-southeast-1` | 06/07/2026 | 06/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - Initialize `Estimator` object for XGBoost with fixed hyperparameters (`objective='binary:logistic'`, `eval_metric='auc'`) <br> - Configure hyperparameter search space ranges (`HyperparameterRanges`): `max_depth` (3-10), `eta` (0.01-0.2), `min_child_weight` (1-6) <br> - Package into `HyperparameterTuner` (running 6 total jobs, 2 parallel jobs on `ml.m5.large`) and create `TuningStep` (`TelcoChurnHpoStep`) | 07/07/2026 | 07/07/2026 | SageMaker SDK Documentation |
| Wed | - Run trial independent HPO Job to verify data reading capability from `s3://.../processed/train` and `s3://.../processed/validation` <br> - Monitor Tuning Jobs progress and read `validation:auc` metrics directly on SageMaker Console & CloudWatch Logs | 08/07/2026 | 08/07/2026 | AWS SageMaker Console |
| Thu | - Program Python script file `evaluate.py`: <br>&emsp; + Automatically extract the best `model.tar.gz` file from HPO Job <br>&emsp; + Download test dataset from `s3://.../processed/test/test.csv` <br>&emsp; + Predict Churn probabilities and compute ROC-AUC Score <br>&emsp; + Package AUC metric into standard JSON format (`evaluation.json`) | 09/07/2026 | 09/07/2026 | Scikit-Learn / XGBoost Docs |
| Fri | - Create `ScriptProcessor` object packaging `evaluate.py` into `TelcoChurnEvalStep` <br> - Set up `PropertyFile` (`evaluation.json`) to extract AUC metric serving the condition check step (Condition Gate) for next week <br> - Successfully test sequential execution flow: `ProcessingStep` $\rightarrow$ `TuningStep` $\rightarrow$ `EvalStep` | 10/07/2026 | 10/07/2026 | AWS Hands-on Labs |

### Week 4 Accomplishments:

* Mastered operating mechanisms of SageMaker Training/HPO Jobs, understood how SageMaker automatically pulls Docker Containers from Amazon ECR and pushes logs to CloudWatch Logs.
* Successfully built `TelcoChurnHpoStep`:
  * Automatically executed 6 hyperparameter optimization training jobs in parallel.
  * Found optimal XGBoost hyperparameter set for Telco Churn dataset, exporting `model.tar.gz` model file stored securely at `s3://telco-churn-mlops-fcaj/models/`.
* Completed building `evaluate.py` script and packaged into `TelcoChurnEvalStep` using `ScriptProcessor`.
* Successfully extracted `evaluation.json` file containing Test AUC value (~0.84), ready as input for `ConditionStep` for automated model evaluation.
