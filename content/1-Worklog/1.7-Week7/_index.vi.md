---
title: "Worklog Tuần 7"
date: 2026-07-27
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Đào sâu kiến thức AWS Cloud về Quản lý API & Giám sát hệ thống (API Management & Operational Excellence): Học chuyên sâu về **Amazon API Gateway** (REST API vs HTTP API), **Amazon CloudWatch Alarms** và **Amazon SNS Notifications**.
* Triển khai cổng REST API công khai cho Real-time Inference bằng **API Gateway (HTTP API)** kết hợp hàm AWS Lambda `TelcoChurnPredictHandler` kết nối trực tiếp với SageMaker Serverless Endpoint.
* Cấu hình hệ thống giám sát và thông báo sự cố tự động: Bắt sự kiện kết thúc Pipeline bằng EventBridge Rule và gửi email thông báo trạng thái qua Amazon SNS, thiết lập CloudWatch Alarms cảnh báo lỗi 5XX.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Học nâng cao về **Amazon API Gateway**: <br>&emsp; + So sánh HTTP API vs REST API (HTTP API tối ưu chi phí hơn 70% và giảm độ trễ cho tác vụ Serverless) <br>&emsp; + Các dạng Payload Format Version (v1.0 vs v2.0) trong Lambda Proxy Integration <br>&emsp; + Quản lý Stages, Throttling và CORS Security | 27/07/2026 | 27/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Lập trình hàm AWS Lambda Predict Handler (`telco-churn-api-handler`): <br>&emsp; + Trích xuất chuỗi features từ HTTP request body <br>&emsp; + Gọi hàm `sagemaker_runtime.invoke_endpoint()` gửi payload dạng CSV <br>&emsp; + Nhận xác suất Churn, định dạng kết quả JSON (`churn_probability`, `prediction: "CHURN"/"RETAIN"`) <br> - **Thực hành:** Tạo Amazon API Gateway (HTTP API), tạo route `POST /predict` liên kết với Lambda Predict Handler | 28/07/2026 | 28/07/2026 | AWS API Gateway & Lambda Docs |
| 4 | - Học về **Operational Monitoring & Alerting** trên Cloud: <br>&emsp; + Quản lý Log Groups, Metric Filters trong Amazon CloudWatch Logs <br>&emsp; + Thiết lập CloudWatch Alarms dựa trên các chỉ số hiệu năng (Latency, Invocation Errors) <br>&emsp; + Tích hợp Amazon SNS Topic để gửi email cảnh báo tự động cho đội ngũ vận hành | 29/07/2026 | 29/07/2026 | AWS CloudWatch User Guide |
| 5 | - Cấu hình EventBridge Rule thứ hai (`TelcoChurnPipelineStatusRule`): <br>&emsp; + Bắt sự kiện `SageMaker Model Building Pipeline Execution Status Change` <br>&emsp; + Lọc các trạng thái kết thúc (`Succeeded`, `Failed`, `Stopped`) <br>&emsp; + Cấu hình Target gửi trực tiếp đến Amazon SNS Topic `TelcoChurnAlerts` để bắn email thông báo kết quả Retrain tự động về Gmail | 30/07/2026 | 30/07/2026 | EventBridge Target Options |
| 6 | - Cấu hình **CloudWatch Alarm** (`Invocation5XXErrors > 0`) trên SageMaker Endpoint kết nối với SNS Topic <br> - **Thực hành kiểm thử:** <br>&emsp; + Gửi request cURL qua PowerShell tới HTTP API Gateway URL (`https://c6kbjaktj9.execute-api.ap-southeast-1.amazonaws.com/predict`) <br>&emsp; + Kiểm tra log thực thi trên CloudWatch Logs để đo thời gian xử lý và xác nhận kết quả trả về thành công | 31/07/2026 | 31/07/2026 | CloudWatch Logs Console |

### Kết quả đạt được tuần 7:

* Nắm vững kiến thức thiết lập cổng giao tiếp API thời gian thực và kiến trúc giám sát hệ thống trên AWS Cloud, hiểu lý do lựa chọn HTTP API để tối ưu chi phí và hiệu năng.
* Xây dựng và Deploy thành công **Real-time Inference API**:
  * Tích hợp mượt mà **API Gateway (HTTP API)** $\rightarrow$ **Lambda Predict Handler** $\rightarrow$ **SageMaker Serverless Endpoint**.
  * Tiếp nhận request dự đoán thành công và trả về kết quả chuẩn xác (`churn_probability: 0.0406`, `prediction: "RETAIN"`).
* Hoàn thiện **Hệ thống Giám sát & Báo động (Monitoring & Notification System)**:
  * Tự động gửi Email thông báo kết quả chạy Retrain của SageMaker Pipeline (`Succeeded`/`Failed`) về hòm thư Gmail qua Amazon SNS.
  * Thiết lập CloudWatch Alarms sẵn sàng phát cảnh báo khi hệ thống gặp sự cố lỗi 5XX hoặc tăng độ trễ bất thường.