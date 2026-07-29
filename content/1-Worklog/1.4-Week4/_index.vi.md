---
title: "Worklog Tuần 4"
date: 2026-07-06
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Đào sâu kiến thức AWS Cloud về hệ sinh thái Machine Learning Managed Services: Tìm hiểu cơ chế quản lý Container images trên ECR, SageMaker Estimators, Hyperparameter Tuning Jobs và tích hợp Amazon CloudWatch Logs để debug jobs.
* Xây dựng và đóng gói bước Huấn luyện & Tối ưu siêu tham số tự động (`TelcoChurnHpoStep`) sử dụng `HyperparameterTuner` với thuật toán XGBoost.
* Viết script đánh giá mô hình `evaluate.py` và đóng gói bước Đánh giá hiệu năng (`TelcoChurnEvalStep`), trích xuất chỉ số ROC-AUC ra file `evaluation.json`.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Học nâng cao về các dịch vụ Machine Learning & Analytics trên AWS: <br>&emsp; + Kiến trúc SageMaker Training Jobs & cơ chế kéo Built-in Container từ Amazon ECR <br>&emsp; + Các chiến lược Tối ưu siêu tham số (Random, Bayesian, Hyperband Search) <br>&emsp; + Quản lý Log tập trung cho các Training Jobs với Amazon CloudWatch Logs <br> - **Thực hành:** Kiểm tra ECR Image URI của XGBoost v1.5-1 tại region `ap-southeast-1` | 06/07/2026 | 06/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Khởi tạo đối tượng `Estimator` cho XGBoost với các siêu tham số cố định (`objective='binary:logistic'`, `eval_metric='auc'`) <br> - Thiết lập dải không gian tìm kiếm siêu tham số (`HyperparameterRanges`): `max_depth` (3-10), `eta` (0.01-0.2), `min_child_weight` (1-6) <br> - Đóng gói vào `HyperparameterTuner` (chạy 6 jobs song song 2 jobs trên `ml.m5.large`) và tạo `TuningStep` (`TelcoChurnHpoStep`) | 07/07/2026 | 07/07/2026 | SageMaker SDK Documentation |
| 4 | - Chạy thử nghiệm HPO Job độc lập để kiểm tra khả năng đọc dữ liệu từ `s3://.../processed/train` và `s3://.../processed/validation` <br> - Giám sát tiến độ Tuning Jobs và đọc metrics `validation:auc` trực tiếp trên SageMaker Console & CloudWatch Logs | 08/07/2026 | 08/07/2026 | AWS SageMaker Console |
| 5 | - Lập trình file script Python `evaluate.py`: <br>&emsp; + Tự động giải nén file `model.tar.gz` tốt nhất từ HPO Job <br>&emsp; + Tải tập dữ liệu test từ `s3://.../processed/test/test.csv` <br>&emsp; + Dự đoán xác suất Churn và tính chỉ số ROC-AUC Score <br>&emsp; + Đóng gói kết quả AUC vào định dạng JSON tiêu chuẩn (`evaluation.json`) | 09/07/2026 | 09/07/2026 | Scikit-Learn / XGBoost Docs |
| 6 | - Tạo đối tượng `ScriptProcessor` đóng gói file `evaluate.py` thành `TelcoChurnEvalStep` <br> - Thiết lập `PropertyFile` (`evaluation.json`) để trích xuất chỉ số AUC phục vụ cho bước kiểm tra điều kiện (Condition Gate) ở tuần sau <br> - Kiểm thử thành công luồng chạy nối tiếp: `ProcessingStep` $\rightarrow$ `TuningStep` $\rightarrow$ `EvalStep` | 10/07/2026 | 10/07/2026 | AWS Hands-on Labs |

### Kết quả đạt được tuần 4:

* Nắm vững cơ chế vận hành của SageMaker Training/HPO Jobs, hiểu cách SageMaker tự động pull Docker Container từ Amazon ECR và đẩy logs về CloudWatch Logs.
* Xây dựng thành công `TelcoChurnHpoStep`:
  * Tự động thực thi 6 đợt huấn luyện tối ưu siêu tham số song song.
  * Tìm ra bộ tham số XGBoost tối ưu cho tập dữ liệu Telco Churn, xuất file mô hình `model.tar.gz` lưu trữ an toàn tại `s3://telco-churn-mlops-fcaj/models/`.
* Xây dựng hoàn chỉnh script `evaluate.py` và đóng gói thành `TelcoChurnEvalStep` bằng `ScriptProcessor`.
* Trích xuất thành công file `evaluation.json` chứa giá trị Test AUC (~0.84), sẵn sàng làm đầu vào cho `ConditionStep` để đánh giá mô hình tự động.