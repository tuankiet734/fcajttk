---
title: "Week 3"
date: 2026-05-04
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

# Work Log: System Architecture Deep Dive & Workshop Infrastructure Blueprinting

> **Week 3 - May 4, 2026:** Conducted an in-depth analysis of the complete two-zone system architecture for the Fashion Retail & ML Forecasting project. This week produced the finalized infrastructure blueprint with all component assignments, network topology, and inter-service communication flows documented.

---

### Weekly Objectives

- Analyze the complete system architecture across both the **Web Application & API Group** and the **Data & Machine Learning Pipeline Group**.
- Understand how each AWS service is positioned: CloudFront → External ALB → Web ASG → Internal ALB → API ASG → RDS.
- Map the ML pipeline data flow: RDS (`fashion-rds`) → AWS Glue ETL → Training DB (`training-db`) → ML Training Server → S3 → Lambda API → EventBridge Scheduler.
- Document component responsibilities, configuration parameters, and network access boundaries for each resource before implementation begins.

---

### Daily Activity Log

| Day | Activity | Date |
|-----|----------|------|
| Monday | Studied the **Web Application & API Layer** in depth: reviewed CloudFront distribution settings, origin groups, and cache behaviors for the ALB origin. | May 4 |
| Tuesday | Analyzed the **Auto Scaling Group** configuration for both the Web Application tier and the RESTful API tier, including launch template specification, minimum/maximum/desired capacity settings. | May 5 |
| Wednesday | Studied the **Data & ML Pipeline** components: reviewed how AWS Glue Spark jobs (`de-fashion-rds-extract` and `glue_feature_engineering.py`) perform ETL from the production RDS to the training database. | May 6 |
| Thursday | Mapped the **model inference pipeline**: ML-Forecast-Server (EC2) trains the LightGBM model, serializes it to an S3 bucket (`fashion-retail-model-storage`), and the Lambda API loads and serves predictions. | May 7 |
| Friday | Completed the finalized architecture blueprint document. Prepared a list of AWS services to provision, their target VPC subnets, Security Group rules, and IAM role requirements. | May 8 |

---

### Architecture Summary: Two-Zone System

**Zone 1 – Web Application & API Access Layer:**
- **Amazon CloudFront** acts as the CDN entry point, caching static assets and routing dynamic requests to the External ALB.
- **External ALB** balances HTTP/HTTPS traffic across Web Application EC2 instances in the Auto Scaling Group.
- **Internal ALB** securely routes API calls from the Web App to the private backend RESTful API Auto Scaling Group.

**Zone 2 – Data & Machine Learning Pipeline:**
- **Amazon RDS (`fashion-rds`):** PostgreSQL production database storing live transaction, order, user, and inventory data.
- **AWS Glue ETL:** Two automated Spark jobs extract raw transaction records and engineer time-series features (lags, rolling averages, velocity metrics).
- **Amazon RDS (`training-db`):** Dedicated PostgreSQL instance storing the processed feature tables ready for ML ingestion.
- **ML-Forecast-Server (EC2):** Dedicated compute instance running the LightGBM training script and producing serialized model artifacts.
- **Amazon S3 (`fashion-retail-model-storage`):** Durable artifact storage holding trained model pickle files.
- **AWS Lambda + API Gateway:** Serverless inference endpoint loading models from S3 and returning forecast predictions.
- **Amazon EventBridge:** Daily cron trigger refreshing the pipeline automatically.

---

### Key Learning Outcomes

- **End-to-End Traceability:** Every request entering the system through CloudFront can be traced through ALBs, EC2 App Servers, the API tier, and ultimately to the RDS data layer.
- **Glue Spark Jobs:** AWS Glue provides a fully managed Spark environment, eliminating the need to provision and manage dedicated Hadoop/Spark clusters for ETL workloads.
- **S3 as Model Registry:** Using S3 as a model artifact store is a lightweight alternative to a dedicated MLflow server; Lambda functions can download and load models at cold start.
- **EventBridge Scheduling:** A cron-based EventBridge rule triggers the entire ETL + Training + Deployment pipeline daily without any server infrastructure to manage.

---

*Source: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
