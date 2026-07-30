---
title : "Cấu hình Real-time Inference API (API Gateway & Predict Lambda)"
date : 2026-07-27 
weight : 6
chapter : false
pre : " <b> 5.3.6 </b> "
---
Xây dựng cổng giao tiếp HTTPS công khai cho ứng dụng Client gửi request dự đoán.
#### Tạo Lambda Predict Handler (telco-churn-api-handler)

- Tạo hàm Lambda mới tên telco-churn-api-handler.
- Dán đoạn code xử lý tiền xử lý request real-time và invoke Serverless Endpoint:
![inline-policy](/images/5-Workshop/5.3-Implementation/inline-policy.png)
```python
import os
import json
import boto3

sagemaker_runtime = boto3.client('sagemaker-runtime')
ENDPOINT_NAME = os.environ.get('ENDPOINT_NAME', 'telco-churn-serverless-endpoint')

def preprocess_raw_input(raw_json):

    senior_citizen = int(raw_json.get('SeniorCitizen', 0))
    tenure = float(raw_json.get('tenure') or 0)
    monthly_charges = float(raw_json.get('MonthlyCharges') or 0.0)
    
    try:
        total_charges = float(raw_json.get('TotalCharges'))
    except (ValueError, TypeError):
        total_charges = monthly_charges 

    gender_male = 1 if raw_json.get('gender') == 'Male' else 0
    partner_yes = 1 if raw_json.get('Partner') == 'Yes' else 0
    dependents_yes = 1 if raw_json.get('Dependents') == 'Yes' else 0
    phone_service_yes = 1 if raw_json.get('PhoneService') == 'Yes' else 0

    mlines = raw_json.get('MultipleLines', 'No')
    mlines_no_phone = 1 if mlines == 'No phone service' else 0
    mlines_yes = 1 if mlines == 'Yes' else 0

    inet = raw_json.get('InternetService', 'DSL')
    inet_fiber = 1 if inet == 'Fiber optic' else 0
    inet_no = 1 if inet == 'No' else 0

    osec = raw_json.get('OnlineSecurity', 'No')
    osec_no_inet = 1 if osec == 'No internet service' else 0
    osec_yes = 1 if osec == 'Yes' else 0

    obackup = raw_json.get('OnlineBackup', 'No')
    obackup_no_inet = 1 if obackup == 'No internet service' else 0
    obackup_yes = 1 if obackup == 'Yes' else 0

    dprot = raw_json.get('DeviceProtection', 'No')
    dprot_no_inet = 1 if dprot == 'No internet service' else 0
    dprot_yes = 1 if dprot == 'Yes' else 0

    tsup = raw_json.get('TechSupport', 'No')
    tsup_no_inet = 1 if tsup == 'No internet service' else 0
    tsup_yes = 1 if tsup == 'Yes' else 0

    stv = raw_json.get('StreamingTV', 'No')
    stv_no_inet = 1 if stv == 'No internet service' else 0
    stv_yes = 1 if stv == 'Yes' else 0

    smov = raw_json.get('StreamingMovies', 'No')
    smov_no_inet = 1 if smov == 'No internet service' else 0
    smov_yes = 1 if smov == 'Yes' else 0

    contract = raw_json.get('Contract', 'Month-to-month')
    contract_1yr = 1 if contract == 'One year' else 0
    contract_2yr = 1 if contract == 'Two year' else 0

    paperless_yes = 1 if raw_json.get('PaperlessBilling') == 'Yes' else 0

    pmeth = raw_json.get('PaymentMethod', 'Bank transfer (automatic)')
    pmeth_credit = 1 if pmeth == 'Credit card (automatic)' else 0
    pmeth_echeck = 1 if pmeth == 'Electronic check' else 0
    pmeth_mcheck = 1 if pmeth == 'Mailed check' else 0

    features = [
        senior_citizen, tenure, monthly_charges, total_charges,
        gender_male, partner_yes, dependents_yes, phone_service_yes,
        mlines_no_phone, mlines_yes, inet_fiber, inet_no,
        osec_no_inet, osec_yes, obackup_no_inet, obackup_yes,
        dprot_no_inet, dprot_yes, tsup_no_inet, tsup_yes,
        stv_no_inet, stv_yes, smov_no_inet, smov_yes,
        contract_1yr, contract_2yr, paperless_yes,
        pmeth_credit, pmeth_echeck, pmeth_mcheck
    ]

    return ",".join(map(str, features))


def lambda_handler(event, context):
    try:
        if isinstance(event.get('body'), str):
            body = json.loads(event['body'])
        else:
            body = event.get('body', {})
            
        if 'raw_data' in body:
            payload = preprocess_raw_input(body['raw_data'])
        else:
            return {
                'statusCode': 400,
                'headers': {
                    'Content-Type': 'application/json',
                    'Access-Control-Allow-Origin': '*'
                },
                'body': json.dumps({'error': 'Cần truyền trường data (chuỗi CSV) hoặc raw_data (object JSON)'})
            }

        response = sagemaker_runtime.invoke_endpoint(
            EndpointName=ENDPOINT_NAME,
            ContentType='text/csv',
            Body=payload
        )
        
        result = float(response['Body'].read().decode('utf-8'))
        prediction = "CHURN" if result >= 0.5 else "RETAIN"
        
        return {
            'statusCode': 200,
            'headers': {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*' 
            },
            'body': json.dumps({
                'churn_probability': round(result, 4),
                'prediction': prediction
            })
        }
        
    except Exception as e:
        return {
            'statusCode': 500,
            'headers': {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*'
            },
            'body': json.dumps({'error': str(e)})
        }
```
#### Cấu hình Amazon API Gateway
- Vào API Gateway $\rightarrow$ Create API $\rightarrow$ Chọn HTTP API (Build)..
- API Name: telco-churn-api.
![api-name](/images/5-Workshop/5.3-Implementation/api-name.png)
- Tạo Resource /predict $\rightarrow$ Tạo Method POST.
- Integration type: Chọn Lambda Function $\rightarrow$ Chọn telco-churn-api.
![post-api](/images/5-Workshop/5.3-Implementation/post-api.png)
- Bấm Next và Deploy API.
- Sao chép đường dẫn Invoke URL dạng: https://<api-id>[.execute-api.ap-southeast-1.amazonaws.com/predict](https://c6kbjaktj9.execute-api.ap-southeast-1.amazonaws.com/predict)