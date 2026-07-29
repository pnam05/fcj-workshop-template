---
title: "Workshop"
date: 2026-07-29
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Tự động hóa Quy trình MLOps và Triển khai Mô hình Dự đoán Rời bỏ Dịch vụ Viễn thông trên AWS

#### Tổng quan

Trong môi trường doanh nghiệp thực tế, các mô hình Machine Learning thường gặp phải hiện tượng **Data Drift** (suy giảm chất lượng dự đoán theo thời gian) và tốn nhiều công sức để vận hành, cập nhật thủ công. 

Bài Workshop này sẽ hướng dẫn bạn từng bước xây dựng một **Hệ thống MLOps Tự động hóa Khép kín (End-to-End Automated MLOps Platform)** trên nền tảng AWS Cloud dành cho bài toán Telco Customer Churn Prediction. 

Hệ thống kết hợp sức mạnh của kiến trúc Event-Driven Automation và AWS Serverless Services:
+ **Tự động kích hoạt (Automated Retrain Trigger):** Kiểm tra Data Drift và khởi chạy SageMaker Pipeline ngay khi Admin tải dữ liệu mới lên Amazon S3.
+ **Quy trình MLOps chuẩn 4 bước (SageMaker Pipeline):** Tự động hóa từ khâu Tiền xử lý dữ liệu (SKLearnProcessor), Huấn luyện & Tối ưu siêu tham số (HyperparameterTuner), Đánh giá chất lượng mô hình (ScriptProcessor), đến kiểm tra Quality Gate ($AUC \ge 0.80$).
+ **Triển khai tự động (Continuous Deployment - CD):** Sử dụng Amazon EventBridge để lắng nghe sự kiện gán nhãn Approved trong Model Registry, kích hoạt AWS Lambda tự động tạo cấu hình và cập nhật lên **SageMaker Serverless Endpoint** mà không gây gián đoạn dịch vụ (Zero-Downtime Deployment).
+ **Dự đoán thời gian thực (Real-time Inference API):** Tích hợp Amazon API Gateway (HTTP API) và Lambda Inference Handler để tiếp nhận request HTTPS và trả về xác suất Churn tức thì.
+ **Giám sát & Báo động (Monitoring & Alerts):** Quản lý Log tập trung qua CloudWatch Logs, thiết lập CloudWatch Alarms và bắn Email thông báo tự động qua Amazon SNS.

#### Nội dung chi tiết Workshop

1. [1. Tổng quan (Workshop Overview)](5.1-Workshop-overview/)
2. [2. Các bước chuẩn bị (Prerequisites)](5.2-Prerequiste/)
3. [3. Triển khai Kỹ thuật Chi tiết (Step-by-Step Implementation)](5.3-Implementation/)
4. [4. Kiểm thử Toàn bộ Hệ thống (Test & Validation)](5.4-Test-Validation/)
5. [5. Dọn dẹp tài nguyên (Clean-up)](5.5-Cleanup/)