---
title: "Worklog Tuần 8"
date: 2026-08-03
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Đào sâu kiến thức AWS Cloud về Tối ưu hóa Chi phí & Vận hành xuất sắc (AWS Well-Architected Framework & Cost Optimization): Học cách phân tích chi phí qua AWS Cost Explorer, đặt AWS Budgets và tối ưu tài nguyên Serverless.
* Thực hành kiểm thử toàn diện toàn bộ hệ thống (End-to-End Testing & Validation Matrix) cho cả 2 luồng Auto-Retrain và Real-time Inference API.
* Hoàn thiện tài liệu Workshop song ngữ (Việt - Anh), nghiệm thu báo cáo thực tập với Mentor và thực hiện dọn dẹp (Clean-up) toàn bộ tài nguyên trên AWS Cloud.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Học nâng cao về **AWS Well-Architected Framework** (5 trụ cột: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization) <br> - Phân tích chi phí vận hành hệ thống qua **AWS Cost Explorer** và thiết lập **AWS Budgets Alarm** cảnh báo khi chi phí vượt $10 USD/tháng <br> - Đánh giá các giải pháp tối ưu chi phí Serverless (Auto-scaling, Concurrency limits) | 03/08/2026 | 03/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - **Kiểm thử End-to-End Kịch bản 1 (Automated Retrain Flow):** <br>&emsp; + Giả lập Admin upload file dữ liệu mới vào `s3://.../raw/` <br>&emsp; + Xác nhận Lambda Drift Checker kích hoạt SageMaker Pipeline <br>&emsp; + Kiểm tra Pipeline 4 bước chạy báo xanh, gửi Email SNS thông báo `Succeeded` <br>&emsp; + Kiểm tra EventBridge kích hoạt Lambda Deployer tự động cập nhật Serverless Endpoint | 04/08/2026 | 04/08/2026 | AWS Console Testing |
| 4 | - **Kiểm thử End-to-End Kịch bản 2 (Real-time Inference Flow):** <br>&emsp; + Gửi nhiều mẫu payload JSON kiểm thử qua API Gateway HTTP Endpoint bằng cURL/Postman <br>&emsp; + Kiểm tra các trường hợp dự đoán Churn (`Yes`/`No`) và đánh giá độ trễ (Latency) <br>&emsp; + Lập bảng Tổng kết Kết quả Kiểm thử (**Validation Matrix**) đầy đủ các tiêu chí | 05/08/2026 | 05/08/2026 | Postman / cURL Tests |
| 5 | - Rà soát và hoàn thiện toàn bộ tài liệu báo cáo thực tập & tài liệu Workshop trên website Hugo: <br>&emsp; + Kiểm tra tính đầy đủ của cả 2 ngôn ngữ (**Tiếng Việt** và **Tiếng Anh**) <br>&emsp; + Bổ sung sơ đồ kiến trúc, hình ảnh bằng chứng (Proof of Work) từ CloudWatch Logs và cURL response <br> - Báo cáo nghiệm thu kết quả dự án cá nhân với Mentor | 06/08/2026 | 06/08/2026 | Website Template FCAJ |
| 6 | - **Thực hiện Dọn dẹp Tài nguyên (Clean-up):** <br>&emsp; + Xóa SageMaker Serverless Endpoint & Configurations <br>&emsp; + Làm rỗng và xóa S3 Data Lake Bucket <br>&emsp; + Xóa các hàm AWS Lambda, API Gateway, EventBridge Rules & SNS Topic <br> - Tổng kết kỳ thực tập và đóng Worklog | 07/08/2026 | 07/08/2026 | Workshop Clean-up Guide |

### Kết quả đạt được tuần 8:

* Nắm vững các nguyên tắc cốt lõi của **AWS Well-Architected Framework**, biết cách phân tích và tối ưu hóa chi phí hạ tầng Cloud bằng AWS Budgets và Cost Explorer (duy trì tổng chi phí dự án dưới $5 USD/tháng).
* Hoàn thành kiểm thử End-to-End xuất sắc 100% các kịch bản:
  * Chuỗi sự kiện tự động hóa Retrain và Auto-Deploy hoạt động chính xác không lỗi.
  * API Gateway trả về kết quả suy luận Real-time với độ trễ thấp (~45ms - 80ms sau Cold Start) và phản hồi chuẩn xác.
* Đã xuất bản hoàn chỉnh tài liệu Workshop song ngữ (Việt - Anh) trên website báo cáo thực tập cá nhân với đầy đủ hình ảnh chứng minh và hướng dẫn từng bước.
* Đã nghiệm thu dự án thành công với Mentor và thực hiện Clean-up toàn bộ tài nguyên trên AWS, đảm bảo tài khoản không phát sinh chi phí ngoài ý muốn.