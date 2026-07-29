---
title : "Thực hành Kỹ thuật"
date : 2026-07-29
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Thực hành Kỹ thuật: Triển khai Hệ thống MLOps trên AWS

Phần này hướng dẫn từng bước xây dựng, kết nối và kiểm thử toàn bộ các thành phần của hệ thống **MLOps Platform for Telco Customer Churn Prediction** trên môi trường AWS Console và Jupyter Notebook.


#### Mục tiêu chính của phần Workshop
Sau khi hoàn thành chuỗi bài lab trong mục này, bạn sẽ tự tay triển khai thành công một hệ thống MLOps tự động hóa:
- Khởi tạo Data Lake lưu trữ dữ liệu thô và mô hình trên Amazon S3.
- Xây dựng MLOps Pipeline 4 bước bằng AWS SageMaker Pipelines (Processing, HPO, Evaluation, Condition & Model Registry).
- Cấu hình Event-Driven Auto-Deployment sử dụng Amazon EventBridge và AWS Lambda để tự động cập nhật Serverless Endpoint khi mô hình đạt chuẩn.
- Xây dựng Real-time Inference API với Amazon API Gateway (HTTP API) và AWS Lambda.
- Giám sát & Cảnh báo tự động thông qua Amazon CloudWatch Logs/Alarms và Amazon SNS.

