---
title: "Week 7"
date: 2026-06-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

# Work Log: ML Model Training Pipeline & S3 Model Artifact Storage

> **Week 7 - June 1, 2026:** Executed the full machine learning training cycle on the dedicated `ML-Forecast-Server` EC2 instance. This week involved setting up the Python training environment, loading feature data from `training-db`, running the LightGBM model training script, evaluating model performance metrics, and uploading serialized model artifacts to the `fashion-retail-model-storage` S3 bucket.

---

### Weekly Objectives

- Configure the **ML-Forecast-Server EC2 instance**: install Python 3.10, LightGBM, scikit-learn, psycopg2, and boto3 dependencies.
- Load the engineered feature tables from `training-db` and prepare train/validation dataset splits.
- Train a **LightGBM gradient-boosted tree model** optimized for time-series demand forecasting.
- Evaluate the model using RMSE, MAE, and MAPE metrics across the validation set.
- Serialize the trained model as a `.pkl` artifact and upload it to the designated S3 bucket for Lambda inference consumption.

---

### Daily Activity Log

| Day | Activity | Date |
|-----|----------|------|
| Monday | Configured the **ML-Forecast-Server** environment: SSH'd into the EC2 instance, installed Python 3.10 via deadsnakes PPA, and created a virtual environment with `pip install lightgbm scikit-learn psycopg2-binary boto3 pandas`. | Jun 1 |
| Tuesday | Developed the **data loading module**: wrote a Python script connecting to `training-db` via psycopg2 connection pool, loaded the feature tables into Pandas DataFrames, applied chronological train/validation split (80/20 by date), and confirmed feature distribution statistics. | Jun 2 |
| Wednesday | Trained the **LightGBM forecasting model**: configured hyperparameters (num_leaves=63, learning_rate=0.05, n_estimators=500, subsample=0.8), ran the training loop with early stopping on the validation set, and logged feature importance rankings. | Jun 3 |
| Thursday | Evaluated model performance on the validation set: computed **RMSE** (Root Mean Square Error), **MAE** (Mean Absolute Error), and **MAPE** (Mean Absolute Percentage Error) per store and per SKU segment. Identified high-error SKUs for diagnostic review. | Jun 4 |
| Friday (office visit) | Serialized the trained model with `pickle.dump()` into three segmented artifact files and uploaded them to S3 using `boto3`: `s3://fashion-retail-model-storage/models/model_global.pkl`, `model_store.pkl`, `model_sku.pkl`. Verified upload integrity with `aws s3 ls`. | Jun 5 |

---

### Key Learning Outcomes

- **LightGBM Suitability for Retail Forecasting:** LightGBM's leaf-wise tree growth strategy efficiently captures non-linear seasonal patterns in retail transaction data, outperforming simpler linear regression models on MAPE metrics.
- **Chronological Train/Validation Split:** For time-series problems, **random shuffling must be avoided**—splits must respect temporal ordering (all training data predates all validation data) to simulate real-world forecasting conditions without data leakage.
- **Feature Importance Analysis:** LightGBM natively computes feature importance by split count and gain, revealing that `rolling_avg_7d` and `lag_7` were the most predictive features, validating the Glue feature engineering design choices.
- **S3 as Model Registry:** Uploading versioned model artifacts to S3 with path conventions (`/models/v1/`, `/models/v2/`) enables Lambda functions to load the latest version dynamically without code redeployment.
- **IAM Instance Profile for S3 Access:** Attaching an IAM Instance Profile with `AmazonS3FullAccess` permissions to the EC2 instance allows `boto3` to authenticate S3 operations without embedding access keys in the codebase.

---

*Source: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
