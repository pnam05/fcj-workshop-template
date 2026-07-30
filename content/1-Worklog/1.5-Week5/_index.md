---
title: "Week 5 Worklog"
date: 2026-07-06
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Collaborate with team members to design MLOps system architecture and finalize the Proposal document.
* Individual learning on Model Governance & Versioning on AWS Cloud with SageMaker Model Registry.
* Learn Quality Gate concepts (Condition Steps) and exception handling mechanics in Cloud automation pipelines.

### Tasks to implement this week:

| Day | Task                                                                                                                                                                                                                                                                      | Start Date | Completion Date | Reference Documentation                                                              |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------------------ |
| Mon | - Advanced study of Model Governance & Versioning on AWS: <br>&emsp; + Role of SageMaker Model Registry in Production environments <br>&emsp; + Managing Model Package Groups, Model Versions, and approval states (PendingManualApproval, Approved, Rejected)            | 06/07/2026 | 06/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html>                |
| Tue | - Study Model Lineage & Governance concepts on Cloud: <br>&emsp; + Automated model quality control, centralized model metadata storage <br>&emsp; + Tracking source data lineage connected directly to each model version                                                 | 07/07/2026 | 07/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/governance.html>                    |
| Wed | Design full MLOps Pipeline architecture with team - S3 => 4-step Pipeline => Model Registry => EventBridge => Lambda Deployer => Serverless Endpoint <br> - Write Proposal with team - Problem Statement, Solution Architecture, Timeline                                 | 08/07/2026 | 08/07/2026      |                                                                                      |
| Thu | - Learn syntax for condition evaluation step (ConditionStep) setup on Cloud: <br>&emsp; + Reading metric values from JSON output files using JsonGet <br>&emsp; + Setting ConditionGreaterThanOrEqualTo comparison condition for Quality Gate checks                      | 09/07/2026 | 09/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/build-and-manage-propertyfile.html> |
| Fri | - Research automated workflow termination and error handling (FailStep): <br>&emsp; + Managing else_steps error branch when model fails Quality Gate threshold <br>&emsp; + Configuring error notifications and protecting Production environment from low-quality models | 10/07/2026 | 10/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines-sdk.html>                 |

### Week 5 Achievements:
* Designed full MLOps Pipeline architecture with team and wrote comprehensive Proposal covering Problem Statement, Architecture Diagram, Timeline, Budget, and Risk Assessment.
* Mastered MLOps Governance concepts on AWS Cloud, understanding centralized model version management with SageMaker Model Registry.
* Understood ConditionStep setup mechanics and JsonGet comparison operators for automated control gates.
* Mastered conditional execution branching and FailStep triggering mechanics when models fail threshold evaluation.
