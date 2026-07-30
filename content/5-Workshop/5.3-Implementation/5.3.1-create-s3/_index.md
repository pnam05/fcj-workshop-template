---
title : "Create S3 and Upload Data"
date : 2026-07-27 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

1. Open [Amazon S3 console](https://ap-southeast-1.console.aws.amazon.com/s3/home?region=ap-southeast-1) => Click Create bucket.
2. Bucket name: Enter telco-churn-mlops-fcaj (or a unique name with your account ID).
3. Keep default settings and click Create bucket.
![createS3](/images/5-Workshop/5.3-Implementation/s3name.png)
4. Inside the newly created Bucket, create the following folders:

- raw/
- processed/
- models/
![createfolder](/images/3-Prerequiste/S3.png)
5. Upload data file WA_Fn-UseC_-Telco-Customer-Churn.csv to folder s3://telco-churn-mlops-fcaj/raw/.
![uploadfile](/images/5-Workshop/5.3-Implementation/upfile.png)