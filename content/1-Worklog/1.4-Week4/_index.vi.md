---
title: "Worklog Tuần 4"
date: 2026-06-29
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Cùng các thành viên trong nhóm tìm hiểu định hướng MLOps và phân tích chuyên sâu tập dữ liệu Telco Customer Churn.
* Học cá nhân về các phương pháp chuẩn hóa & mã hóa dữ liệu (Data Preprocessing) trong môi trường SageMaker Notebooks.
* Học cá nhân về thuật toán XGBoost trên AWS SageMaker và các chỉ số đánh giá mô hình phân loại (ROC-AUC, Precision, Recall).

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                  | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo                                                                                |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ---------------------------------------------------------------------------------------------- |
| 2   | - Học kỹ thuật tiền xử lý dữ liệu và mã hóa biến (One-Hot Encoding, Label Encoding) trong môi trường Cloud Notebooks <br> - Tìm hiểu cách chuẩn hóa kiểu dữ liệu CSV theo định dạng chuẩn của SageMaker Built-in Algorithms                                | 29/06/2026   | 29/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/numpytensormulticlass.html>                   |
| 3   | - Nghiên cứu thuật toán XGBoost trên SageMaker: <br>&emsp; + Phân tích các tham số huấn luyện cốt lõi: objective='binary:logistic', eval_metric='auc' <br>&emsp; + Ý nghĩa của các siêu tham số max_depth, eta, min_child_weight, subsample                | 30/06/2026   | 30/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html>                                 |
| 4   | - Cùng nhóm tìm hiểu MLOps: quy trình End-to-End, các thành phần SageMaker (Studio, Processing Jobs, Training Jobs, Pipelines) <br> - Cùng nhóm phân tích dataset Telco Churn: cấu trúc features, thống kê phân phối, xác định bài toán phân loại nhị phân | 01/07/2026   | 01/07/2026      | [SageMaker Developer Guide](https://docs.aws.amazon.com/sagemaker/latest/dg/)                  |
| 5   | - Tìm hiểu nguyên lý phân chia tập dữ liệu (Train/Validation/Test Split) chuẩn hóa trên Cloud cho các bài toán phân loại nhị phân <br> - Nghiên cứu cơ chế nén và định dạng dữ liệu đầu vào phù hợp với SageMaker Estimators                               | 02/07/2026   | 02/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-training.html>                            |
| 6   | - Học cách đánh giá hiệu năng mô hình phân loại nhị phân: <br>&emsp; + Các chỉ số ROC-AUC Score, Confusion Matrix, Accuracy, Precision, Recall <br>&emsp; + Thiết lập ngưỡng Quality Gate tiêu chuẩn cho các mô hình Machine Learning trên Cloud           | 03/07/2026   | 03/07/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-model-monitor-model-performance.html> |

### Kết quả đạt được tuần 4:

* Cùng nhóm hiểu rõ quy trình MLOps End-to-End: từ data ingestion => preprocessing => training => evaluation => model registry => deployment => monitoring. Phân tích xong dataset Telco Churn.

* Master kỹ thuật tiền xử lý dữ liệu và mã hóa One-Hot Encoding chuẩn hóa cho SageMaker.
* Nắm vững các tham số cốt lõi và phương pháp huấn luyện thuật toán XGBoost trên AWS.
* Hiểu rõ nguyên lý đánh giá hiệu năng mô hình phân loại nhị phân qua chỉ số ROC-AUC và cách thiết lập Quality Gate.