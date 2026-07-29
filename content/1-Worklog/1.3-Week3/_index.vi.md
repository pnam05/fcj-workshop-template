---
title: "Worklog Tuần 3"
date: 2026-06-29
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Đào sâu kiến thức AWS Cloud nâng cao về Quản lý Lưu trữ (Amazon S3 Storage Classes, Bucket Policies), Bảo mật IAM (Assume Role, Principle of Least Privilege) và Mạng (VPC Endpoints).
* Khởi tạo kiến trúc lưu trữ Data Lake trên Amazon S3 với cấu trúc phân cấp thư mục tiêu chuẩn (`raw/`, `processed/`, `models/`).
* Đóng gói bước tiền xử lý dữ liệu vào `SageMaker ProcessingStep` bằng `SKLearnProcessor` và kết nối thành công với S3 Bucket.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Học nâng cao về dịch vụ **Amazon S3**: <br>&emsp; + Phân biệt các Storage Classes (Standard, Intelligent-Tiering, Glacier) <br>&emsp; + Cấu hình S3 Versioning, Lifecycle Rules & Encryption (SSE-S3, SSE-KMS) <br>&emsp; + Tìm hiểu S3 Bucket Policy & CORS <br> - **Thực hành:** Khởi tạo S3 Bucket chính `telco-churn-mlops-fcaj` và tạo cấu trúc folder (`raw/`, `processed/`, `models/`) | 29/06/2026 | 29/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Tìm hiểu chuyên sâu về **AWS IAM Security**: <br>&emsp; + IAM Roles vs IAM Users, khái niệm Service Trust <br>&emsp; + Cơ chế `iam:PassRole` và cách gán quyền cho các dịch vụ AWS <br>&emsp; + Tìm hiểu khái niệm VPC Endpoints (Gateway vs Interface) để truy cập S3 nội bộ an toàn <br> - **Thực hành:** Tạo `SageMaker-Execution-Role` với các Policy tối thiểu chuẩn Least Privilege | 30/06/2026 | 30/06/2026 | AWS Security Best Practices |
| 4 | - Tải tập dữ liệu thô `WA_Fn-UseC_-Telco-Customer-Churn.csv` lên thư mục `s3://telco-churn-mlops-fcaj/raw/` bằng AWS CLI <br> - Nghiên cứu tài liệu SageMaker Python SDK về đối tượng `SKLearnProcessor` và `ProcessingStep` <br> - Đưa script `preprocessing.py` lên môi trường SageMaker Studio để chuẩn bị đóng gói | 01/07/2026 | 01/07/2026 | SageMaker SDK Documentation |
| 5 | - Cấu hình `ProcessingInput` (trỏ tới file dữ liệu thô trên S3) và `ProcessingOutput` (trỏ tới các folder `train/`, `validation/`, `test/`) <br> - Viết mã nguồn khởi chạy thử nghiệm `ProcessingStep` độc lập trên instance `ml.m5.large` | 02/07/2026 | 02/07/2026 | AWS Hands-on Labs |
| 6 | - Kiểm tra log thực thi của Processing Job trên Amazon CloudWatch Logs <br> - Kiểm tra kết quả output trên S3: Xác nhận 3 file `train.csv`, `validation.csv`, `test.csv` được tự động sinh ra và đẩy đúng vào `s3://telco-churn-mlops-fcaj/processed/` <br> - Đóng gói code `ProcessingStep` hoàn chỉnh để chuẩn bị ghép vào SageMaker Pipeline | 03/07/2026 | 03/07/2026 | CloudWatch Logs Console |

### Kết quả đạt được tuần 3:

* Nắm vững kiến thức bảo mật AWS: Hiểu rõ cơ chế ủy quyền IAM Role, chính sách Least Privilege và cách thiết lập kết nối an toàn đến Amazon S3 qua VPC Endpoints.
* Thiết lập thành công S3 Data Lake `telco-churn-mlops-fcaj` với cấu trúc thư mục phân cấp rõ ràng, bật Encryption mặc định để bảo mật dữ liệu khách hàng.
* Khởi tạo và gán quyền chính xác cho `SageMaker-Execution-Role`, cho phép SageMaker truy cập S3 và thực thi các container job.
* Đóng gói và chạy thành công `TelcoChurnProcessStep` bằng `SKLearnProcessor` trên SageMaker:
  * Tự động đọc dữ liệu thô từ `s3://.../raw/`.
  * Thực hiện làm sạch, encode và split dữ liệu.
  * Tự động ghi kết quả ra 3 thư mục tương ứng trên S3 (`processed/train`, `processed/validation`, `processed/test`) mà không cần thao tác thủ công.