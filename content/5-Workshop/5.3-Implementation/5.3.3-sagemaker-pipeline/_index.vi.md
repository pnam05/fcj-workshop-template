---
title : "Xây dựng & Khởi chạy SageMaker Pipeline"
date : 2026-07-27 
weight : 3
chapter : false
pre : " <b> 5.3.1 </b> "
---

 Trong phần này, chúng ta sẽ thực hành toàn bộ quy trình MLOps trên Amazon SageMaker, từ bước chuẩn bị dữ liệu, huấn luyện đơn lẻ, tinh chỉnh siêu tham số (HPO), đăng ký mô hình, triển khai Serverless Endpoint cho tới việc đóng gói tự động thành một **SageMaker Pipeline 4 bước**.

#### Khởi tạo Môi trường & Kết nối S3 Bucket
Đầu tiên, nhập các thư viện cần thiết, thiết lập phiên làm việc SageMaker và đọc dữ liệu thô từ S3.

```python
import boto3
import pandas as pd
import sagemaker

sagemaker_session = sagemaker.Session()
role = sagemaker.get_execution_role()
region = sagemaker_session.boto_region_name

bucket_name = "telco-churn-mlops-fcaj" 
raw_data_key = "raw/WA_Fn-UseC_-Telco-Customer-Churn.csv"

s3_path = f"s3://{bucket_name}/{raw_data_key}"
df = pd.read_csv(s3_path)

print(f" Kết nối thành công tới S3 Bucket: {bucket_name}")
print(f" Region: {region}")
print(f" Kích thước dataset: {df.shape}")
df.head(3)
```

#### Xử lý Dữ liệu với Processing Job
Sử dụng SKLearnProcessor để kích hoạt một container chạy file preprocessing.py. Dữ liệu thô sẽ được đọc từ S3, sau đó chia làm các tập Train, Validation, Test và tự động đẩy ngược lại S3

```python
from sagemaker.sklearn.processing import SKLearnProcessor
from sagemaker.processing import ProcessingInput, ProcessingOutput

# 1. Khai báo SKLearnProcessor
sklearn_processor = SKLearnProcessor(
    framework_version="1.2-1",
    role=role,
    instance_type="ml.t3.medium",
    instance_count=1,
    base_job_name="telco-churn-preprocessing"
)

# 2. Định nghĩa vị trí Input (S3) và Output (S3)
input_s3_uri = f"s3://{bucket_name}/raw/WA_Fn-UseC_-Telco-Customer-Churn.csv"
output_s3_uri = f"s3://{bucket_name}/processed"

print(" Đang khởi chạy Processing Job trên AWS...")

# 3. Kích hoạt Processing Job
sklearn_processor.run(
    code="preprocessing.py",
    inputs=[
        ProcessingInput(
            source=input_s3_uri,
            destination="/opt/ml/processing/input"
        )
    ],
    outputs=[
        ProcessingOutput(
            output_name="train_data",
            source="/opt/ml/processing/output/train",
            destination=f"{output_s3_uri}/train"
        ),
        ProcessingOutput(
            output_name="validation_data",
            source="/opt/ml/processing/output/validation",
            destination=f"{output_s3_uri}/validation"
        ),
        ProcessingOutput(
            output_name="test_data",
            source="/opt/ml/processing/output/test",
            destination=f"{output_s3_uri}/test"
        )
    ]
)
```
![preprocess](/images/5-Workshop/5.3-Implementation/preprocess.png)


#### Huấn luyện Mô hình XGBoost Đơn lẻ (Training Job)
##### Lấy Image URI & Thiết lập Cấu hình S3

```python
from sagemaker.inputs import TrainingInput

# 1. Lấy URI của Docker Image XGBoost
xgboost_container = sagemaker.image_uris.retrieve(
    framework="xgboost",
    region=region,
    version="1.5-1" 
)

# 2. Đường dẫn dữ liệu và output trên S3
s3_train_data = f"s3://{bucket_name}/processed/train/train.csv"
s3_validation_data = f"s3://{bucket_name}/processed/validation/validation.csv"
s3_output_path = f"s3://{bucket_name}/models/xgboost-single-train"

train_input = TrainingInput(s3_data=s3_train_data, content_type="text/csv")
validation_input = TrainingInput(s3_data=s3_validation_data, content_type="text/csv")

print(" Đã cấu hình xong đường dẫn dữ liệu S3 cho Training Job")
```

##### Cấu hình Hyperparameters & Huấn luyện

```python
# 1. Khai báo Estimator
xgb_estimator = sagemaker.estimator.Estimator(
    image_uri=xgboost_container,
    role=role,
    instance_count=1,
    instance_type="ml.m5.large", 
    output_path=s3_output_path,
    sagemaker_session=sagemaker_session,
    base_job_name="telco-churn-xgb-train"
)

# 2. Thiết lập Hyperparameters
xgb_estimator.set_hyperparameters(
    max_depth=5,
    eta=0.2,
    gamma=4,
    min_child_weight=6,
    subsample=0.8,
    objective="binary:logistic",
    eval_metric="auc", 
    num_round=100
)

# 3. Kích hoạt Training Job
print(" Đang khởi chạy SageMaker Training Job...")
xgb_estimator.fit({
    "train": train_input,
    "validation": validation_input
})
```

#### Tinh chỉnh Siêu tham số (Hyperparameter Optimization - HPO)
Thiết lập dải tham số tìm kiếm và khởi tạo HyperparameterTuner để tự động chạy 6 thử nghiệm (2 job song song) tìm ra cấu hình đạt Validation AUC cao nhất.
##### Thiết lập Dải Tham số & Khởi tạo Tuner
```python
from sagemaker.tuner import IntegerParameter, ContinuousParameter, HyperparameterTuner
from sagemaker.workflow.pipeline_context import PipelineSession
from sagemaker.processing import ScriptProcessor

pipeline_session = PipelineSession()

# 1. Khai báo dải tìm kiếm
hyperparameter_ranges = {
    "max_depth": IntegerParameter(3, 8),
    "eta": ContinuousParameter(0.01, 0.2),
    "min_child_weight": IntegerParameter(1, 10),
    "subsample": ContinuousParameter(0.5, 0.9),
    "alpha": ContinuousParameter(0.0, 2.0)
}

objective_metric_name = "validation:auc"
objective_type = "Maximize"

# 2. Estimator & ScriptProcessor cho HPO
xgb_hpo_estimator = sagemaker.estimator.Estimator(
    image_uri=xgboost_container,
    role=role,
    instance_count=1,
    instance_type="ml.m5.large",
    output_path=f"s3://{bucket_name}/models/xgboost-hpo",
    sagemaker_session=pipeline_session, 
    base_job_name="telco-churn-hpo"
)

xgb_hpo_estimator.set_hyperparameters(
    objective="binary:logistic",
    eval_metric="auc",
    num_round=100
)

xgb_eval_processor = ScriptProcessor(
    image_uri=xgboost_container, 
    command=["python3"],
    role=role,
    instance_count=1,
    instance_type="ml.m5.large", 
    sagemaker_session=pipeline_session
)

# 3. Khai báo HyperparameterTuner
tuner = HyperparameterTuner(
    estimator=xgb_hpo_estimator,
    objective_metric_name=objective_metric_name,
    hyperparameter_ranges=hyperparameter_ranges,
    objective_type=objective_type,
    max_jobs=6,
    max_parallel_jobs=2,
    base_tuning_job_name="hpo-telco-churn"
)

print(" Đã khởi tạo HyperparameterTuner thành công!")
```

##### Chạy HPO Job & Trích xuất Kết quả Tốt nhất

```python
print(" Đang khởi chạy Hyperparameter Tuning Job trên AWS...")
tuner.fit({"train": train_input, "validation": validation_input})

# Lấy thông tin Job tốt nhất
best_job_name = tuner.best_training_job()
print(f" Training Job cho kết quả tốt nhất: {best_job_name}")

hpo_results = tuner.analytics().dataframe()
best_job_row = hpo_results[hpo_results['TrainingJobName'] == best_job_name].iloc[0]

print(f" Validation AUC tốt nhất: {best_job_row['FinalObjectiveValue']}")
for col in hpo_results.columns:
    if col not in ['TrainingJobName', 'TrainingJobStatus', 'FinalObjectiveValue', 'TrainingStartTime', 'TrainingEndTime']:
        print(f" - {col}: {best_job_row[col]}")
```
![best-conf](/images/5-Workshop/5.3-Implementation/best-conf.png)

#### Đăng ký Mô hình vào SageMaker Model Registry
Tạo mới một Model Package Group và đăng ký mô hình tốt nhất đạt được từ bước HPO.
```python
import boto3

sm_boto3_client = boto3.client('sagemaker', region_name=region)
model_package_group_name = "TelcoChurnModelGroup"

# 1. Tạo Group trong Model Registry
try:
    sm_boto3_client.create_model_package_group(
        ModelPackageGroupName=model_package_group_name,
        ModelPackageGroupDescription="Group chua cac phien ban mo hinh du doan Telco Customer Churn"
    )
    print(f" Đã tạo mới Model Package Group: {model_package_group_name}")
except Exception as e:
    if "already exists" in str(e):
        print(f" Model Package Group '{model_package_group_name}' đã tồn tại.")

# 2. Đăng ký phiên bản mô hình mới
best_model_s3_uri = f"s3://{bucket_name}/models/xgboost-hpo/{best_job_name}/output/model.tar.gz"

create_model_package_input = {
    "ModelPackageGroupName": model_package_group_name,
    "ModelPackageDescription": f"Mo hinh XGBoost tot nhat tu HPO Job {best_job_name}",
    "ModelApprovalStatus": "PendingManualApproval",
    "InferenceSpecification": {
        "Containers": [{"Image": xgboost_container, "ModelDataUrl": best_model_s3_uri}],
        "SupportedContentTypes": ["text/csv"],
        "SupportedResponseMIMETypes": ["text/csv"],
    }
}

response = sm_boto3_client.create_model_package(**create_model_package_input)
model_package_arn = response["ModelPackageArn"]
print(f"🔗 Model Package ARN: {model_package_arn}")

# 3. Phê duyệt mô hình (Approved)
sm_boto3_client.update_model_package(
    ModelPackageArn=model_package_arn,
    ModelApprovalStatus="Approved",
    ApprovalDescription="Mo hinh dat chi so AUC cao tu HPO, du dieu kien deploy len Serverless Endpoint."
)
print(" Đã chuyển trạng thái mô hình sang APPROVED thành công!")
```
![best-conf](/images/5-Workshop/5.3-Implementation/model-reg.png)

#### Triển khai & Kiểm thử Serverless Endpoint

##### Deploy Serverless Endpoint
```python
from sagemaker.serverless import ServerlessInferenceConfig
from sagemaker.model import Model

serverless_config = ServerlessInferenceConfig(
    memory_size_in_mb=2048,
    max_concurrency=10
)

endpoint_name = "telco-churn-serverless-endpoint"

model = Model(
    image_uri=xgboost_container,
    model_data=best_model_s3_uri,
    role=role,
    sagemaker_session=sagemaker_session
)

print(" Đang khởi tạo Serverless Endpoint...")
predictor = model.deploy(
    endpoint_name=endpoint_name,
    serverless_inference_config=serverless_config
)
print(f" Triển khai Serverless Endpoint thành công: {endpoint_name}")
```
![deploy](/images/5-Workshop/5.3-Implementation/deploy.png)


##### Kiểm thử dự đoán (Inference Test)

```python
# Lấy 1 mẫu dữ liệu từ tập Test trên S3 để gửi tới Endpoint
s3_test_path = f"s3://{bucket_name}/processed/test/test.csv"
test_df = pd.read_csv(s3_test_path, header=None)

sample_data = test_df.iloc[0, 1:].values
sample_csv_string = ",".join(map(str, sample_data))

response = sagemaker_session.sagemaker_runtime_client.invoke_endpoint(
    EndpointName=endpoint_name,
    ContentType="text/csv",
    Body=sample_csv_string
)

churn_probability = float(response["Body"].read().decode("utf-8"))
print(f" Xác suất rời bỏ dịch vụ (Churn Probability): {churn_probability:.4f}")
print(f" Dự đoán: {'CHURN (Rời bỏ)' if churn_probability >= 0.5 else 'RETAIN (Ở lại)'}")
```
![deploy](/images/5-Workshop/5.3-Implementation/inference.png)

#### Đóng gói & Tự động hóa với SageMaker Pipeline (4 Bước)
Toàn bộ quy trình sẽ được tự động hóa bằng SageMaker Pipeline gồm:
- **ProcessingStep:** Làm sạch & phân chia dữ liệu.  
- **TuningStep:** Tìm kiếm siêu tham số tối ưu.  
- **ProcessingStep (Evaluation):** Giải nén mô hình tốt nhất, dự đoán tập Test và xuất chỉ số AUC vào file evaluation.json. 
-  **ConditionStep:** Kiểm tra chỉ số AUC. Nếu AUC >= 0.80, tự động đăng ký mô hình vào Registry (ModelStep). Ngược lại, hủy Pipeline (FailStep)[cite: 2].
```python
from sagemaker.workflow.steps import ProcessingStep, TuningStep
from sagemaker.workflow.model_step import ModelStep
from sagemaker.workflow.pipeline import Pipeline
from sagemaker.workflow.properties import PropertyFile
from sagemaker.workflow.conditions import ConditionGreaterThanOrEqualTo
from sagemaker.workflow.condition_step import ConditionStep
from sagemaker.workflow.fail_step import FailStep
from sagemaker.workflow.functions import JsonGet
from sagemaker.workflow.pipeline_context import PipelineSession
from sagemaker.model import Model

pipeline_session = PipelineSession()

# STEP 1: PROCESSING STEP
step_process = ProcessingStep(
    name="TelcoChurnProcessStep",
    processor=sklearn_processor,
    inputs=[
        ProcessingInput(
            source=f"s3://{bucket_name}/raw/",
            destination="/opt/ml/processing/input"
        )
    ],
    outputs=[
        ProcessingOutput(output_name="train_data", source="/opt/ml/processing/output/train", destination=f"{output_s3_uri}/train"),
        ProcessingOutput(output_name="validation_data", source="/opt/ml/processing/output/validation", destination=f"{output_s3_uri}/validation"),
        ProcessingOutput(output_name="test_data", source="/opt/ml/processing/output/test", destination=f"{output_s3_uri}/test")
    ],
    code="preprocessing.py"
)

train_data_uri = step_process.properties.ProcessingOutputConfig.Outputs["train_data"].S3Output.S3Uri
val_data_uri = step_process.properties.ProcessingOutputConfig.Outputs["validation_data"].S3Output.S3Uri
test_data_uri = step_process.properties.ProcessingOutputConfig.Outputs["test_data"].S3Output.S3Uri

# STEP 2: TUNING STEP
step_tuning = TuningStep(
    name="TelcoChurnHpoStep",
    tuner=tuner,
    inputs={
        "train": TrainingInput(s3_data=train_data_uri, content_type="text/csv"),
        "validation": TrainingInput(s3_data=val_data_uri, content_type="text/csv")
    }
)

# STEP 3: EVALUATION STEP
evaluation_report = PropertyFile(
    name="EvaluationReport",
    output_name="evaluation",
    path="evaluation.json"
)

best_model_s3_uri = step_tuning.get_top_model_s3_uri(top_k=0, s3_bucket=f"{bucket_name}/models/xgboost-hpo")

step_eval = ProcessingStep(
    name="TelcoChurnEvalStep",
    processor=xgb_eval_processor,
    code="evaluate.py",
    inputs=[
        ProcessingInput(source=best_model_s3_uri, destination="/opt/ml/processing/model"),
        ProcessingInput(source=test_data_uri, destination="/opt/ml/processing/test")
    ],
    outputs=[
        ProcessingOutput(output_name="evaluation", source="/opt/ml/processing/evaluation")
    ],
    property_files=[evaluation_report]
)

# STEP 4: REGISTER & FAIL STEPS
model = Model(
    image_uri=xgb_hpo_estimator.image_uri,
    model_data=best_model_s3_uri,
    sagemaker_session=pipeline_session,
    role=role
)

step_register = ModelStep(
    name="TelcoChurnRegisterModelStep",
    step_args=model.register(
        content_types=["text/csv"],
        response_types=["text/csv"],
        inference_instances=["ml.m5.large"],
        transform_instances=["ml.m5.large"],
        model_package_group_name="TelcoChurnModelGroup",
        approval_status="Approved"
    )
)

step_fail = FailStep(
    name="TelcoChurnAUCFailStep",
    error_message=" Retrain thất bại: Mô hình mới có AUC thấp hơn ngưỡng cho phép (0.80)!"
)

# CONDITION STEP
cond_gte = ConditionGreaterThanOrEqualTo(
    left=JsonGet(
        step_name=step_eval.name,
        property_file=evaluation_report,
        json_path="binary_classification_metrics.auc.value"
    ),
    right=0.80 
)

step_cond = ConditionStep(
    name="TelcoChurnCheckAUCThreshold",
    conditions=[cond_gte],
    if_steps=[step_register], 
    else_steps=[step_fail]    
)

# PIPELINE UPSERT & START
pipeline_name = "TelcoChurnMLOpsPipeline"
pipeline = Pipeline(
    name=pipeline_name,
    steps=[step_process, step_tuning, step_eval, step_cond],
    sagemaker_session=sagemaker_session
)

pipeline.upsert(role_arn=role)
print(f" Đã khởi tạo thành công SageMaker Pipeline 4 bước: {pipeline_name}")

execution = pipeline.start()
print(f" Pipeline đang tự động thực thi! Execution ARN: {execution.arn}")
```
![deploy](/images/5-Workshop/5.3-Implementation/pipeline.png)



