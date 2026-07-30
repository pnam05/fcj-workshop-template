---
title : "Cấu hình AWS Lambda Check Data Drift & Kích hoạt Retrain"
date : 2026-07-27 
weight : 4
chapter : false
pre : " <b> 5.3.4 </b> "
---

Hàm Lambda này được đính kèm với sự kiện S3 Event Notification (ObjectCreated). Mỗi khi Admin upload một file dữ liệu CSV mới vào folder raw/, Lambda sẽ tự động chạy để kiểm tra Data Drift, gửi thông báo qua Amazon SNS và gọi pipeline.start() để bắt đầu Retrain.

#### Lập trình Hàm Lambda (TelcoChurnDriftChecker)
- Vào AWS Lambda => chọn Create function.
- Function name: TelcoChurnDriftChecker | Runtime: Python 3.11.
- Gán Role có các quyền: AmazonS3ReadOnlyAccess, AmazonSageMakerFullAccess, AmazonSNSFullAccess.
- Trong tab Configuration => General configuration, chỉnh Timeout thành 2 min 0 sec.
- Điều chỉnh Environment variables và Layers như bên dưới.
![ev](/images/5-Workshop/5.3-Implementation/ev.png)
![layers](/images/5-Workshop/5.3-Implementation/layers.png)
- Dán đoạn code dưới đây vào lambda_function.py và bấm Deploy:

```python
import os
import json
import urllib.parse
import io
import boto3
import numpy as np
import pandas as pd

s3_client = boto3.client('s3')
sagemaker_client = boto3.client('sagemaker')
sns_client = boto3.client('sns')

BUCKET_NAME = os.environ.get('BUCKET_NAME', 'telco-churn-mlops-fcaj')
PIPELINE_NAME = os.environ.get('PIPELINE_NAME', 'TelcoChurnMLOpsPipeline')
SNS_TOPIC_ARN = os.environ.get('SNS_TOPIC_ARN', 'arn:aws:sns:ap-southeast-1:606754308917:Telco-Churn-Alarm-Topic')

def calculate_drift_ks_test(df_ref, df_curr, threshold=0.15):
    """
    Tính khoảng cách Kolmogorov-Smirnov (KS-Statistic) giữa 2 phân phối bằng NumPy.
    Nếu khoảng cách d_stat >= 0.15 -> Đặc trưng đó bị trôi lệch (Drifted).
    """
    drifted_features = []
    numeric_cols = df_ref.select_dtypes(include=[np.number]).columns
    
    for col in numeric_cols:
        if col in df_curr.columns:
            ref_data = df_ref[col].dropna().values
            curr_data = df_curr[col].dropna().values
            
            if len(ref_data) > 0 and len(curr_data) > 0:
                # Sắp xếp 2 mảng
                ref_sorted = np.sort(ref_data)
                curr_sorted = np.sort(curr_data)
                
                # Gom tất cả các giá trị duy nhất để tính ECDF
                data_all = np.concatenate([ref_sorted, curr_sorted])
                
                # Tính ECDF cho từng tập
                cdf_ref = np.searchsorted(ref_sorted, data_all, side='right') / len(ref_sorted)
                cdf_curr = np.searchsorted(curr_sorted, data_all, side='right') / len(curr_sorted)
                
                # Khoảng cách KS (Max Absolute Difference)
                d_stat = np.max(np.abs(cdf_ref - cdf_curr))
                
                # Nếu khoảng cách vượt ngưỡng (Ví dụ > 0.15) -> Phát hiện Drift
                if d_stat >= threshold:
                    drifted_features.append((col, round(float(d_stat), 4)))
                    
    drift_share = len(drifted_features) / len(numeric_cols) if len(numeric_cols) > 0 else 0
    return drift_share, drifted_features


def lambda_handler(event, context):
    try:
        record = event['Records'][0]
        new_file_key = urllib.parse.unquote_plus(record['s3']['object']['key'], encoding='utf-8')
        print(f" File MỚI vừa upload: s3://{BUCKET_NAME}/{new_file_key}")

        # Chỉ xử lý các file CSV nằm trong folder raw/
        if not new_file_key.startswith("raw/") or not new_file_key.endswith(".csv"):
            print(" File không thuộc thư mục raw/ hoặc không phải CSV, bỏ qua.")
            return {'statusCode': 200, 'body': 'Ignored non-raw/non-csv file.'}

        # 2. Liệt kê các file trong raw/ để tìm danh sách file CŨ
        response = s3_client.list_objects_v2(Bucket=BUCKET_NAME, Prefix="raw/")
        all_objects = response.get('Contents', [])

        old_csv_keys = [
            obj['Key'] for obj in all_objects 
            if obj['Key'].endswith('.csv') and obj['Key'] != new_file_key
        ]

        if not old_csv_keys:
            print(" Đây là file CSV đầu tiên trong raw/. Chưa có dữ liệu cũ để so sánh Drift.")
            return {'statusCode': 200, 'body': 'First file uploaded. No baseline to compare.'}

        print(f" Tìm thấy {len(old_csv_keys)} file CSV cũ làm Baseline: {old_csv_keys}")
        
        # 3. Đọc và GỘP TẤT CẢ các file CSV cũ thành 1 DataFrame Baseline duy nhất
        baseline_dfs = []
        for old_key in old_csv_keys:
            obj_old = s3_client.get_object(Bucket=BUCKET_NAME, Key=old_key)
            df_temp = pd.read_csv(io.BytesIO(obj_old['Body'].read()))
            baseline_dfs.append(df_temp)
            
        # Gộp tất cả dữ liệu lịch sử lại
        df_baseline = pd.concat(baseline_dfs, axis=0, ignore_index=True)
        print(f" Đã gộp tổng cộng {len(df_baseline)} dòng dữ liệu lịch sử làm Baseline.")

        # Đọc file MỚI vừa upload
        obj_new = s3_client.get_object(Bucket=BUCKET_NAME, Key=new_file_key)
        df_new = pd.read_csv(io.BytesIO(obj_new['Body'].read()))
        print(f" File mới có {len(df_new)} dòng dữ liệu.")

        # Preprocessing nhanh để chuẩn hóa cột dạng số
        for df in [df_baseline, df_new]:
            if 'customerID' in df.columns:
                df.drop(columns=['customerID'], inplace=True)
            if 'TotalCharges' in df.columns:
                df['TotalCharges'] = pd.to_numeric(df['TotalCharges'], errors='coerce')
                df['TotalCharges'].fillna(df['TotalCharges'].median(), inplace=True)

        # 4. Tính toán Data Drift với KS-Test
        drift_share, drifted_cols = calculate_drift_ks_test(df_baseline, df_new)
        print(f" Tỷ lệ đặc trưng bị Drift: {drift_share * 100:.2f}%")

        DRIFT_THRESHOLD = 0.25 # Ngưỡng 25% số lượng đặc trưng biến động
        
        if drift_share >= DRIFT_THRESHOLD:
            message = (
                f" CẢNH BÁO DATA DRIFT DỮ LIỆU CHURN!\n\n"
                f"- File mới upload: {new_file_key}\n"
                f"- Số lượng file Baseline cũ gộp lại: {len(old_csv_keys)} file ({len(df_baseline)} dòng)\n"
                f"- Tỷ lệ đặc trưng biến động: {drift_share * 100:.1f}% (Ngưỡng: {DRIFT_THRESHOLD * 100}%)\n"
                f"- Các cột biến động mạnh: {[col[0] for col in drifted_cols]}\n\n"
                f" Hệ thống đang TỰ ĐỘNG KÍCH HOẠT SageMaker Pipeline '{PIPELINE_NAME}'..."
            )
            print(message)

            if SNS_TOPIC_ARN:
                try:
                    sns_client.publish(
                        TopicArn=SNS_TOPIC_ARN,
                        Subject="[MLOps Alert] Phát hiện Data Drift - Tự động Retrain",
                        Message=message
                    )
                except Exception as sns_err:
                    print(f" Không thể gửi SNS Alert: {str(sns_err)}")

            # Kích hoạt SageMaker Pipeline
            execution = sagemaker_client.start_pipeline_execution(
                PipelineName=PIPELINE_NAME,
                PipelineExecutionDisplayName=f"AutoRetrain-Drift-{pd.Timestamp.now().strftime('%Y%m%d-%H%M')}"
            )

            return {
                'statusCode': 200,
                'body': json.dumps({
                    'status': 'Drifted',
                    'drift_share': drift_share,
                    'pipeline_execution_arn': execution['PipelineExecutionArn']
                })
            }
        else:
            message = (
                f" THÔNG BÁO DỮ LIỆU MỚI\n\n"
                f"- File mới upload: {new_file_key}\n"
                f"- Tỷ lệ biến động: {drift_share * 100:.1f}% (Dưới ngưỡng {DRIFT_THRESHOLD * 100}%)\n\n"
                f" Dữ liệu ổn định, KHÔNG CẦN retrain lại mô hình."
            )
            print(message)

            if SNS_TOPIC_ARN:
                try:
                    sns_client.publish(
                        TopicArn=SNS_TOPIC_ARN,
                        Subject="[MLOps Info] Dữ liệu mới ổn định - Không cần Retrain",
                        Message=message
                    )
                except Exception as sns_err:
                    print(f" Không thể gửi SNS Info: {str(sns_err)}")

            return {
                'statusCode': 200,
                'body': json.dumps({'status': 'No Drift', 'drift_share': drift_share})
            }

    except Exception as e:
        print(f" Lỗi Lambda execution: {str(e)}")
        return {'statusCode': 500, 'body': json.dumps({'error': str(e)})}
```

#### Gán S3 Event Notification kết nối với Lambda
Để khi S3 nhận file mới thì Lambda tự động chạy:
- Mở dịch vụ Amazon S3 => Click chọn Bucket telco-churn-mlops-fcaj.
- Chuyển sang tab Properties => Kéo xuống mục Event notifications => Bấm Create event notification.
![event-s3](/images/5-Workshop/5.3-Implementation/event-s3.png)
- Thiết lập:
  - Event name: NewRawCsvUploaded.
  - Prefix: raw/ (chỉ bắt sự kiện trong thư mục raw).
  - Suffix: .csv
  ![event-s3](/images/5-Workshop/5.3-Implementation/event-conf.png)
  - Event types: Tick chọn All object create events (s3:ObjectCreated:*).
    ![event-s3](/images/5-Workshop/5.3-Implementation/event-type.png)
- Tại phần Destination ở cuối trang:
  - Chọn Lambda function.
  - Lambda function: Chọn TelcoChurnDriftChecker.
- Bấm Save changes.