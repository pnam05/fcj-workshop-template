# FCJ Workshop & AWS Internship Report

Trang báo cáo thực tập và workshop được xây dựng bằng **Hugo** với giao diện **Hugo Theme Learn**, tự động triển khai lên **GitHub Pages** thông qua **GitHub Actions**.

**Live Demo / Website Báo cáo:** [https://pnam05.github.io/fcj-workshop-template/](https://pnam05.github.io/fcj-workshop-template/)

---

## Cấu Trúc Nội Dung Báo Cáo (`content/`)

Trang web bao gồm các phần chính như sau:

1. **[Worklog](content/1-Worklog/):** Nhật ký công việc theo từng tuần trong suốt quá trình thực tập.
2. **[Proposal](content/2-Proposal/):** Đề xuất dự án / đề tài thực tập.
3. **[BlogsPosted](content/3-BlogsPosted/):** Danh sách các bài viết kỹ thuật, blog chia sẻ kiến thức đã đăng.
4. **[EventParticipated](content/4-EventParticipated/):** Các sự kiện, hội thảo, buổi training đã tham gia.
5. **[Workshop](content/5-Workshop/):** Hướng dẫn chi tiết các bài lab / workshop thực hành trên AWS.
6. **[Self-evaluation](content/6-Self-evaluation/):** Tự đánh giá bản thân sau kỳ thực tập.
7. **[Feedback](content/7-Feedback/):** Góp ý và chia sẻ cảm nhận về chương trình.


## Dự Án Thực Hiện: MLOps Platform for Telco Customer Churn Prediction

### 1. Tổng quan dự án
Dự án **Telco Customer Churn MLOps Platform** xây dựng quy trình MLOps khép kín (End-to-End MLOps Pipeline) tự động hóa toàn bộ vòng đời của mô hình Machine Learning: từ xử lý dữ liệu, huấn luyện/tối ưu siêu tham số (HPO), đánh giá chất lượng, đăng ký mô hình (Model Registry) cho tới tự động triển khai phục vụ suy luận thời gian thực (Serverless Inference).

### 2. Các dịch vụ AWS chính sử dụng
* **AWS SageMaker Pipelines:** Điều phối quy trình MLOps tự động 4 bước (Processing, Hyperparameter Tuning với XGBoost, Evaluation, Condition Step kiểm tra AUC >= 0.80).
* **AWS SageMaker Model Registry:** Quản lý các phiên bản mô hình và trạng thái phê duyệt (Approved).
* **Event-Driven Auto Deployment:** Sử dụng Amazon EventBridge kết hợp AWS Lambda Deployer tự động cập nhật AWS SageMaker Serverless Endpoint khi mô hình chuyển trạng thái Approved.
* **Bảo vệ API thời gian thực:** Amazon CloudFront kết hợp AWS WAF (Rate Limiting 100 requests/5 phút) bảo vệ Amazon API Gateway & AWS Lambda Inference trước nguy cơ tấn công DDoS/Layer 7.
* **Phát hiện Data Drift:** Amazon S3 kết hợp AWS Lambda & Amazon SNS tự động kiểm tra dữ liệu mới, gửi thông báo và kích hoạt pipeline retrain.
* **Giám sát & Cảnh báo:** Amazon CloudWatch lưu vết logs, cấu hình Alarms và phát thông báo qua Amazon SNS.
