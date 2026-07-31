---
title: "Worklog Tuần 8"
date: 2026-07-27
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Cùng nhóm viết Blog 1 (RDS Proxy), Blog 2 (Bảo mật AWS), và Blog 3 (Quản lý hạ tầng với Terraform).
* Học cá nhân về AWS Well-Architected Framework (5 trụ cột thiết kế hệ thống Cloud chuẩn mực).
* Học cá nhân về Cost Optimization qua AWS Cost Explorer, AWS Budgets và quy trình Clean-up tài nguyên Cloud an toàn.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                                                                                     | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo                                                                                      |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ---------------------------------------------------------------------------------------------------- |
| 2   | - Học nâng cao AWS Well-Architected Framework (5 trụ cột: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization) <br> - Phân tích chi phí vận hành qua AWS Cost Explorer và thiết lập AWS Budgets Alarm cảnh báo chi phí                                                                   | 27/07/2026   | 27/07/2026      | <https://aws.amazon.com/architecture/well-architected/>                                              |
| 3   | Cùng nhóm tìm hiểu RDS Proxy: Connection Pooling, Multiplexing, Graceful Failover, IAM Authentication <br> - Cùng nhóm viết Blog 1: "Bài toán cạn kiệt kết nối với RDS Proxy"                                                                                                                                                 | 28/07/2026   | 28/07/2026      | [RDS Proxy](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-proxy.html)                   |
| 4   | Cùng nhóm tìm hiểu bảo mật AWS: IAM Least Privilege, WAF, GuardDuty, Security Hub, Public/Private Subnet <br> - Cùng nhóm viết Blog 2: "Bảo mật trong phát triển phần mềm trên AWS"                                                                                                                                           | 29/07/2026   | 29/07/2026      | [AWS Well-Architected Security](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/) |
| 5   | Cùng nhóm tìm hiểu Terraform & Infrastructure as Code (IaC): HCL syntax, plan/apply workflow, Remote Backend (S3 & DynamoDB), Modules, Infrastructure Drift <br> - Cùng nhóm viết Blog 3: "Quản lý hạ tầng với Terraform — Không chỉ là Click on the Console" <br> - Nghiên cứu các phương pháp kiểm thử tích hợp (Integration Testing) và thiết lập bảng tiêu chí đánh giá hiệu năng (Validation Matrix) cho hệ thống Serverless trên Cloud | 30/07/2026   | 30/07/2026      | [Terraform AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)         |
| 6   | - Học quy trình dọn dẹp tài nguyên (Resource Clean-up Best Practices) trên AWS Cloud: <br>&emsp; + Các bước xóa Endpoint, làm rỗng S3 Buckets, hủy Lambda functions, API Gateway & EventBridge Rules <br>&emsp; + Đảm bảo tài khoản AWS không phát sinh chi phí duy trì tài nguyên ngoài ý muốn sau khi kết thúc đợt thực tập | 31/07/2026   | 31/07/2026      | <https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html>                       |

### Kết quả đạt được tuần 8:

* Cùng nhóm viết và đăng Blog 1 - phân tích chi tiết bài toán Connection Exhaustion khi kết hợp Lambda + RDS, và cách RDS Proxy giải quyết qua Multiplexing, Graceful Failover, và IAM Authentication.
* Cùng nhóm viết và đăng Blog 2 - tổng hợp 5 bài học bảo mật thực tế khi phát triển trên AWS: không hardcode Access Key, Least Privilege, phân tách Public/Private Subnet, bảo vệ với WAF, giám sát với GuardDuty/Inspector/Security Hub.
* Cùng nhóm viết và đăng Blog 3 - làm rõ hành trình dịch chuyển từ thao tác thủ công ("ClickOps") sang tư duy Infrastructure as Code (IaC) với Terraform, quản lý state an toàn với Remote Backend (S3 + DynamoDB), tái sử dụng code qua Modules và xử lý Infrastructure Drift.
* Nắm vững các nguyên tắc cốt lõi của AWS Well-Architected Framework, biết cách phân tích và tối ưu hóa chi phí hạ tầng Cloud bằng AWS Budgets và Cost Explorer.
* Hiểu rõ quy trình kiểm thử tích hợp và xây dựng Validation Matrix đánh giá độ tin cậy của hệ thống Cloud.
* Làm chủ quy trình Clean-up dọn dẹp tài nguyên chuẩn mực trên AWS Cloud, bảo vệ tài khoản khỏi phát sinh chi phí ngoài ý muốn.