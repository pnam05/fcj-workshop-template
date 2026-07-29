---
title: "Worklog Tuần 2"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Khám phá, làm sạch dữ liệu (EDA) và xây dựng mô hình thử nghiệm (Baseline Model XGBoost) cho bài toán Telco Customer Churn.
* Viết script tiền xử lý dữ liệu chuẩn hóa (`preprocessing.py`) để chuẩn bị tích hợp vào SageMaker Processing Step.
* Hoàn thiện bản Đề xuất Dự án (Proposal) chi tiết về kiến trúc MLOps Platform trình Mentor phê duyệt.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tải tập dữ liệu Telco Customer Churn (`WA_Fn-UseC_-Telco-Customer-Churn.csv`) về Jupyter Notebook local/colab <br> - Tiến hành Exploratory Data Analysis (EDA): Phân tích phân phối biến target `Churn`, xử lý giá trị thiếu (missing values) ở cột `TotalCharges` và mã hóa các biến định tính (Categorical Features) | 22/06/2026 | 22/06/2026 | Pandas / Scikit-Learn Docs |
| 3 | - Thử nghiệm huấn luyện mô hình dự đoán rời bỏ dịch vụ với thuật toán XGBoost <br> - Đánh giá hiệu năng mô hình baseline bằng các chỉ số Machine Learning: ROC-AUC, Accuracy, Precision, Recall <br> - Xác định threshold AUC tối thiểu cho Quality Gate ($AUC \ge 0.80$) | 23/06/2026 | 23/06/2026 | XGBoost Documentation |
| 4 | - Đóng gói logic tiền xử lý dữ liệu thành script Python độc lập `preprocessing.py` <br> - Xử lý ép kiểu dữ liệu, One-Hot Encoding, đưa cột target `Churn` lên đầu chuỗi (đúng chuẩn định dạng input của XGBoost) và chia tập dữ liệu thành Train (70%), Validation (15%), Test (15%) | 24/06/2026 | 24/06/2026 | SageMaker Python SDK |
| 5 | - Soạn thảo tài liệu Bản đề xuất dự án (**Proposal**) bằng Tiếng Việt và Tiếng Anh: <br>&emsp; + Tóm tắt điều hành & Tuyên bố bài toán <br>&emsp; + Kiến trúc giải pháp MLOps Event-Driven trên AWS <br>&emsp; + Danh sách dịch vụ AWS sử dụng (SageMaker, S3, Lambda, EventBridge, API Gateway, SNS, CloudWatch) <br>&emsp; + Ước tính ngân sách & Lộ trình triển khai 8 tuần | 25/06/2026 | 25/06/2026 | AWS MLOps Framework |
| 6 | - Kiểm thử chạy script `preprocessing.py` trên môi trường cục bộ để đảm bảo dữ liệu sau xuất ra dạng CSV tương thích với SageMaker <br> - Trình bày Proposal dự án với Mentor, tiếp thu nhận xét và cập nhật lại sơ đồ kiến trúc hệ thống MLOps | 26/06/2026 | 26/06/2026 | Feedback từ Mentor |

### Kết quả đạt được tuần 2:

* Complete phần phân tích dữ liệu EDA bài toán Telco Customer Churn: Phát hiện và xử lý thành công 11 giá trị trống ở cột `TotalCharges`, chuyển đổi biến `Churn` từ kiểu chuỗi (`Yes`/`No`) sang định dạng nhị phân (`1`/`0`).
* Huấn luyện thành công mô hình Baseline XGBoost trên tập dữ liệu đã làm sạch, đạt chỉ số **ROC-AUC ~0.84** (vượt ngưỡng mục tiêu 0.80).
* Xây dựng xong file script Python `preprocessing.py` hoàn chỉnh, hỗ trợ nhận tham số từ `SKLearnProcessor` của SageMaker để tự động chia dữ liệu thành 3 tập `train.csv`, `validation.csv`, và `test.csv`.
* Hoàn thiện và xuất bản file Proposal cho dự án **MLOps Platform for Telco Customer Churn Prediction** lên hệ thống báo cáo thực tập, sẵn sàng cho giai đoạn đóng gói tự động hóa trên AWS Cloud.