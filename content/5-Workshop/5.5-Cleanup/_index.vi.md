---
title : "Dọn dẹp tài nguyên"
date : 2026-07-29
weight : 6
chapter : false
pre : " <b> 5.5. </b> "
---

Để tránh phát sinh chi phí không cần thiết trên tài khoản AWS sau khi hoàn tất kiểm thử bài workshop, hãy thực hiện dọn dẹp toàn bộ tài nguyên theo trình tự các bước dưới đây.

#### Xóa SageMaker Serverless Endpoint & Configurations
- Truy cập Amazon SageMaker Studio $\rightarrow$ Deployments $\rightarrow$ Endpoints: Chọn telco-churn-serverless-endpoint $\rightarrow$ Delete.
![clean-server](/images/5-Workshop/5.5-Cleanup/clean-server.png)
- Chọn JumpStart / Models: Chọn TelcoChurnModelGroup $\rightarrow$ Delete.
![clean-group](/images/5-Workshop/5.5-Cleanup/clean-group.png)

#### Xóa SageMaker Pipeline
- Trong SageMaker Console, mở mục Pipelines.
- Chọn TelcoChurnPipeline (hoặc tên Pipeline của bạn).
- Bấm Delete và xác nhận xóa Pipeline.
![clean-pipeline](/images/5-Workshop/5.5-Cleanup/clean-pipeline.png)

#### Xóa Amazon API Gateway & Các Hàm AWS Lambda
##### Xóa API Gateway
- Truy cập Amazon API Gateway.
- Tìm API telco-churn-api $\rightarrow$ Bấm Delete $\rightarrow$ Xác nhận xóa.
![clean-api](/images/5-Workshop/5.5-Cleanup/clean-api.png)

##### Xóa các Hàm Lambda
- Truy cập AWS Lambda $\rightarrow$ Functions.
- Tick chọn 3 hàm Lambda đã tạo trong bài workshop:
  - TelcoChurnDriftChecker  
  - TelcoChurnAutoDeployer
  - telco-churn-api-handler 
- Bấm Actions $\rightarrow$ Delete $\rightarrow$ Nhập delete để xác nhận.
![clean-lambda](/images/5-Workshop/5.5-Cleanup/clean-lambda.png)

#### Xóa Amazon EventBridge Rules & Amazon SNS Topic
##### Xóa EventBridge Rules
- Truy cập Amazon EventBridge $\rightarrow$ Rules.
- Tick chọn các Rules:
  - TelcoChurnModelApprovedRule
  - TelcoChurnPipelineSuccessRule
- Bấm Delete.
![clean-event](/images/5-Workshop/5.5-Cleanup/clean-event.png)

##### Xóa SNS Topic
- Truy cập Amazon SNS $\rightarrow$ Topics.
- Chọn Topic TelcoChurnAlerts $\rightarrow$ Bấm Delete.
![clean-sns](/images/5-Workshop/5.5-Cleanup/clean-sns.png)
- Vào mục Subscriptions $\rightarrow$ Xóa Subscription liên kết với email của bạn.
![clean-email](/images/5-Workshop/5.5-Cleanup/clean-email.png)

##### Xóa S3 Bucket & Dữ liệu
- Truy cập Amazon S3.
- Chọn Bucket telco-churn-mlops-fcaj $\rightarrow$ Bấm Empty $\rightarrow$ Nhập permanently delete để xóa sạch dữ liệu thô, dữ liệu sau xử lý và model artifacts.
![empty-s3](/images/5-Workshop/5.5-Cleanup/empty-s3.png)
- Sau khi làm rỗng, chọn lại Bucket đó và bấm Delete để xóa Bucket khỏi hệ thống.
![delete-s3](/images/5-Workshop/5.5-Cleanup/delete-s3.png)

---
title : "Dọn dẹp tài nguyên"
date : 2026-07-29
weight : 6
chapter : false
pre : " <b> 5.5. </b> "
---

Để tránh phát sinh chi phí không cần thiết trên tài khoản AWS sau khi hoàn tất kiểm thử bài workshop, hãy thực hiện dọn dẹp toàn bộ tài nguyên theo trình tự các bước dưới đây.

#### Xóa SageMaker Serverless Endpoint & Configurations
- Truy cập Amazon SageMaker Studio $\rightarrow$ Deployments $\rightarrow$ Endpoints: Chọn telco-churn-serverless-endpoint $\rightarrow$ Delete.
![clean-server](/images/5-Workshop/5.5-Cleanup/clean-server.png)
- Chọn JumpStart / Models: Chọn TelcoChurnModelGroup $\rightarrow$ Delete.
![clean-group](/images/5-Workshop/5.5-Cleanup/clean-group.png)

#### Xóa SageMaker Pipeline
- Trong SageMaker Console, mở mục Pipelines.
- Chọn TelcoChurnPipeline (hoặc tên Pipeline của bạn).
- Bấm Delete và xác nhận xóa Pipeline.
![clean-pipeline](/images/5-Workshop/5.5-Cleanup/clean-pipeline.png)

#### Xóa Amazon API Gateway & Các Hàm AWS Lambda
##### Xóa API Gateway
- Truy cập Amazon API Gateway.
- Tìm API telco-churn-api $\rightarrow$ Bấm Delete $\rightarrow$ Xác nhận xóa.
![clean-api](/images/5-Workshop/5.5-Cleanup/clean-api.png)

##### Xóa các Hàm Lambda
- Truy cập AWS Lambda $\rightarrow$ Functions.
- Tick chọn 3 hàm Lambda đã tạo trong bài workshop:
  - TelcoChurnDriftChecker  
  - TelcoChurnAutoDeployer
  - telco-churn-api-handler 
- Bấm Actions $\rightarrow$ Delete $\rightarrow$ Nhập delete để xác nhận.
![clean-lambda](/images/5-Workshop/5.5-Cleanup/clean-lambda.png)

#### Xóa Amazon EventBridge Rules & Amazon SNS Topic
##### Xóa EventBridge Rules
- Truy cập Amazon EventBridge $\rightarrow$ Rules.
- Tick chọn các Rules:
  - TelcoChurnModelApprovedRule
  - TelcoChurnPipelineSuccessRule
- Bấm Delete.
![clean-event](/images/5-Workshop/5.5-Cleanup/clean-event.png)

##### Xóa SNS Topic
- Truy cập Amazon SNS $\rightarrow$ Topics.
- Chọn Topic TelcoChurnAlerts $\rightarrow$ Bấm Delete.
![clean-sns](/images/5-Workshop/5.5-Cleanup/clean-sns.png)
- Vào mục Subscriptions $\rightarrow$ Xóa Subscription liên kết với email của bạn.
![clean-email](/images/5-Workshop/5.5-Cleanup/clean-email.png)

##### Xóa S3 Bucket & Dữ liệu
- Truy cập Amazon S3.
- Chọn Bucket telco-churn-mlops-fcaj $\rightarrow$ Bấm Empty $\rightarrow$ Nhập permanently delete để xóa sạch dữ liệu thô, dữ liệu sau xử lý và model artifacts.
![empty-s3](/images/5-Workshop/5.5-Cleanup/empty-s3.png)
- Sau khi làm rỗng, chọn lại Bucket đó và bấm Delete để xóa Bucket khỏi hệ thống.
![delete-s3](/images/5-Workshop/5.5-Cleanup/delete-s3.png)

#### Xóa Amazon CloudFront Distribution & AWS WAF
##### Disable và Xóa CloudFront Distribution
- Truy cập **Amazon CloudFront** $\rightarrow$ **Distributions**.
- Chọn Distribution telco-churn-cloudfront-waf.
- Nhấn **Disable** và chờ trạng thái chuyển sang Disabled.
![disable](/images/5-Workshop/5.5-Cleanup/disable.png)
- Sau khi đã Disable, chọn Distribution đó và nhấn **Delete** để xóa hoàn toàn.

##### Xóa Web ACLs trên AWS WAF
- Truy cập **AWS WAF & Shield** $\rightarrow$ **Web ACLs**.
- Chọn region **Global (CloudFront)**.
- Chọn Web ACL liên kết với CloudFront Distribution vừa tạo $\rightarrow$ Bấm **Action** $\rightarrow$ **Delete**.
![acls](/images/5-Workshop/5.5-Cleanup/acls.png)