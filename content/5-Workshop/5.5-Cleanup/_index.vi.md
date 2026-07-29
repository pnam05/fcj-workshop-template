---
title : "Dọn dẹp tài nguyên"
date : 2026-07-29
weight : 6
chapter : false
pre : " <b> 5.5. </b> "
---

Để tránh phát sinh chi phí không cần thiết trên tài khoản AWS sau khi hoàn tất kiểm thử bài workshop, hãy thực hiện dọn dẹp toàn bộ tài nguyên theo trình tự các bước dưới đây.

#### Xóa SageMaker Serverless Endpoint & Configurations
- Truy cập Amazon SageMaker Studio $\rightarrow$ Deployments $\rightarrow$ Endpoints: Chọn telco-churn-serverless-endpoint $\rightarrow$ Delete.
![clean-server](../../../static/images/5-Workshop/5.4-Test-Validation/clean-server.png)
- Chọn JumpStart / Models: Chọn TelcoChurnModelGroup $\rightarrow$ Delete.
![clean-group](../../../static/images/5-Workshop/5.4-Test-Validation/clean-group.png)

#### Xóa SageMaker Pipeline
- Trong SageMaker Console, mở mục Pipelines.
- Chọn TelcoChurnPipeline (hoặc tên Pipeline của bạn).
- Bấm Delete và xác nhận xóa Pipeline.
![clean-pipeline](../../../static/images/5-Workshop/5.4-Test-Validation/clean-pipeline.png)
