---
title: "Initialize Training Database"
date: 2024-07-07
weight : 3
chapter: false
pre : " <b> 5.3.2. </b> "
---

### 5.3.2. Initialize Training Database (`training-db`)

A dedicated training database is used to store feature tables for machine learning workflows. Below is the step-by-step creation guide on the AWS Console:

1. In the database creation interface (Standard create / PostgreSQL), under the **Engine Version** section, select the latest version **PostgreSQL 18.3**.

![Select PostgreSQL Engine 18.3](/images/5-Workshop/5.3-Database-setup/5.3.2-step01-engine-postgresql18.png)

2. Under the Settings section, set the DB instance identifier to `training-db` and enter the master username as `dbadmin`.

![Configure Settings training-db](/images/5-Workshop/5.3-Database-setup/5.3.2-step02-settings-trainingdb.png)

3. Under the Connectivity section, select the Default VPC, set Public access to **Yes**, and choose **Create new** VPC Security Group.

![Configure Connectivity for training-db](/images/5-Workshop/5.3-Database-setup/5.3.2-step03-connectivity-public.png)

4. Expand the Additional configuration section and enter the initial database name as `fashiondb`.

![Configure Database Name fashiondb](/images/5-Workshop/5.3-Database-setup/5.3.2-step04-additional-fashiondb.png)

5. After creation, check the RDS Databases list to verify that both `fashion-rds` and `training-db` instances are in the **Available** state.

![RDS Databases List Available](/images/5-Workshop/5.3-Database-setup/5.3.2-step05-training-db-available.png)