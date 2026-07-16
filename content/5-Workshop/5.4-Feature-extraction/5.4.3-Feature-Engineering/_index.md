---
title: "Spark ETL Feature Engineering Script"
date: 2024-07-07
weight : 4
chapter: false
pre : " <b> 5.4.3. </b> "
---

### 5.4.3. Spark ETL Feature Engineering Script (`glue_feature_engineering.py`)

The core compute task is run via **Apache Spark** on **AWS Glue** to calculate historical time-series features.

#### Step-by-Step Configuration of Spark Job:

1. To connect Spark with PostgreSQL RDS, upload the PostgreSQL JDBC Driver `postgresql-42.7.3.jar` to S3, and enter the S3 path under **Job details** > **Libraries** > **Dependent jars path**.

![Configure Dependent Jars Path](/images/5-Workshop/5.4-Feature-extraction/5.4.3-step04-dependent-jars.png)

2. Copy the Spark ETL code from [glue_feature_engineering.py](file:///d:/b%C3%A1o%20c%C3%A1o%20AWS/source_code/glue_feature_engineering.py) and paste it into the Glue Job script editor.

![Paste Spark Script into Editor](/images/5-Workshop/5.4-Feature-extraction/5.4.3-step05-spark-code.png)