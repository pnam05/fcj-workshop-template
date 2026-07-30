---
title: "Week 5 Worklog"
date: 2026-07-13
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Deepen AWS Cloud knowledge in Centralized Model Governance, SageMaker Model Registry, Quality Gate concepts (Condition Steps), and software lifecycle management mechanisms on Cloud.
* End-to-end connect all 4 steps into a closed MLOps workflow (SageMaker Pipeline): Processing $\rightarrow$ HPO $\rightarrow$ Evaluation $\rightarrow$ Condition Check.
* Automate the model quality evaluation step with condition $AUC \ge 0.80$, automatically registering qualifying models into Model Registry with Approved status.

### Tasks to implement this week:

| Day | Task | Start Date | Completion Date | Reference Documentation |
| --- | --- | --- | --- | --- |
| Mon | - Advanced learning on **Model Governance & Versioning** on AWS: <br>&emsp; + Learn the role of **SageMaker Model Registry** in Production environments <br>&emsp; + Manage Model Package Groups, model versions, and approval statuses (PendingManualApproval, Approved, Rejected) <br>&emsp; + Basic Event-Driven Architecture concepts when resource state changes on Cloud | 13/07/2026 | 13/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - Set up model quality condition check step (ConditionStep): <br>&emsp; + Read AUC value from evaluation.json file using JsonGet <br>&emsp; + Set comparison condition ConditionGreaterThanOrEqualTo with value $0.80$ <br>&emsp; + Configure if_steps branch (calls RegisterModel) and else_steps branch (calls FailStep) | 14/07/2026 | 14/07/2026 | SageMaker SDK Documentation |
| Wed | - Initialize RegisterModel object to automatically register the best model from HPO step into Model Package Group TelcoChurnModelGroup with initial status Approved <br> - Configure FailStep (TelcoChurnAUCFailStep) to immediately stop Pipeline and report an error if model fails to reach AUC threshold of $0.80$ | 15/07/2026 | 15/07/2026 | AWS MLOps Best Practices |
| Thu | - Combine and connect 4 steps into a complete Pipeline (TelcoChurnPipeline): <br>&emsp; 1. TelcoChurnProcessStep <br>&emsp; 2. TelcoChurnHpoStep <br>&emsp; 3. TelcoChurnEvalStep <br>&emsp; 4. TelcoChurnCheckAUCThreshold <br> - Push Pipeline definition to SageMaker service using pipeline.upsert() method and trigger test run via pipeline.start() | 16/07/2026 | 16/07/2026 | AWS Hands-on Labs |
| Fri | - Monitor execution graph of TelcoChurnPipeline on **SageMaker Studio / Pipelines Console** interface <br> - Inspect execution results: Confirm all 4 steps report green (Succeeded), ConditionStep branches to True, and new model automatically appears in **Model Registry** with status Approved | 17/07/2026 | 17/07/2026 | SageMaker Pipelines Console |

### Week 5 Accomplishments:

* Mastered MLOps Governance concepts on AWS Cloud, understood centralized model versioning management and automated quality control mechanisms prior to deployment.
* Successfully built and upserted a **complete 4-step SageMaker Pipeline** (TelcoChurnPipeline) operating fully automatically on AWS.
* Successfully integrated **Quality Gate (ConditionStep)**: Automatically blocked low-quality models ($AUC < 0.80$) and triggered FailStep.
* Automatically registered qualifying XGBoost model ($AUC \approx 0.84$) into **SageMaker Model Registry** (TelcoChurnModelGroup) with status **Approved**, ready as input for Continuous Deployment workflow in following weeks.
