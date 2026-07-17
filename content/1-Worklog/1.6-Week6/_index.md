---
title: "Week 6"
date: 2026-05-25
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

# Work Log: Feature Extraction – AWS Glue ETL Overview, Raw Ingestion & Feature Engineering

> **Week 6 - May 25, 2026:** Implemented the automated data transformation pipeline using AWS Glue. This week involved configuring two sequential ETL jobs: the first job (`de-fashion-rds-extract`) ingesting raw transaction records from the production RDS into a staging format, and the second job (`glue_feature_engineering.py`) computing time-series features essential for demand forecasting model accuracy.

---

### Weekly Objectives

- Understand the role of **AWS Glue** as a fully managed serverless ETL service in the ML pipeline.
- Configure the first Glue job (**Raw Ingestion**): extract raw sales transactions from `fashion-rds` and land them into the staging area inside `training-db`.
- Configure the second Glue job (**Feature Engineering**): apply time-series feature transformations including lag features, rolling window statistics, and sales velocity metrics.
- Validate the transformation outputs in the `training-db` database by querying the resulting feature tables.
- Test and monitor Glue job runs using the AWS Glue Console (run logs, duration, DPU metrics).

---

### Daily Activity Log

| Day | Activity | Date |
|-----|----------|------|
| Monday | Studied the AWS Glue **ETL Overview**: reviewed the Glue Data Catalog concept, Spark-based job runtime, Glue connections to RDS via JDBC, and DPU (Data Processing Unit) cost allocation. | May 25 |
| Tuesday | Created the **Raw Ingestion Glue Job** (`de-fashion-rds-extract`): configured a JDBC Glue connection to `fashion-rds`, authored a PySpark script to read the `transactions` table and write the output to a staging schema inside `training-db`. | May 26 |
| Wednesday | Authored and deployed the **Feature Engineering Glue Job** (`glue_feature_engineering.py`): wrote PySpark transformations to compute 7-day and 30-day rolling sales averages (`rolling_avg_7d`, `rolling_avg_30d`), lag values (`lag_1`, `lag_7`), and daily velocity metrics per SKU per store. | May 27 |
| Thursday | Executed both Glue jobs sequentially and monitored run progress in the **Glue Console**: checked job run status, reviewed CloudWatch logs for errors, and confirmed successful completion of both stages within acceptable runtime. | May 28 |
| Friday | Connected to `training-db` with `psql` and ran validation queries to confirm that the `features` table was populated correctly: verified row counts matched expected record volumes and spot-checked lag/rolling feature values for logical consistency. | May 29 |

---

### Key Learning Outcomes

- **Glue JDBC Connections:** Configuring a Glue connection to RDS requires that the RDS Security Group explicitly allows inbound traffic from the **Glue Elastic Network Interface (ENI)** CIDR range, not just from EC2 Security Groups.
- **PySpark Feature Engineering:** Lag and rolling window computations are implemented using Spark Window functions (`Window.partitionBy("store_id", "sku").orderBy("date")`), enabling efficient parallel execution across data partitions.
- **DPU Cost Awareness:** Each Glue job run is billed by the minute per DPU consumed. Setting the minimum DPU to 2 for lightweight ETL jobs reduces costs significantly compared to the default allocation.
- **Job Bookmarks:** Enabling Glue Job Bookmarks allows incremental processing—only new records added since the last run are ingested—preventing full table re-scans on every execution.
- **CloudWatch Monitoring:** Streaming Glue job logs to CloudWatch Log Groups provides real-time debugging capability; common errors (JDBC driver class not found, schema mismatch) are immediately surfaced in the log stream.

---

*Source: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
