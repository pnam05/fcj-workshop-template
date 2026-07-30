---
title: "Worklog Tuần 3"
date: 2026-06-22
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Đào sâu kiến thức AWS Cloud về hệ sinh thái Machine Learning Managed Services: Tìm hiểu Amazon ECR, Amazon SageMaker Estimators và cơ chế vận hành của SageMaker Training Jobs.
* Nghiên cứu cơ chế tối ưu siêu tham số tự động với SageMaker Hyperparameter Tuning (HPO).
* Tìm hiểu cơ chế tiền xử lý dữ liệu với SageMaker Processing Jobs và quản lý log tập trung qua Amazon CloudWatch Logs.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo                                                               |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------------------------------------------- |
| 2   | - Tìm hiểu dịch vụ Amazon Elastic Container Registry (ECR): <br>&emsp; + Quản lý Docker Container Images, ECR Public vs Private Registries <br>&emsp; + Tìm hiểu cơ chế SageMaker kéo Built-in Containers từ ECR cho các thuật toán chuẩn                | 22/06/2026   | 22/06/2026      | <https://docs.aws.amazon.com/ecr/>                                            |
| 3   | - Nghiên cứu kiến trúc Amazon SageMaker Training Jobs: <br>&emsp; + Cấu hình Training Instances, S3 Data Input/Output Channels <br>&emsp; + Tìm hiểu cấu hình SageMaker Estimator SDK cho các bài toán phân loại và dự đoán                              | 23/06/2026   | 23/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-training.html>  |
| 4   | - Tìm hiểu cơ chế SageMaker Hyperparameter Tuning (HPO): <br>&emsp; + Các chiến lược tìm kiếm siêu tham số: Bayesian Optimization, Random Search, Hyperband <br>&emsp; + Cấu hình HyperparameterRanges (Continuous, Integer, Categorical Ranges)         | 24/06/2026   | 24/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning.html> |
| 5   | - Tìm hiểu khái niệm SageMaker Processing Jobs: <br>&emsp; + Khái niệm SKLearnProcessor, ScriptProcessor trong SageMaker Python SDK <br>&emsp; + Cấu hình ProcessingInput (trỏ S3) và ProcessingOutput (trỏ S3) cho các bài toán xử lý dữ liệu lớn       | 25/06/2026   | 25/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/processing-job.html>         |
| 6   | - Học cách tích hợp Amazon CloudWatch Logs cho SageMaker Jobs: <br>&emsp; + Tìm hiểu cách SageMaker tự động đẩy stdout/stderr từ container về CloudWatch Logs <br>&emsp; + Đọc và phân tích logs để phát hiện sự cố lỗi thực thi trong các container job | 26/06/2026   | 26/06/2026      | <https://docs.aws.amazon.com/sagemaker/latest/dg/logging-cloudwatch.html>     |

### Kết quả đạt được tuần 3:

* Nắm vững cơ chế lưu trữ và kéo container image từ Amazon ECR sang các môi trường thực thi Managed của SageMaker.
* Hiểu rõ kiến trúc vận hành của SageMaker Training Jobs và cách truyền dữ liệu từ Amazon S3 vào container instances.
* Làm chủ nguyên lý tối ưu siêu tham số tự động HPO và thiết lập không gian tìm kiếm HyperparameterRanges.
* Hiểu cách đóng gói các tác vụ xử lý dữ liệu với SKLearnProcessor và theo dõi log tập trung trên CloudWatch Logs.