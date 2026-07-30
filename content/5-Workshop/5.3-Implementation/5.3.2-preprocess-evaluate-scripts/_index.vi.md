---
title : "Tạo Scripts Xử lý Dữ liệu & Đánh giá Mô hình"
date : 2026-07-29 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

#### Trong môi trường Jupyter Notebook (hoặc SageMaker Studio), tạo 2 file Python script để phục vụ các bước trong SageMaker Pipeline:

1. Trong giao diện **Amazon SageMaker AI Console**, ở menu bên trái dưới mục **Applications and IDEs**, chọn **Notebooks**.
2. Chọn tab **Notebook instances** và nhấn vào nút **Create notebook instance** màu cam.

![create](/images/5-Workshop/5.3-Implementation/create-notebook.png)

#### Cấu hình Notebook Instance

1. Trong trang **Create notebook instance**, tại mục **Notebook instance settings**, thiết lập các thông số như sau:
   + **Notebook instance name**: Nhập `telco-churn`
   + **Notebook instance type**: Chọn ml.t3.medium
   + **Platform identifier**: Chọn Amazon Linux 2023, Jupyter Lab 4

2. Kéo xuống mục **Permissions and encryption**:
   + **IAM role**: Chọn IAM Role SageMaker-Telco-Churn-Role đã tạo trước đó .
   + **Root access**: Chọn **Enable - Give users root access to the notebook**.
   + **Encryption key**: Giữ mặc định **No custom encryption**.

3. Kéo xuống cuối trang và nhấn **Create notebook instance** để khởi tạo.

![create](/images/5-Workshop/5.3-Implementation/conf-notebook.png)



#### Tạo file `preprocessing.py` (Dùng cho TelcoChurnProcessStep)

Script này thực hiện làm sạch dữ liệu, xử lý ép kiểu cột TotalCharges, One-Hot Encoding cho các biến phân loại và chia tập dữ liệu thành train, validation, test.

1. Trong môi trường Jupyter Notebook (hoặc SageMaker Studio), tạo file `preprocessing.py` với nội dung sau:

```python
import argparse
import os
import glob
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split

if __name__ == "__main__":
    # 1. Định nghĩa đường dẫn nhận/xuất dữ liệu
    input_dir = "/opt/ml/processing/input"
    output_dir = "/opt/ml/processing/output"

    all_files = glob.glob(os.path.join(input_dir, "*.csv"))

    if not all_files:
        raise FileNotFoundError(f" Không tìm thấy file CSV nào trong {input_dir}")

    # 2. Đọc và gộp TOÀN BỘ file CSV tìm thấy
    df_list = [pd.read_csv(file) for file in all_files]
    df = pd.concat(df_list, axis=0, ignore_index=True)
    print(f" Đã đọc {len(all_files)} file CSV. Kích thước dữ liệu gốc gộp lại: {df.shape}")

    # 3. Làm sạch dữ liệu Telco Churn
    # Bỏ cột customerID không có giá trị dự đoán
    if 'customerID' in df.columns:
        df = df.drop('customerID', axis=1)

    # Chuyển TotalCharges sang định dạng số 
    df['TotalCharges'] = pd.to_numeric(df['TotalCharges'], errors='coerce')
    df['TotalCharges'].fillna(df['TotalCharges'].median(), inplace=True)

    # Chuyển biến Target 'Churn' thành dạng số (Yes: 1, No: 0)
    if 'Churn' in df.columns:
        # Ép kiểu dữ liệu sang String và xóa khoảng trắng thừa
        df['Churn'] = df['Churn'].astype(str).str.strip()

        # Ánh xạ cả dạng Chữ (Yes/No) lẫn dạng Số ('1'/'0', 1/0)
        churn_map = {'Yes': 1, 'No': 0, '1': 1, '0': 0, '1.0': 1, '0.0': 0, 1: 1, 0: 0}
        df['Churn'] = df['Churn'].map(churn_map)

    # Loại bỏ bất kỳ dòng nào không có nhãn Churn (NaN) để train_test_split không bị lỗi
    df = df.dropna(subset=['Churn'])
    df['Churn'] = df['Churn'].astype(int)

    # One-Hot Encoding cho các biến phân loại (Categorical features)
    cat_cols = df.select_dtypes(include=['object']).columns.tolist()
    df = pd.get_dummies(df, columns=cat_cols, drop_first=True)

    # Đảm bảo tất cả các cột One-Hot dạng True/False được ép chuẩn sang 1/0
    for col in df.columns:
        if df[col].dtype == 'bool':
            df[col] = df[col].astype(int)

    # Đưa cột Churn lên ĐẦU TIÊN (bắt buộc cho XGBoost)
    churn_col = df.pop('Churn')
    df.insert(0, 'Churn', churn_col)

    print(f" Dữ liệu sau khi xử lý & One-Hot: {df.shape}")

    # 4. Chia tập Train / Validation / Test (Tỷ lệ 70% / 15% / 15%)
    train, test_val = train_test_split(df, test_size=0.3, random_state=42, stratify=df['Churn'])
    validation, test = train_test_split(test_val, test_size=0.5, random_state=42, stratify=test_val['Churn'])

    print(f" Train set: {train.shape}, Validation set: {validation.shape}, Test set: {test.shape}")

    # 5. Lưu ra các file CSV (Không chứa tiêu đề header và index)
    os.makedirs(os.path.join(output_dir, "train"), exist_ok=True)
    os.makedirs(os.path.join(output_dir, "validation"), exist_ok=True)
    os.makedirs(os.path.join(output_dir, "test"), exist_ok=True)

    train.to_csv(os.path.join(output_dir, "train", "train.csv"), index=False, header=False)
    validation.to_csv(os.path.join(output_dir, "validation", "validation.csv"), index=False, header=False)
    test.to_csv(os.path.join(output_dir, "test", "test.csv"), index=False, header=False)

    print(" Hoàn tất Processing Job và đã lưu các file vào thư mục output!")
```

{{% notice info %}}
Dữ liệu đầu ra sẽ được tự động đồng bộ lên S3 theo đường dẫn được định nghĩa trong TelcoChurnProcessStep của SageMaker Pipeline.
{{% /notice %}}

#### 2.2. File `evaluate.py` (Dùng cho TelcoChurnEvalStep)

Script này giải nén mô hình XGBoost tốt nhất từ bước HPO, dự đoán trên tập test.csv và tính chỉ số AUC score.

1. Tạo file `evaluate.py` với nội dung sau:

```python
import json
import os
import tarfile
import pathlib
import pandas as pd
import numpy as np
import xgboost as xgb
from sklearn.metrics import roc_auc_score

if __name__ == "__main__":
    model_path = "/opt/ml/processing/model/model.tar.gz"
    test_path = "/opt/ml/processing/test/test.csv"
    output_dir = "/opt/ml/processing/evaluation"

    with tarfile.open(model_path) as tar:
        tar.extractall(path=".")

    model = xgb.Booster()
    model.load_model("xgboost-model")

    df_test = pd.read_csv(test_path, header=None)
    y_test = df_test.iloc[:, 0].values
    X_test = df_test.iloc[:, 1:].values

    dtest = xgb.DMatrix(X_test)
    predictions = model.predict(dtest)

    auc_score = roc_auc_score(y_test, predictions)
    print(f" AUC Score thu được trên tập Test: {auc_score:.4f}")

    report_dict = {
        "binary_classification_metrics": {
            "auc": {
                "value": auc_score,
                "standard_deviation": "NaN"
            }
        }
    }

    pathlib.Path(output_dir).mkdir(parents=True, exist_ok=True)
    evaluation_path = os.path.join(output_dir, "evaluation.json")
    with open(evaluation_path, "w") as f:
        json.dump(report_dict, f)
```

#### Tóm tắt

Trong phần này, bạn đã chuẩn bị hai script quan trọng:
* preprocessing.py: Xử lý, làm sạch và chia tập dữ liệu theo định dạng yêu cầu của SageMaker XGBoost.
* evaluate.py: Đánh giá mô hình đã huấn luyện bằng chỉ số AUC và xuất file evaluation.json phục vụ việc kiểm tra điều kiện đăng ký mô hình (ConditionStep).