---
title: "Bản đề xuất"
date: 2026-07-27
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# MLOps Platform for Telco Customer Churn Prediction
## Hệ thống MLOps tự động hóa huấn luyện, đánh giá và triển khai mô hình dự đoán rời bỏ dịch vụ viễn thông trên AWS

### 1. Tóm tắt điều hành  
Dự án Telco Customer Churn MLOps Platform được thiết kế nhằm xây dựng một quy trình MLOps khép kín (End-to-End MLOps Pipeline) tự động hóa toàn bộ vòng đời của mô hình Machine Learning. Nền tảng thực hiện từ khâu xử lý dữ liệu, huấn luyện/tối ưu siêu tham số (HPO), đánh giá chất lượng mô hình, đến đăng ký vào Model Registry và tự động triển khai (Deploy) lên Serverless Endpoint ngay khi mô hình được phê duyệt (Approved). Toàn bộ cổng giao tiếp API thời gian thực được bảo vệ và tối ưu bằng Amazon CloudFront kết hợp tường lửa AWS WAF, giúp doanh nghiệp viễn thông chủ động phát hiện khách hàng có rủi ro rời bỏ dịch vụ một cách an toàn, bảo mật với chi phí vận hành tối ưu nhất. 

### 2. Tuyên bố vấn đề  
#### Vấn đề hiện tại  
Các mô hình dự đoán Churn truyền thống thường được phát triển thủ công trên môi trường cục bộ (Local Notebooks), gây ra hiện tượng lệch mô hình (Model Drift) khi dữ liệu thực tế thay đổi theo thời gian. Quy trình triển khai thủ công từ Notebook lên Production mất nhiều thời gian, dễ gây ra lỗi vận hành và không có cơ chế tự động Retrain khi hiệu năng mô hình giảm sút. Ngoài ra, việc mở API trực tiếp ra Internet mà không có các lớp bảo vệ tường lửa khiến hệ thống dễ bị tấn công DoS/DDoS hoặc bị khai thác quá mức. 
 
#### Giải pháp  
Xây dựng hệ thống MLOps trên nền tảng AWS SageMaker Workflow (Pipeline) kết hợp với kiến trúc Event-Driven Automation. Khi Admin tải dữ liệu mới lên Amazon S3, AWS Lambda kiểm tra Data Drift và gửi thông báo qua SNS. Nếu cần retrain, SageMaker Pipeline sẽ chạy quy trình 4 bước (Processing, HPO, Evaluation, Condition Check với AUC >= 0.80). Khi mô hình đạt chuẩn và chuyển trạng thái Approved trong SageMaker Model Registry, Amazon EventBridge sẽ kích hoạt Lambda Deployer để tự động cập nhật Serverless Endpoint mà không làm gián đoạn dịch vụ (Zero-downtime Deployment). Hệ thống sử dụng Amazon CloudFront làm Edge Location tối ưu tốc độ phản hồi API và tích hợp AWS WAF với quy tắc Rate Limiting để ngăn chặn các truy cập bất thường.

#### Lợi ích và hoàn vốn đầu tư (ROI)  
- **Kỹ thuật:** Rút ngắn thời gian từ khâu Huấn luyện đến Deploy từ vài ngày xuống còn vài giờ. Tăng cường bảo mật và giảm độ trễ API cho Client nhờ CloudFront & AWS WAF. Cơ chế Serverless Endpoint giúp tối ưu chi phí hạ tầng vì chỉ trả tiền khi có request suy luận (Inference).  
- **Kinh doanh:** Phát hiện sớm khách hàng rời bỏ dịch vụ, bảo vệ nguồn thu cho doanh nghiệp và đảm bảo tính khả dụng của hệ thống trước các cuộc tấn công mạng.
- **Chi phí ước tính:** ~$2.7 - $4.0 USD/tháng cho các đợt pipeline retrain và serverless inference (có hỗ trợ lớp bảo vệ WAF cơ bản).
 
### 3. Kiến trúc giải pháp  

![Architecture](/images/2-Proposal/architecture.png)

#### Dịch vụ AWS sử dụng  
- **Amazon S3:** Lưu trữ dữ liệu thô, dữ liệu đã xử lý và các Model Artifacts.
- **Amazon CloudFront & AWS WAF:** Cung cấp CDN làm cổng đại diện công khai (Edge Location), tăng tốc độ truyền tải API và áp dụng quy tắc tường lửa (Rate Limiting) chống tấn công DDoS/DDoS Layer 7.
- **Amazon API Gateway & AWS Lambda (Inference):** Tiếp nhận request dự đoán từ CloudFront qua REST API, thực hiện tiền xử lý dữ liệu thời gian thực.
- **AWS SageMaker Serverless Endpoint:** Cung cấp API suy luận thời gian thực với mô hình XGBoost, tự động co giãn theo lưu lượng.
- **AWS Lambda (Drift Checker & Trigger):** Kiểm tra Data Drift khi có dữ liệu mới trên S3 và kích hoạt SageMaker Pipeline.
- **AWS SageMaker Pipelines:** Điều phối quy trình MLOps 4 bước (Processing, Tuning, Evaluation, Condition & Register).
- **AWS SageMaker Model Registry:** Quản lý các phiên bản mô hình và trạng thái phê duyệt.
- **Amazon EventBridge & AWS Lambda (Deployer):** Lắng nghe sự kiện Model Approved để tự động cập nhật Serverless Endpoint.
- **Amazon CloudWatch & Amazon SNS:** Lưu trữ Logs, đặt Alarm cảnh báo lỗi API/Endpoint và gửi Email thông báo tự động về Gmail.

### 4. Triển khai kỹ thuật  
#### Các giai đoạn triển khai  
  
1. **Khám phá Dữ liệu & Huấn luyện Thử nghiệm:** Phân tích tập dữ liệu Telco Customer Churn. Tiền xử lý dữ liệu bằng SKLearnProcessor. Huấn luyện thử nghiệm mô hình XGBoost đơn lẻ và đánh giá chỉ số AUC.
2. **Tự động hóa Pipeline & MLOps Workflow:** Cấu hình Hyperparameter Tuning Job cho XGBoost. Viết script đánh giá xuất file evaluation.json chứa thông số AUC. Xây dựng SageMaker Pipeline hoàn chỉnh với ConditionStep (chỉ đăng ký mô hình nếu AUC >= 0.80).
3. **Event-Driven Automated Deployment & Public API Security:** Lập trình AWS Lambda Deployer xử lý việc cập nhật Endpoint linh hoạt. Cấu hình EventBridge Rule bắt sự kiện từ Model Registry. Thiết lập Amazon CloudFront Distribution trỏ về API Gateway và bật AWS WAF (Rate Limiting 100 requests/5 phút). Tích hợp CloudWatch Alarm và SNS Email Alert.

### 5. Lộ trình & Mốc triển khai  
- Tuần 4: Nhóm cùng tìm hiểu khái niệm MLOps, nghiên cứu các thành phần SageMaker và phân tích dataset Telco Churn.  
- Tuần 5: Thiết kế kiến trúc giải pháp hoàn chỉnh, vẽ sơ đồ kiến trúc và viết Proposal.  
- Tuần 6: Xây dựng SageMaker Pipeline hoàn chỉnh (Processing, HPO, Evaluation, ConditionStep AUC >= 0.80, Register).  
- Tuần 7: Triển khai EventBridge + Lambda Auto-Deploy, xây dựng Inference API và End-to-End Testing.  
- Tuần 8: Dọn dẹp tài nguyên AWS, viết blog kỹ thuật, hoàn thiện báo cáo thực tập.   

### 6. Ước tính ngân sách  
- **AWS Lambda & Amazon EventBridge:** 0,00 USD/tháng (Thuộc Free Tier).  
- **Amazon S3:** ~$0.12/tháng (~5 GB bao gồm Artifacts & Data).  
- **AWS SageMaker Processing & Training:** ~$0.35/tháng (instance ml.m5.large).  
- **AWS SageMaker Hyperparameter Tuning:** ~$0.80/tháng (6 Tuning Jobs song song ml.m5.large).  
- **AWS SageMaker Serverless Endpoint:** ~$1.20/tháng (2048 MB Memory, ~10,000 requests/tháng).  
- **Amazon CloudFront & AWS WAF:** Chi phí tùy thuộc lưu lượng thực tế (Nằm trong Free Tier cho 1TB CloudFront data transfer, WAF thử nghiệm ngắn hạn ~$0.00 - $15.00/tháng tùy theo quy mô cấu hình).  
- **Amazon CloudWatch & SNS:** ~$0.10/tháng.  
 

**Tổng:** ~$2.57 - $4.00 USD/tháng (Chi phí vận hành cơ bản chưa tính scale WAF lớn)
 

### 7. Đánh giá rủi ro  
#### Ma trận rủi ro  
- Hiện tượng Model Performance Drift: Ảnh hưởng cao, xác suất trung bình.  
- Tấn công từ chối dịch vụ (DDoS) hoặc spam request làm quá tải API: Ảnh hưởng cao, xác suất trung bình.  
- Phát sinh chi phí tài nguyên không kiểm soát: Ảnh hưởng trung bình, xác suất thấp.  

#### Chiến lược giảm thiểu      
- Hiệu năng mô hình: Đặt bước kiểm tra điều kiện (ConditionStep) trong Pipeline. Nếu mô hình mới có AUC < 0.80, Pipeline sẽ kích hoạt FailStep và dừng ngay việc đăng ký vào Registry.  
- An toàn API: Sử dụng AWS WAF với Rate Limiting (chặn nếu vượt quá 100 requests/IP/5 phút) đứng sau CloudFront để bảo vệ hệ thống backend khỏi request rác.  
- Kiểm soát chi phí: Sử dụng Serverless Endpoint (chỉ tốn tiền khi invocation xảy ra, không tính phí chờ idle). Đặt AWS Budgets Alarm cảnh báo khi chi phí vượt quá $10 USD/tháng.  


### 8. Kết quả kỳ vọng  
- Về kỹ thuật: Triển khai thành công quy trình MLOps khép kín tự động 100%: Data Upload => Drift Check => Pipeline => Model Registry => Auto-Deploy => Serverless Endpoint => CloudFront (WAF).
- Về vận hành & An toàn: Giảm 95% công sức thao tác thủ công của Data/MLOps Engineer khi triển khai phiên bản mô hình mới, đồng thời đảm bảo API thời gian thực luôn an toàn và sẵn sàng phục vụ các yêu cầu từ Client.