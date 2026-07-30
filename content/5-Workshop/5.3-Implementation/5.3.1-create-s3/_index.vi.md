---
title : "Tạo S3 và upload dữ liệu"
date : 2026-07-27 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

1. Mở [Amazon S3 console](https://ap-southeast-1.console.aws.amazon.com/s3/home?region=ap-southeast-1) => Bấm Create bucket.
2. Bucket name: Nhập telco-churn-mlops-fcaj (hoặc tên duy nhất kèm ID tài khoản của bạn).
3. Giữ nguyên các thiết lập mặc định và bấm Create bucket.
![createS3](/images/5-Workshop/5.3-Implementation/s3name.png)
4. Trong Bucket vừa tạo, tạo các folder sau:

- raw/
- processed/
- models/
![createfolder](/images/3-Prerequiste/S3.png)
5. Tải file dữ liệu WA_Fn-UseC_-Telco-Customer-Churn.csv lên thư mục s3://telco-churn-mlops-fcaj/raw/.
![uploadfile](/images/5-Workshop/5.3-Implementation/upfile.png)


