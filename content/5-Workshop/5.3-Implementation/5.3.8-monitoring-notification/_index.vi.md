---
title : "Cấu hình Monitoring & Notification (CloudWatch & SNS)"
date : 2026-07-27 
weight : 8
chapter : false
pre : " <b> 5.3.8 </b> "
---
#### Tạo topic SNS
- Vào Amazon SNS $\rightarrow$ Topics $\rightarrow$ Bấm Create topic tên TelcoChurnAlerts.
- Bấm Create subscription $\rightarrow$ Protocol: Email $\rightarrow$ Nhập địa chỉ Gmail của bạn $\rightarrow$ Xác nhận email trong hộp thư đến.
![sns](/images/5-Workshop/5.3-Implementation/sns.png)
#### Tạo alarm
- Trong giao diện **CloudWatch Console**, chuyển đến **Alarms** và chọn **Create alarm**.
- Nhấn nút **Select metric** màu cam bên dưới mục **Metric**.
![metric](/images/5-Workshop/5.3-Implementation/metric.png)
- Trong cửa sổ **Select metric** hiện ra, ở tab **Browse**, chọn **Lambda** $\rightarrow$ **By Function Name** $\rightarrow$ **telco-churn-api-handler** $\rightarrow$ **Select metric**.
![metric-name](/images/5-Workshop/5.3-Implementation/metric-name.png)
- Sau khi chọn metric, giao diện sẽ quay trở lại trang cấu hình. Kéo xuống mục **Conditions**:
   + **Threshold type**: Chọn **Static**
   + **Whenever Errors is...**: Chọn **Greater/Equal** (>= threshold)
   + **than...**: Nhập giá trị ngưỡng là 1
 ![cond](/images/5-Workshop/5.3-Implementation/cond.png)
- Tại mục **Notification**:
   + **Alarm state trigger**: Chọn **In alarm** (Thực thi hành động khi metric vượt ngưỡng cảnh báo).
   + **Send a notification to the following SNS topic**: Chọn **Select an existing SNS topic**.
   + **Send a notification to...**: Chọn SNS Topic đã tạo sẵn từ menu thả xuống (ví dụ: Telco-Churn-Alarm-Topic).
   +  Kiểm tra thông tin Email nhận thông báo hiển thị ở mục **Email (endpoints)** bên dưới (ví dụ: trungnam2682005@gmail.com).
   +  Kéo xuống cuối trang và nhấn **Next**.
    ![notice](/images/5-Workshop/5.3-Implementation/notice.png)
- Tại mục **Name and description**:
   + **Alarm name**: Nhập tên cho cảnh báo, ví dụ: `Telco-Churn-API-Error-Alarm`.
   + **Alarm description - optional**: Nhập mô tả cho cảnh báo nếu cần thiết.
   + Nhấn **Next** ở cuối trang.
    ![alarm-name](/images/5-Workshop/5.3-Implementation/alarm-name.png)