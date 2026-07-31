---
title: "Các bước chuẩn bị"
date: 2026-07-27
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---


Trước khi tiến hành triển khai hệ thống MLOps trên AWS, cần chuẩn bị đầy đủ tài khoản, quyền truy cập IAM, các IAM Role hỗ trợ, cấu trúc lưu trữ S3 và môi trường thực thi theo hướng dẫn dưới đây.

---

## 1. Yêu cầu Tài khoản & Quyền hạn (AWS Account & IAM)

- **Tài khoản AWS:** Đã đăng ký và có quyền truy cập vào **AWS Management Console**.
- **AWS Region:** Khuyến nghị sử dụng vùng **us-east-1 (N. Virginia)** hoặc **ap-southeast-1 (Singapore)** để đảm bảo hỗ trợ đầy đủ các dịch vụ (SageMaker Serverless Inference, CloudFront, AWS WAF, EventBridge).
- **Quyền IAM (IAM Permissions):** Tài khoản IAM người dùng cần có quyền **AdministratorAccess** hoặc tập hợp các Managed Policies tối thiểu bao gồm:
  - AmazonSageMakerFullAccess
  - AWSLambda_FullAccess
  - AmazonS3FullAccess
  - AmazonEventBridgeFullAccess
  - AmazonSNSFullAccess
  - AmazonAPIGatewayAdministrator
  - CloudWatchLogsFullAccess
  - AWSCloudFrontFullAccess
  - AWSWAFv2FullAccess
  - IAMFullAccess (để tạo và gắn inline policy `iam:PassRole`)

---

## 2. Khởi tạo IAM Roles cho các dịch vụ (Service Roles)

Hệ thống tuân thủ nguyên tắc **Least Privilege**, sử dụng các IAM Role riêng biệt để phân quyền giữa các dịch vụ AWS:

### 2.1. SageMaker Execution Role 
- **Service Trust:** sagemaker.amazonaws.com
- **Attached Policies:** AmazonSageMakerFullAccess, AmazonS3FullAccess
- **Mục đích:** Cấp quyền cho SageMaker Processing Job, HPO Job, Evaluation, SageMaker Pipeline và Serverless Endpoint đọc/ghi dữ liệu trên S3.

![telco-churn-role](/images/3-Prerequiste/telco-churn-role.png)

### 2.2. Lambda Execution Role 
- **Service Trust:** lambda.amazonaws.com
- **Attached Policies:** AWSLambdaBasicExecutionRole, AmazonSageMakerFullAccess, AmazonS3FullAccess, AmazonSNSFullAccess.
- **Inline Policy (PassRolePolicy):** Bắt buộc cấu hình cho phép Lambda chuyển đổi quyền (`iam:PassRole`) sang **SageMaker-Telco-Churn-Role** khi khởi chạy Pipeline và triển khai Endpoint:
  ```json
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": "iam:PassRole",
              "Resource": "arn:aws:iam::<YOUR_ACCOUNT_ID>:role/SageMaker-Telco-Churn-Role"
          }
      ]
  }
  ```

---

## 3. Chuẩn bị Dữ liệu & Môi trường Lập trình

- **Tập dữ liệu Telco Customer Churn:** Tải file dữ liệu huấn luyện ban đầu `WA_Fn-UseC_-Telco-Customer-Churn.csv` từ bộ dữ liệu chuẩn IBM / Kaggle.
- **Môi trường Lập trình:** Khởi tạo môi trường Python 3.10+ trên **SageMaker Studio / JupyterLab Notebook** hoặc máy local với các thư viện cần thiết:
  ```bash
  pip install boto3 sagemaker pandas numpy scikit-learn xgboost
  ```
