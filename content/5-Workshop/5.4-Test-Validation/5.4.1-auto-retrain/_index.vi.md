---
title : "Kiểm thử Luồng Retrain & Triển khai Tự động"
date : 2026-07-29
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

#### 1. Giả lập Upload File Dữ liệu Mới lên S3
- Vào S3 Console $\rightarrow$ Buckets $\rightarrow$ telco-churn-mlops-fcaj $\rightarrow$
raw/ -> Upload.
- Upload file dữ liệu mới 
![new](/images/5-Workshop/5.4-Test-Validation/up-newdata.png)

#### 2. Kiểm tra Cảnh báo Data Drift & Trigger Pipeline (Lambda Drift Checker)
- Mở AWS Lambda $\rightarrow$ chọn hàm TelcoChurnDriftChecker $\rightarrow$ chọn tab Monitor $\rightarrow$ View CloudWatch logs.
- Kiểm tra log thực thi mới nhất:
  - Log mong đợi: Lambda phát hiện file mới raw/new_telecom_data.csv, thực hiện kiểm tra drift, bắn mail qua SNS và gọi start_pipeline_execution().
  ![log](/images/5-Workshop/5.4-Test-Validation/newdata-log.png)
- Mở Hòm thư Gmail để kiểm tra Email cảnh báo từ Amazon SNS.
  ![newdata-gmail](/images/5-Workshop/5.4-Test-Validation/newdata-gmail.png)

#### 3. Theo dõi Tiến độ Thực thi của SageMaker Pipeline
- Truy cập Amazon SageMaker $\rightarrow$ chọn Pipelines $\rightarrow$ chọn TelcoChurnPipeline.
- Click vào lượt chạy mới nhất (Execution ID) để quan sát sơ đồ 4 bước đang thực thi:
  - TelcoChurnProcessStep (Succeeded) $\rightarrow$ TelcoChurnHpoStep (Succeeded) $\rightarrow$ TelcoChurnEvalStep (Succeeded) $\rightarrow$ TelcoChurnCheckAUCThreshold (True) $\rightarrow$ TelcoChurnRegisterModelStep (Approved).
  
![pipeline](/images/5-Workshop/5.4-Test-Validation/pipeline.png)
- Xác nhận Email báo kết quả Pipeline: Khi Pipeline hoàn thành, EventBridge Rule (TelcoChurnPipelineStatusRule) bắt sự kiện và gửi email xác nhận:
 ![done-gmail](/images/5-Workshop/5.4-Test-Validation/done-gmail.png)

#### 4. Xác nhận Tự động Deploy lên Serverless Endpoint (Lambda Deployer)
- Truy cập Amazon SageMaker $\rightarrow$ chọn Model Registry $\rightarrow$ TelcoChurnModelGroup.
  - Bạn sẽ thấy mô hình mới vừa được đăng ký với phiên bản (Version) mới nhất và trạng thái Approved.
   ![model-registry](/images/5-Workshop/5.4-Test-Validation/model-registry.png)
- EventBridge Rule (TelcoChurnModelApprovedRule) bắt sự kiện Approved và tự động kích hoạt TelcoChurnAutoDeployer.
- Kiểm tra log của Lambda Deployer (TelcoChurnAutoDeployer):
    ![deploy-logs](/images/5-Workshop/5.4-Test-Validation/deploy-logs.png)
- Truy cập Amazon SageMaker $\rightarrow$ Endpoints $\rightarrow$ telco-churn-serverless-endpoint:
  - Trạng thái Endpoint sẽ chuyển từ Updating sang InService với EndpointConfig mới trỏ về Version mô hình vừa được retrain!
  
![inservice](/images/5-Workshop/5.4-Test-Validation/inservice.png)