---
title : "Kiểm thử Toàn bộ Hệ thống"
date : 2026-07-29 
weight : 4 
chapter : false
pre : " <b> 5.4. </b> "
---

#### Tổng quan
Phần này hướng dẫn kiểm thử End-to-End toàn bộ hệ thống MLOps qua 2 kịch bản chính:

- Kiểm thử Luồng Huấn luyện & Deploy Tự động (Automated Retrain & CD Flow): Giả lập Admin upload file dữ liệu mới lên S3 để kích hoạt chuỗi sự kiện S3 -> Lambda Drift Checker -> SageMaker Pipeline -> Model Registry -> EventBridge -> Lambda Deployer -> Serverless Endpoint.

- Kiểm thử Luồng Dự đoán Thời gian thực (Real-time Inference Flow): Gửi HTTP POST Request từ Client qua CloudFront Distribution (hoặc AWS WAF) đến API Gateway để kiểm tra khả năng phân phối, xử lý và trả về kết quả Churn Prediction.





