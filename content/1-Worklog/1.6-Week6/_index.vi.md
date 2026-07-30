---
title: "Worklog Tuần 6"
date: 2026-07-13
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Cùng các thành viên trong nhóm build và ghép nối hoàn chỉnh SageMaker Pipeline trên môi trường AWS.
* Học cá nhân về Event-Driven Architecture trên AWS với Amazon EventBridge, S3 Event Notifications và AWS Lambda Serverless Compute.
* Tìm hiểu cơ chế giám sát lệch phân phối dữ liệu (Data Drift / Model Drift) và kiến trúc tự động tái huấn luyện trên Cloud.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                    | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo                                                                       |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------------------- |
| 2   | - Học nâng cao về Event-Driven Architecture trên AWS: <br>&emsp; + Mô hình Publish/Subscribe và cách EventBridge đóng vai trò Router trung tâm <br>&emsp; + Học cách viết Event Patterns bằng JSON để lọc sự kiện tài nguyên Cloud chính xác | 13/07/2026   | 13/07/2026      | <https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html>            |
| 3   | - Tìm hiểu cơ chế Amazon S3 Event Notifications: <br>&emsp; + Cấu hình các sự kiện s3:ObjectCreated:* trên S3 Bucket Prefix <br>&emsp; + Kết nối S3 Event Triggers gửi payload notification sang AWS Lambda / Amazon SQS / SNS               | 14/07/2026   | 14/07/2026      | <https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventNotifications.html>       |
| 4   | - Học về AWS Lambda Serverless Compute: <br>&emsp; + Vòng đời hàm Lambda, cấu hình Execution Role, Memory Allocation & Timeout limits <br>&emsp; + Cách viết hàm Lambda bằng Python với SDK boto3 để tương tác với tài nguyên AWS            | 15/07/2026   | 15/07/2026      | <https://docs.aws.amazon.com/lambda/latest/dg/welcome.html>                           |
| 5   | - Tìm hiểu khái niệm Data Drift & Model Drift trên Cloud: <br>&emsp; + Khái niệm thay đổi phân phối dữ liệu đầu vào theo thời gian <br>&emsp; + Các giải pháp giám sát chất lượng dữ liệu và tự động kích hoạt luồng tái huấn luyện          | 16/07/2026   | 16/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-data-quality.html>     |
| 6   | Cùng nhóm build SageMaker Pipeline - ProcessingStep => TuningStep => EvalStep => ConditionStep (AUC >= 0.80) => ModelStep (Register) <br> - Test Pipeline execution                                                                          | 17/07/2026   | 17/07/2026      | [SageMaker Pipelines](https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines.html) |

### Kết quả đạt được tuần 6:

* Cùng nhóm xây dựng thành công SageMaker Pipeline 4 bước: Processing (chuẩn bị dữ liệu) => HPO (tuning XGBoost) => Evaluation (kiểm tra AUC) => Condition/Register. Pipeline tự động approve model khi AUC >= 0.80.
* Nắm vững tư duy thiết lập hệ thống tự động hóa dựa trên sự kiện (Event-Driven) với EventBridge, AWS Lambda và S3 Event Triggers.
* Hiểu rõ cơ chế bắn tin sự kiện từ Amazon S3 tới AWS Lambda để khởi chạy các tác vụ Serverless.
* Hiểu sâu nguyên lý giám sát Data Drift và thiết lập luồng tự động tái huấn luyện mô hình.