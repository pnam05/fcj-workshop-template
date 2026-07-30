---
title: "Worklog Tuần 1"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

* Kết nối, làm quen với các thành viên trong First Cloud AI Journey (FCAJ) và nắm rõ nội quy thực tập.
* Nắm vững các dịch vụ AWS Cloud cơ bản (IAM, S3, EC2) và làm quen với giao diện AWS Console & AWS CLI.
* Khảo sát bài toán dự đoán khách hàng rời bỏ dịch vụ viễn thông (Telco Customer Churn) và định hình mục tiêu cho dự án MLOps.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tham gia buổi Onboarding, làm quen với các thành viên FCAJ <br> - Đọc và lưu ý các nội quy, quy định, kỷ luật tại đơn vị thực tập | 15/06/2026 | 15/06/2026 | Quy định thực tập FCAJ |
| 3 | - Tìm hiểu tổng quan về AWS Cloud & các nhóm dịch vụ nền tảng: <br>&emsp; + Identity & Access Management (IAM) <br>&emsp; + Compute (EC2) <br>&emsp; + Storage (S3) <br>&emsp; + Networking (VPC, Security Group) | 16/06/2026 | 16/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Thiết lập tài khoản AWS Free Tier & cấu hình bảo mật cơ bản (MFA) <br> - Cài đặt AWS CLI v2 trên máy cục bộ <br> - **Thực hành:** Tạo IAM User, tạo Access Key, cấu hình aws configure (Region ap-southeast-1) và kiểm tra kết nối qua terminal | 17/06/2026 | 17/06/2026 | AWS Documentation |
| 5 | - Nghiên cứu dịch vụ lưu trữ Amazon S3 & dịch vụ ảo hóa Amazon EC2 <br> - Khảo sát tập dữ liệu Telco Customer Churn (`WA_Fn-UseC_-Telco-Customer-Churn.csv`) <br> - Xác định các yêu cầu kỹ thuật và công nghệ cần dùng cho hệ thống MLOps | 18/06/2026 | 18/06/2026 | Kaggle / AWS SageMaker Docs |
| 6 | - **Thực hành:** <br>&emsp; + Tạo S3 Bucket thử nghiệm qua AWS CLI <br>&emsp; + Khởi tạo EC2 Instance (Amazon Linux 2), kết nối SSH qua Terminal <br>&emsp; + Thảo luận với Mentor về định hướng xây dựng MLOps Platform tự động | 19/06/2026 | 19/06/2026 | AWS Hands-on Labs |

### Kết quả đạt được tuần 1:

* Hiểu rõ nội quy thực tập, quy trình làm việc và kết nối tốt với các thành viên trong nhóm FCAJ.
* Cài đặt và cấu hình thành công AWS CLI v2 trên máy cá nhân với IAM Access Key (Region ap-southeast-1).
* Nắm vững khái niệm và thao tác cơ bản trên các dịch vụ cốt lõi:
  * **IAM:** Tạo Role, cấp quyền Least Privilege và hiểu về PassRole.
  * **S3:** Khai niệm Bucket, Prefix, Object và phân quyền truy cập.
  * **EC2:** Launch instance, cấu hình Security Group (Inbound/Outbound rules) và kết nối SSH an toàn.
* Khảo sát xong tập dữ liệu Telco Customer Churn và chốt được định hướng kiến trúc MLOps Platform tự động trên AWS cho dự án cá nhân.