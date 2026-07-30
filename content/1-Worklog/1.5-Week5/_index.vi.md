---
title: "Worklog Tuần 5"
date: 2026-07-06
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Cùng các thành viên trong nhóm thiết kế kiến trúc hệ thống MLOps và hoàn thiện tài liệu Proposal.
* Học cá nhân về Model Governance & Versioning trên AWS Cloud với SageMaker Model Registry.
* Tìm hiểu khái niệm Quality Gate (Condition Steps) và các cơ chế xử lý ngoại lệ trong quy trình tự động hóa trên Cloud.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                                                     | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo                                                                      |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------------------ |
| 2   | - Học nâng cao về Model Governance & Versioning trên AWS: <br>&emsp; + Vai trò của SageMaker Model Registry trong môi trường Production <br>&emsp; + Quản lý Model Package Groups, phiên bản mô hình (Model Versions) và các trạng thái phê duyệt (PendingManualApproval, Approved, Rejected) | 06/07/2026   | 06/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html>                |
| 3   | - Tìm hiểu khái niệm Model Lineage & Governance trên Cloud: <br>&emsp; + Kiểm soát chất lượng mô hình tự động, lưu trữ metadata mô hình tập trung <br>&emsp; + Theo dõi luồng dữ liệu nguồn (Data Lineage) kết nối trực tiếp với từng phiên bản mô hình                                       | 07/07/2026   | 07/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/governance.html>                    |
| 4   | Cùng nhóm thiết kế kiến trúc MLOps Pipeline hoàn chỉnh - S3 => Pipeline 4 bước => Model Registry => EventBridge => Lambda Deployer => Serverless Endpoint <br> - Cùng nhóm viết Proposal - Problem Statement, Solution Architecture, Timeline                                                 | 08/07/2026   | 08/07/2026      |                                                                                      |
| 5   | - Tìm hiểu cú pháp thiết lập bước kiểm tra điều kiện (ConditionStep) trên Cloud: <br>&emsp; + Đọc giá trị chỉ số từ file kết quả JSON bằng JsonGet <br>&emsp; + Đặt điều kiện so sánh ConditionGreaterThanOrEqualTo phục vụ Quality Gate kiểm soát chất lượng                                 | 09/07/2026   | 09/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/build-and-manage-propertyfile.html> |
| 6   | - Nghiên cứu cơ chế dừng quy trình tự động và báo lỗi (FailStep): <br>&emsp; + Quản lý nhánh lỗi else_steps khi mô hình không vượt qua ngưỡng Quality Gate <br>&emsp; + Cấu hình thông báo lỗi và bảo vệ môi trường Production khỏi các mô hình kém chất lượng                                | 10/07/2026   | 10/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines-sdk.html>                 |

### Kết quả đạt được tuần 5:
* Cùng nhóm thiết kế xong kiến trúc MLOps Pipeline hoàn chỉnh và viết Proposal đầy đủ với Problem Statement, Architecture Diagram, Timeline, Budget và Risk Assessment.
* Nắm vững khái niệm MLOps Governance trên AWS Cloud, hiểu cách quản lý phiên bản mô hình tập trung bằng SageMaker Model Registry.
* Hiểu rõ cơ chế xây dựng bước ConditionStep và toán tử so sánh JsonGet cho các hệ thống kiểm soát tự động.
* Nắm vững cách quản lý nhánh rẽ điều kiện và cơ chế khởi chạy FailStep khi mô hình không đạt chuẩn.