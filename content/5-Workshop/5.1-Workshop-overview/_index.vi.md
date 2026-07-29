---
title: "Tổng quan (Overview)"
date: 2026-07-27
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Tổng quan Workshop: MLOps Platform cho bài toán Dự đoán Rời bỏ Dịch vụ Viễn thông (Telco Customer Churn)

## Giới thiệu bài toán
Trong ngành viễn thông (Telco), chi phí để tìm kiếm một khách hàng mới thường cao gấp 5 - 25 lần so với chi phí giữ chân một khách hàng hiện tại. Việc dự đoán sớm nguy cơ khách hàng rời bỏ dịch vụ (Churn) giúp bộ phận chăm sóc khách hàng chủ động đưa ra các chính sách khuyến mãi và hỗ trợ kịp thời.

Tuy nhiên, các mô hình Machine Learning thực tế thường gặp phải vấn đề **Data Drift / Model Drift** — chất lượng dự đoán suy giảm theo thời gian do thói quen người dùng thay đổi. Hơn nữa, việc huấn luyện và triển khai mô hình thủ công từ Jupyter Notebook lên môi trường Production tiêu tốn nhiều thời gian và dễ phát sinh lỗi vận hành.

Workshop này sẽ xây dựng một **Hệ thống MLOps Tự động hóa Khép kín (End-to-End Automated MLOps Platform)** trên nền tảng **AWS Cloud**, giúp giải quyết triệt để các thách thức trên.

---

## Mục tiêu Workshop
Sau khi hoàn thành bài lab này, bạn sẽ nắm vững và triển khai được:
1. **Dự đoán thời gian thực (Real-time Inference):** Tích hợp Amazon API Gateway, AWS Lambda và AWS SageMaker Serverless Endpoint để xử lý request và trả về xác suất rời bỏ dịch vụ tức thì với chi phí tối ưu (0đ khi không có lưu lượng).
2. **Kích hoạt tự động (Event-Driven Trigger):** Tự động phát hiện khi Admin tải dữ liệu mới lên Amazon S3, kiểm tra Data Drift và khởi chạy luồng Retrain.
3. **Luồng làm việc MLOps (SageMaker Pipeline - 4 bước):**
   - TelcoChurnProcessStep: Tiền xử lý dữ liệu và chia tập Train/Validation/Test (SKLearnProcessor).
   - TelcoChurnHpoStep: Huấn luyện & Tối ưu siêu tham số tự động với mô hình XGBoost (HyperparameterTuner).
   - TelcoChurnEvalStep: Đánh giá chất lượng mô hình trên tập Test (ScriptProcessor).
   - ConditionStep: Kiểm tra ngưỡng chất lượng ($AUC \ge 0.80$). Nếu đạt, tự động đăng ký vào SageMaker Model Registry ở trạng thái Approved.
4. **Triển khai tự động (Continuous Deployment - CD):** Sử dụng Amazon EventBridge lắng nghe trạng thái Approved từ Model Registry để kích hoạt AWS Lambda Deployer tự động cập nhật Serverless Endpoint mà không gây gián đoạn dịch vụ (Zero-Downtime Deployment).
5. **Giám sát & Báo động (Monitoring & Alerting):** Lưu trữ log tập trung qua CloudWatch Logs, thiết lập CloudWatch Alarm và gửi email cảnh báo tự động về hòm thư qua Amazon SNS.

---

##  Sơ đồ Kiến trúc Hệ thống (Architecture Diagram)

![Sơ đồ Kiến trúc MLOps AWS](../../../static/images/5-Workshop/5.1-Workshop-overview/architecture.png)

### Các Dịch vụ AWS Sử Dụng:
- **Amazon S3:** Lưu trữ Dữ liệu thô, Dữ liệu đã xử lý  và Model Artifacts.
- **Amazon API Gateway & AWS Lambda:** Cung cấp điểm cuối REST API và xử lý tiền xử lý dữ liệu request thời gian thực.
- **AWS SageMaker Serverless Endpoint:** Triển khai mô hình XGBoost ở dạng Serverless, tự động co giãn.
- **AWS SageMaker Pipelines:** Quản lý và điều phối workflow ML tự động 4 bước.
- **AWS SageMaker Model Registry:** Lưu trữ và quản lý phiên bản mô hình tập trung.
- **Amazon EventBridge:** Lắng nghe các sự kiện chuyển đổi trạng thái của Pipeline và Model Registry.
- **Amazon SNS:** Gửi Email thông báo kết quả Retrain và cảnh báo sự cố tự động.
- **Amazon CloudWatch:** Lưu log hệ thống, giám sát metrics và phát cảnh báo lỗi.

---

## Thời gian & Chi phí ước tính
- **Thời gian thực hiện:** ~60 - 90 phút.
- **Chi phí hạ tầng:** ~$0.50 - $1.00 USD (Nếu dọn dẹp tài nguyên đúng theo bước Clean-up ở cuối bài lab, hầu hết các dịch vụ đều nằm trong AWS Free Tier).