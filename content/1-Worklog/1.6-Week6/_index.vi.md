---
title: "Worklog Tuần 6"
date: 2026-07-20
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Đào sâu kiến thức AWS Cloud về Kiến trúc Tự động hóa Dựa trên Sự kiện (Event-Driven Architecture): Học chuyên sâu về dịch vụ **Amazon EventBridge** (Bus, Rules, Event Patterns), **S3 Event Notifications** và **AWS Lambda Serverless Compute**.
* Viết và cấu hình hàm AWS Lambda `TelcoChurnDriftChecker` đính kèm S3 Event Notification để tự động kiểm tra Data Drift và khởi chạy SageMaker Pipeline khi có dữ liệu mới.
* Viết và triển khai hàm AWS Lambda `TelcoChurnAutoDeployer` kết hợp EventBridge Rule bắt sự kiện `Approved` từ Model Registry để tự động tạo/cập nhật **SageMaker Serverless Endpoint** (Continuous Deployment - CD).

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Học nâng cao về **Event-Driven Architecture trên AWS**: <br>&emsp; + Tìm hiểu mô hình Publish/Subscribe và cách EventBridge đóng vai trò làm Router trung tâm <br>&emsp; + Học cách viết Event Patterns bằng JSON để lọc sự kiện chính xác <br>&emsp; + Tối ưu hóa AWS Lambda (Execution Role, Timeouts, Memory Allocation, Error Handling) | 20/07/2026 | 20/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Lập trình hàm AWS Lambda `TelcoChurnDriftChecker` bằng Python 3.11: <br>&emsp; + Đọc file CSV mới upload từ S3 qua `boto3` <br>&emsp; + Đánh giá rủi ro lệch phân phối dữ liệu (Data Drift / Missing values) <br>&emsp; + Bắn mail cảnh báo qua SNS và gọi `sagemaker_client.start_pipeline_execution()` <br> - **Thực hành:** Tạo S3 Event Notification trên folder `raw/` kết nối trực tiếp với Lambda | 21/07/2026 | 21/07/2026 | AWS Lambda & S3 Developer Guide |
| 4 | - Học về **SageMaker Serverless Inference**: <br>&emsp; + Ưu điểm so với Real-time Provisioned Endpoints (Tối ưu chi phí, tự động scale về 0 khi không có traffic) <br>&emsp; + Cấu hình MemorySizeInMB (2048 MB) và MaxConcurrency <br> - Viết hàm AWS Lambda `TelcoChurnAutoDeployer` dùng thư viện `boto3` để tự động tạo Model, Endpoint Configuration và gọi `update_endpoint()` / `create_endpoint()` | 22/07/2026 | 22/07/2026 | SageMaker Serverless Inference Docs |
| 5 | - Cấu hình **Amazon EventBridge Rule** (`TelcoChurnModelApprovedRule`): <br>&emsp; + Thiết lập Event Pattern bắt sự kiện `SageMaker Model Package State Change` <br>&emsp; + Lọc chính xác `ModelPackageGroupName`: `TelcoChurnModelGroup` và `ModelApprovalStatus`: `Approved` <br>&emsp; + Gán Target chuyển sang hàm Lambda `TelcoChurnAutoDeployer` | 23/07/2026 | 23/07/2026 | EventBridge User Guide |
| 6 | - Kiểm thử tích hợp chuỗi tự động hóa liên hoàn (Automation Chain): <br>&emsp; 1. Upload file CSV mới lên `s3://.../raw/` $\rightarrow$ Lambda Drift Checker kích hoạt Pipeline <br>&emsp; 2. Pipeline chạy xong $\rightarrow$ Đăng ký Model `Approved` vào Registry <br>&emsp; 3. EventBridge phát hiện Model `Approved` $\rightarrow$ Kích hoạt Lambda Deployer <br>&emsp; 4. Endpoint `telco-churn-serverless-endpoint` chuyển trạng thái sang `Updating` $\rightarrow$ `InService` thành công | 24/07/2026 | 24/07/2026 | AWS CloudWatch Logs |

### Kết quả đạt được tuần 6:

* Nắm vững tư duy thiết lập hệ thống tự động hóa dựa trên sự kiện (Event-Driven) trên Cloud, làm chủ các dịch vụ AWS Lambda, EventBridge Rules và S3 Event Triggers.
* Xây dựng và tích hợp thành công **Lambda Drift Checker**: Tự động phát hiện dữ liệu mới upload vào `s3://.../raw/`, kiểm tra chất lượng và tự động bấm nút chạy SageMaker Pipeline.
* Xây dựng và triển khai thành công luồng **Continuous Deployment (CD)** hoàn toàn tự động:
  * EventBridge Rule lắng nghe chính xác sự kiện mô hình được phê duyệt (`Approved`).
  * Lambda `TelcoChurnAutoDeployer` tự động khởi tạo Model, Endpoint Config và cập nhật mô hình mới lên **SageMaker Serverless Endpoint** mà không làm gián đoạn hệ thống (Zero-Downtime Deployment).