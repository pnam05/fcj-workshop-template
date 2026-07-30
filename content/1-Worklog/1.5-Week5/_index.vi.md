---
title: "Worklog Tuần 5"
date: 2026-07-13
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Đào sâu kiến thức AWS Cloud về Quản lý Mô hình tập trung (Model Governance), SageMaker Model Registry, khái niệm Quality Gate (Condition Steps) và các cơ chế quản lý vòng đời phần mềm trên Cloud.
* Kết nối trọn vẹn 4 bước thành một quy trình MLOps khép kín (SageMaker Pipeline): Processing $\rightarrow$ HPO $\rightarrow$ Evaluation $\rightarrow$ Condition Check.
* Tự động hóa khâu đánh giá chất lượng mô hình với điều kiện $AUC \ge 0.80$, tự động đăng ký mô hình đạt chuẩn vào Model Registry với trạng thái Approved.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Học nâng cao về **Model Governance & Versioning** trên AWS: <br>&emsp; + Tìm hiểu vai trò của **SageMaker Model Registry** trong môi trường Production <br>&emsp; + Quản lý Model Package Groups, phiên bản mô hình (Model Versions) và các trạng thái phê duyệt (PendingManualApproval, Approved, Rejected) <br>&emsp; + Khái niệm Event-Driven Architecture cơ bản khi chuyển đổi trạng thái tài nguyên trên Cloud | 13/07/2026 | 13/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Thiết lập bước kiểm tra điều kiện chất lượng mô hình (ConditionStep): <br>&emsp; + Đọc giá trị AUC từ file evaluation.json bằng JsonGet <br>&emsp; + Đặt ngưỡng so sánh ConditionGreaterThanOrEqualTo với giá trị $0.80$ <br>&emsp; + Cấu hình nhánh if_steps (gọi RegisterModel) và else_steps (gọi FailStep) | 14/07/2026 | 14/07/2026 | SageMaker SDK Documentation |
| 4 | - Khởi tạo đối tượng RegisterModel để tự động đăng ký mô hình xuất sắc nhất từ bước HPO vào Model Package Group TelcoChurnModelGroup với trạng thái ban đầu là Approved <br> - Cấu hình FailStep (TelcoChurnAUCFailStep) để dừng ngay Pipeline và báo lỗi nếu mô hình không đạt ngưỡng AUC $0.80$ | 15/07/2026 | 15/07/2026 | AWS MLOps Best Practices |
| 5 | - Tổng hợp và kết nối 4 bước thành Pipeline hoàn chỉnh (TelcoChurnPipeline): <br>&emsp; 1. TelcoChurnProcessStep <br>&emsp; 2. TelcoChurnHpoStep <br>&emsp; 3. TelcoChurnEvalStep <br>&emsp; 4. TelcoChurnCheckAUCThreshold <br> - Đẩy định nghĩa Pipeline lên SageMaker service bằng phương thức pipeline.upsert() và khởi chạy thử nghiệm bằng pipeline.start() | 16/07/2026 | 16/07/2026 | AWS Hands-on Labs |
| 6 | - Theo dõi đồ thị thực thi của TelcoChurnPipeline trên giao diện **SageMaker Studio / Pipelines Console** <br> - Kiểm tra kết quả thực thi: Xác nhận cả 4 bước đều báo xanh (Succeeded), luồng ConditionStep rẽ sang nhánh True và mô hình mới tự động xuất hiện trong **Model Registry** ở trạng thái Approved | 17/07/2026 | 17/07/2026 | SageMaker Pipelines Console |

### Kết quả đạt được tuần 5:

* Nắm vững khái niệm MLOps Governance trên AWS Cloud, hiểu cách quản lý phiên bản mô hình tập trung và cơ chế kiểm soát chất lượng tự động trước khi triển khai.
* Xây dựng và upsert thành công **SageMaker Pipeline 4 bước hoàn chỉnh** (TelcoChurnPipeline) vận hành hoàn toàn tự động trên AWS.
* Tích hợp thành công **Quality Gate (ConditionStep)**: Tự động chặn các mô hình kém chất lượng ($AUC < 0.80$) và kích hoạt FailStep.
* Đăng ký tự động mô hình XGBoost đạt chuẩn ($AUC \approx 0.84$) vào **SageMaker Model Registry** (TelcoChurnModelGroup) với trạng thái **Approved**, sẵn sàng làm đầu vào cho luồng Tự động triển khai (Continuous Deployment) ở các tuần tiếp theo.