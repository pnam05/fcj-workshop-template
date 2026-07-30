---
title: "Worklog Tuần 7"
date: 2026-07-20
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Cùng các thành viên trong nhóm code các hàm Lambda, cấu hình API Gateway và kiểm thử API thời gian thực.
* Học cá nhân về kiến trúc suy luận Serverless trên AWS Cloud với SageMaker Serverless Inference.
* Học cá nhân về Amazon API Gateway (HTTP API vs REST API), Amazon EventBridge Rules và hệ thống báo động CloudWatch Alarms & Amazon SNS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                              | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo                                                                                           |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | --------------------------------------------------------------------------------------------------------- |
| 2   | - Học cá nhân về SageMaker Serverless Inference: <br>&emsp; + Ưu điểm tối ưu chi phí, cơ chế tự động auto-scaling về 0 khi không có traffic <br>&emsp; + Cấu hình dung lượng bộ nhớ MemorySizeInMB (1024MB - 6144MB) và giới hạn MaxConcurrency                        | 20/07/2026   | 20/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/serverless-endpoints.html>                               |
| 3   | - Học chuyên sâu Amazon API Gateway: <br>&emsp; + So sánh HTTP API vs REST API (HTTP API tối ưu chi phí hơn 70% và giảm độ trễ cho Serverless workloads) <br>&emsp; + Payload Format Version (v1.0 vs v2.0) trong Lambda Proxy Integration, Throttling & CORS Security | 21/07/2026   | 21/07/2026      | <https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api.html>                              |
| 4   | - Tìm hiểu cách viết EventBridge Rules lọc sự kiện nâng cao: <br>&emsp; + Viết Event Patterns dạng JSON bắt các sự kiện chuyển đổi trạng thái tài nguyên AWS <br>&emsp; + Gán Target tự động kích hoạt hàm AWS Lambda hoặc gửi tin nhắn sang Amazon SNS                | 22/07/2026   | 22/07/2026      | <https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html>                         |
| 5   | - Nghiên cứu dịch vụ Amazon CloudWatch & Amazon SNS: <br>&emsp; + Cấu hình CloudWatch Alarms dựa trên các chỉ số hiệu năng (Latency, Invocation Errors 5XX) <br>&emsp; + Tích hợp Amazon SNS Topic để tự động bắn email cảnh báo sự cố cho đội ngũ vận hành            | 23/07/2026   | 23/07/2026      | <https://docs.aws.amazon.com/cloudwatch/latest/monitoring/AlarmThatSendsEmail.html>                       |
| 6   | Cùng nhóm End-to-End Testing: upload data mới => DriftChecker trigger => Pipeline chạy => Model Registered => Deployer update Endpoint => Predict API trả kết quả <br> - Phân tích CloudWatch logs để xác minh từng bước                                               | 24/07/2026   | 24/07/2026      | [CloudWatch Log Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html) |

### Kết quả đạt được tuần 7:

* Cùng nhóm hoàn thành End-to-End Testing toàn bộ 5 khâu - tất cả hoạt động chính xác từ data ingestion đến inference qua API.
* Nắm vững kiến thức thiết lập cổng giao tiếp API thời gian thực và kiến trúc giám sát hệ thống trên AWS Cloud.
* Làm chủ khái niệm SageMaker Serverless Inference và cách cấu hình memory/concurrency tối ưu chi phí.
* Hiểu cách kết nối HTTP API Gateway với Lambda Proxy Integration và thiết lập hệ thống báo động CloudWatch Alarms & Amazon SNS.