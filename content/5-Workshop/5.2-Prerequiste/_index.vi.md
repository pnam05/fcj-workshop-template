---
title : "Các bước chuẩn bị"
date : 2026-07-27
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

# Các bước chuẩn bị cho Workshop (Prerequisites)

Trước khi tiến hành triển khai hệ thống MLOps trên AWS, cần chuẩn bị đầy đủ tài khoản, quyền truy cập IAM, cấu trúc lưu trữ S3 và môi trường thực thi theo hướng dẫn dưới đây.

---

## Yêu cầu Tài khoản & Quyền hạn (AWS Account & IAM)
- **Tài khoản AWS:** Có quyền truy cập vào AWS Management.
- **Quyền IAM (IAM Permissions):** Tài khoản IAM người dùng cần có quyền AdministratorAccess hoặc các policy tối thiểu bao gồm:
  - AmazonSageMakerFullAccess
  - AWSLambda_FullAccess
  - AmazonS3FullAccess
  - AmazonEventBridgeFullAccess
  - AmazonSNSFullAccess
  - AmazonAPIGatewayAdministrator
  - IAMFullAccess (để cấu hình PassRole)

---

## Khởi tạo IAM Roles cho các dịch vụ
Hệ thống sử dụng các IAM Role sau để phân quyền giữa các dịch vụ (nguyên tắc Principal of Least Privilege):

1. **SageMaker-Telco-Churn-Role:**
   - **Service Trust:** sagemaker.amazonaws.com
   - **Attached Policies:** AmazonSageMakerFullAccess, AmazonS3FullAccess.
   - **Mục đích:** Cấp quyền cho SageMaker Processing Job, HPO Job, Evaluation và Serverless Endpoint truy cập dữ liệu S3.
 
![telco-churn-role](/images/3-Prerequiste/telco-churn-role.png)

2. **Lambda-Execution-Role (dùng cho Lambda Trigger & Lambda Deployer):**
   - **Service Trust:** lambda.amazonaws.com
   - **Attached Policies:** AWSLambdaBasicExecutionRole,AmazonSageMakerFullAccess, AmazonS3FullAccess.
   - **Inline Policy (PassRolePolicy):** Cho phép Lambda thực thi lệnh iam:PassRole tới SageMaker-Execution-Role:
     ```json
     {
         "Version": "2012-10-17",
         "Statement": [
             {
                 "Effect": "Allow",
                 "Action": "iam:PassRole",
                 "Resource": "arn:aws:iam::<YOUR_ACCOUNT_ID>:role/<SageMaker-Execution-Role-Name>"
             }
         ]
     }
     ```

---

## Tạo S3 Bucket & Cấu trúc Thư mục Data Lake
Tạo 1 S3 Bucket duy nhất với tên độc nhất trên toàn hệ thống (Ví dụ: telco-churn-mlops-<account-id>).

Tạo cấu trúc thư mục (Prefixes) bên trong Bucket như sau:
```text
telco-churn-mlops-<account-id>/
├── raw/                 # Chứa dữ liệu thô (.csv)
├── processed/           # Chứa dữ liệu sau tiền xử lý
│   ├── train/
│   ├── validation/
│   └── test/
└── models/              # Chứa các file nén model.tar.gz
```
![s3](/images/3-Prerequiste/S3.png)


## Chuẩn bị Dữ liệu & Môi trường Lập trình (Local / SageMaker Studio)
- **Tập dữ liệu:** Tải file dữ liệu huấn luyện ban đầu WA_Fn-UseC_-Telco-Customer-Churn.csv.
- **SageMaker Studio / Jupyter Notebook:** Khởi tạo môi trường Python 3.10+ trên SageMaker Studio.


