---
title: "Worklog Tuần 2"
date: 2026-06-15
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Học nâng cao về quản lý lưu trữ đối tượng với Amazon S3 (Storage Classes, Versioning, Lifecycle Rules, Bucket Policies & CORS).
* Tìm hiểu cơ chế bảo mật AWS IAM chuyên sâu (IAM Roles, Service Trust, chính sách Least Privilege & ủy quyền iam:PassRole).
* Nghiên cứu hạ tầng mạng bảo mật với Amazon VPC Endpoints (Gateway Endpoints vs Interface Endpoints).

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                               | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo                                                                  |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | -------------------------------------------------------------------------------- |
| 2   | -  Nghiên cứu nâng cao dịch vụ Amazon S3: <br>&emsp; + So sánh các lớp lưu trữ S3 Storage Classes (Standard, Intelligent-Tiering, Glacier, Deep Archive) <br>&emsp; + Cấu hình S3 Versioning & S3 Lifecycle Rules tự động chuyển vùng lưu trữ           | 15/06/2026   | 15/06/2026      | <https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html> |
| 3   | - Tìm hiểu cơ chế bảo vệ & mã hóa dữ liệu trên Amazon S3: <br>&emsp; + Mã hóa phía máy chủ SSE-S3 & SSE-KMS với AWS Key Management Service <br>&emsp; + Viết và cấu hình S3 Bucket Policies & Cross-Origin Resource Sharing (CORS)                      | 16/06/2026   | 16/06/2026      | <https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-policies.html>     |
| 4   | - Tìm hiểu chuyên sâu AWS IAM Security: <br>&emsp; + Phân biệt IAM User vs IAM Role, cơ chế Assume Role & Service Trust Relationships <br>&emsp; + Áp dụng nguyên tắc cấp quyền tối thiểu (Principle of Least Privilege) bằng Customer Managed Policies | 17/06/2026   | 17/06/2026      | <https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html>                 |
| 5   | - Nghiên cứu quyền ủy quyền dịch vụ iam:PassRole: <br>&emsp; + Cơ chế cho phép dịch vụ AWS chuyển giao Role cho một dịch vụ khác thực thi tác vụ <br>&emsp; + Cấu hình PassRole giới hạn phạm vi dịch vụ được phép gọi theo chuẩn bảo mật AWS           | 18/06/2026   | 18/06/2026      | <https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html>    |
| 6   | - Nghiên cứu hạ tầng VPC Endpoints (AWS PrivateLink): <br>&emsp; + Phân biệt Gateway Endpoints (Amazon S3, DynamoDB) và Interface Endpoints <br>&emsp; + Cơ chế truy cập dịch vụ AWS nội bộ từ VPC mà không đi qua Internet công cộng                   | 19/06/2026   | 19/06/2026      | <https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints.html>          |

### Kết quả đạt được tuần 2:

* Nắm vững cách lựa chọn S3 Storage Class tối ưu chi phí và thiết lập vòng đời dữ liệu S3 Lifecycle.
* Hiểu rõ cơ chế mã hóa dữ liệu SSE-KMS và phân quyền truy cập an toàn bằng S3 Bucket Policies.
* Nắm vững tư duy bảo mật IAM Role, ủy quyền dịch vụ iam:PassRole và nguyên tắc Least Privilege.
* Làm chủ khái niệm VPC Endpoints để thiết lập kết nối nội bộ riêng tư và an toàn đến Amazon S3.