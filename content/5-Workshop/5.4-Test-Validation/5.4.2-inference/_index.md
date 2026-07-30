---
title : "Test Real-time Inference Flow"
date : 2026-07-29
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

#### 1. Prepare Input Data Payload (JSON)
Create payload.json file containing features of a telecom customer needing Churn probability prediction:
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

#### 2. Execute Request via cURL / Postman
Open your Terminal and send a POST request to the secured CloudFront Domain URL:
```powershell
curl.exe -X POST https://d1jj1dyq01crgf.cloudfront.net/predict \
         -H "Content-Type: application/json" \
         -d "@payload.json"
```

#### 3. Analyze Returned Results (API Response)
Successful result (HTTP 200 OK):

![inference](/images/5-Workshop/5.4-Test-Validation/inference.png)


#### 4. Inspect Logs & Metrics on Amazon CloudWatch
- Access CloudWatch => Log groups => open logs for /aws/lambda/telco-churn-api-handler
   ![api-logs](/images/5-Workshop/5.4-Test-Validation/api-logs.png)
- Init Duration: 442.40 ms: 
  - This is the execution environment initialization time (Cold Start) for the first time Lambda runs
- Duration: 6746.15 ms (Billed Duration: 7189 ms):
  - Taking ~6.7 seconds for the first call is because SageMaker Serverless Endpoint just underwent Cold Start (re-initializing container housing XGBoost model in a few seconds). Subsequent curl requests once Endpoint has "warmed up" will drop to a few dozen to a few hundred milliseconds.