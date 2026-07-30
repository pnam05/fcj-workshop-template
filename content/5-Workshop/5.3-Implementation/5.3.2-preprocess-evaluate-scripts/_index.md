---
title : "Create Data Processing & Model Evaluation Scripts"
date : 2026-07-29 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

#### In your Jupyter Notebook (or SageMaker Studio) environment, create 2 Python script files to serve the steps in SageMaker Pipeline:

1. In the **Amazon SageMaker AI Console**, under **Applications and IDEs** on the left menu, select **Notebooks**.
2. Select the **Notebook instances** tab and click the orange **Create notebook instance** button.

![create](/images/5-Workshop/5.3-Implementation/create-notebook.png)

#### Configure Notebook Instance

1. On the **Create notebook instance** page, under **Notebook instance settings**, set the parameters as follows:
   + **Notebook instance name**: Enter `telco-churn`
   + **Notebook instance type**: Select ml.t3.medium
   + **Platform identifier**: Select Amazon Linux 2023, Jupyter Lab 4

2. Scroll down to **Permissions and encryption**:
   + **IAM role**: Select the SageMaker-Telco-Churn-Role IAM Role created previously.
   + **Root access**: Select **Enable - Give users root access to the notebook**.
   + **Encryption key**: Keep default **No custom encryption**.

3. Scroll to the bottom of the page and click **Create notebook instance** to initialize.

![create](/images/5-Workshop/5.3-Implementation/conf-notebook.png)



#### Create `preprocessing.py` file (Used for TelcoChurnProcessStep)

This script performs data cleaning, data type casting for the TotalCharges column, One-Hot Encoding for categorical variables, and splits dataset into train, validation, test.

1. In the Jupyter Notebook (or SageMaker Studio) environment, create file `preprocessing.py` with the following content:

```python
import argparse
import os
import glob
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split

if __name__ == "__main__":
    # 1. Define input/output paths
    input_dir = "/opt/ml/processing/input"
    output_dir = "/opt/ml/processing/output"

    all_files = glob.glob(os.path.join(input_dir, "*.csv"))

    if not all_files:
        raise FileNotFoundError(f" No CSV file found in {input_dir}")

    # 2. Read and concatenate ALL found CSV files
    df_list = [pd.read_csv(file) for file in all_files]
    df = pd.concat(df_list, axis=0, ignore_index=True)
    print(f" Read {len(all_files)} CSV files. Combined raw data shape: {df.shape}")

    # 3. Clean Telco Churn data
    # Drop customerID column with no predictive value
    if 'customerID' in df.columns:
        df = df.drop('customerID', axis=1)

    # Convert TotalCharges to numeric
    df['TotalCharges'] = pd.to_numeric(df['TotalCharges'], errors='coerce')
    df['TotalCharges'].fillna(df['TotalCharges'].median(), inplace=True)

    # Convert Target variable 'Churn' to numeric (Yes: 1, No: 0)
    if 'Churn' in df.columns:
        # Cast data type to String and strip whitespace
        df['Churn'] = df['Churn'].astype(str).str.strip()

        # Map both String (Yes/No) and Numeric ('1'/'0', 1/0)
        churn_map = {'Yes': 1, 'No': 0, '1': 1, '0': 0, '1.0': 1, '0.0': 0, 1: 1, 0: 0}
        df['Churn'] = df['Churn'].map(churn_map)

    # Drop any rows without Churn label (NaN) to prevent train_test_split errors
    df = df.dropna(subset=['Churn'])
    df['Churn'] = df['Churn'].astype(int)

    # One-Hot Encoding for Categorical features
    cat_cols = df.select_dtypes(include=['object']).columns.tolist()
    df = pd.get_dummies(df, columns=cat_cols, drop_first=True)

    # Ensure all True/False One-Hot columns are explicitly cast to 1/0
    for col in df.columns:
        if df[col].dtype == 'bool':
            df[col] = df[col].astype(int)

    # Move Churn column to FIRST position (required for XGBoost)
    churn_col = df.pop('Churn')
    df.insert(0, 'Churn', churn_col)

    print(f" Data after processing & One-Hot: {df.shape}")

    # 4. Split Train / Validation / Test sets (Ratio 70% / 15% / 15%)
    train, test_val = train_test_split(df, test_size=0.3, random_state=42, stratify=df['Churn'])
    validation, test = train_test_split(test_val, test_size=0.5, random_state=42, stratify=test_val['Churn'])

    print(f" Train set: {train.shape}, Validation set: {validation.shape}, Test set: {test.shape}")

    # 5. Save output CSV files (No header titles and index)
    os.makedirs(os.path.join(output_dir, "train"), exist_ok=True)
    os.makedirs(os.path.join(output_dir, "validation"), exist_ok=True)
    os.makedirs(os.path.join(output_dir, "test"), exist_ok=True)

    train.to_csv(os.path.join(output_dir, "train", "train.csv"), index=False, header=False)
    validation.to_csv(os.path.join(output_dir, "validation", "validation.csv"), index=False, header=False)
    test.to_csv(os.path.join(output_dir, "test", "test.csv"), index=False, header=False)

    print(" Processing Job completed and files saved to output directory!")
```

{{% notice info %}}
Output data will be automatically synchronized to S3 based on the path defined in TelcoChurnProcessStep of SageMaker Pipeline.
{{% /notice %}}

#### 2.2. File `evaluate.py` (Used for TelcoChurnEvalStep)

This script extracts the best XGBoost model from the HPO step, predicts on test.csv, and calculates the AUC score.

1. Create file `evaluate.py` with the following content:

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
    print(f" AUC Score obtained on Test set: {auc_score:.4f}")

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

#### Summary

In this section, you prepared two critical scripts:
* preprocessing.py: Process, clean, and split dataset according to format required by SageMaker XGBoost.
* evaluate.py: Evaluate trained model using AUC metric and export evaluation.json file serving model registration condition check (ConditionStep).
