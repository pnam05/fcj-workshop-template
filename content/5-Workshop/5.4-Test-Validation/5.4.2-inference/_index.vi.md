---
title : "Kiểm thử Luồng Dự đoán Thời gian thực"
date : 2026-07-29
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

#### 1. Chuẩn bị Payload Dữ liệu Đầu vào (JSON)
Tạo file payload.json chứa các tính năng (features) của một khách hàng viễn thông cần dự đoán xác suất Churn:
```json
{
    "raw_data": {
        "gender": "Female",
        "SeniorCitizen": 1,
        "Partner": "Yes",
        "Dependents": "No",
        "tenure": 1,
        "PhoneService": "No",
        "MultipleLines": "No phone service",
        "InternetService": "DSL",
        "OnlineSecurity": "No",
        "OnlineBackup": "Yes",
        "DeviceProtection": "No",
        "TechSupport": "No",
        "StreamingTV": "No",
        "StreamingMovies": "No",
        "Contract": "Month-to-month",
        "PaperlessBilling": "Yes",
        "PaymentMethod": "Electronic check",
        "MonthlyCharges": 29.85,
        "TotalCharges": 29.85
    }
}
```

#### 2. Thực thi Request qua cURL / Postman
Mở Terminal và gửi yêu cầu POST tới CloudFront Domain URL đã tích hợp bảo mật:
```powershell
curl.exe -X POST https://d1jj1dyq01crgf.cloudfront.net/predict \
         -H "Content-Type: application/json" \
         -d "@payload.json"
```

#### 3. Phân tích Kết quả Trả về (API Response)
Kết quả thành công (HTTP 200 OK):

![inference](/images/5-Workshop/5.4-Test-Validation/inference.png)


#### 4. Kiểm tra Logs & Metrics trên Amazon CloudWatch
- Truy cập CloudWatch $\rightarrow$ Log groups $\rightarrow$ mở log của /aws/lambda/telco-churn-api-handler
   ![api-logs](/images/5-Workshop/5.4-Test-Validation/api-logs.png)
- Init Duration: 442.40 ms: 
  - Đây là thời gian khởi tạo môi trường thực thi (Cold Start) lần đầu tiên của Lambda
- Duration: 6746.15 ms (Billed Duration: 7189 ms):
  - Mất ~6.7 giây cho lần gọi đầu tiên là do SageMaker Serverless Endpoint vừa mới trải qua đợt Cold Start (khởi tạo lại container chứa model XGBoost trong vài giây). Các lượt curl tiếp theo khi Endpoint đã "warm-up" sẽ giảm xuống chỉ còn vài chục đến vài trăm miligiây.

