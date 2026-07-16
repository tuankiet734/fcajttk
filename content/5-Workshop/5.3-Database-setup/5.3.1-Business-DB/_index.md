---
title: "Initialize Business Database"
date: 2024-07-07
weight: 1
chapter: false
pre : " <b> 5.3.1. </b> "
---

### 5.3.1. Initialize Business Database (`fashion-rds`)

Below is the detailed step-by-step guide to initialize the business database using Amazon RDS PostgreSQL on the AWS Console:

1. Sign in to the AWS Management Console, search for `RDS` in the top search bar, and select the **RDS** service.

![Access RDS service](/images/5-Workshop/5.3-Database-setup/5.3.1-step01-search-rds.png)

2. In the RDS Dashboard interface, click the orange **Create database** button.

![RDS Dashboard](/images/5-Workshop/5.3-Database-setup/5.3.1-step02-rds-dashboard.png)

3. Select the **Standard create** method to customize detailed configurations.

![Select Standard Create](/images/5-Workshop/5.3-Database-setup/5.3.1-step03-standard-create.png)

4. Select the Engine type as **PostgreSQL** and choose the appropriate default version (e.g. PostgreSQL 15.18-R1).

![Select PostgreSQL Engine](/images/5-Workshop/5.3-Database-setup/5.3.1-step04-engine-postgresql.png)

5. Select the Template as **Free Tier** to ensure cost optimization and use free tier resources.

![Select Free Tier Template](/images/5-Workshop/5.3-Database-setup/5.3.1-step05-template-freetier.png)

6. Under the Settings section, enter the DB instance identifier as `fashion-rds` and configure the administrative master username as `dbadmin`.

![Configure Settings Identifier](/images/5-Workshop/5.3-Database-setup/5.3.1-step06-settings-identifier.png)

7. Configure the default instance type `db.t3.micro` and storage type `gp2` with a capacity of `20 GiB`, and enable Storage Autoscaling.

![Configure Instance and Storage](/images/5-Workshop/5.3-Database-setup/5.3.1-step07-instance-storage.png)

8. Configure Connectivity: Select the Default VPC, set Public access to **Yes**, and choose **Create new** VPC Security Group.

![Configure Connectivity](/images/5-Workshop/5.3-Database-setup/5.3.1-step08-connectivity.png)

9. Scroll down to the Additional configuration section and enter the initial database name as `fashiondb`.

![Configure Additional Configuration](/images/5-Workshop/5.3-Database-setup/5.3.1-step09-additional-config.png)

10. Review the estimated costs at the bottom of the page and click the orange **Create database** button to start database creation.

![Click Create Database Button](/images/5-Workshop/5.3-Database-setup/5.3.1-step10-create-button.png)

11. The system begins initializing the database with the status **Creating**. This process takes approximately 5-7 minutes.

![Creating Status on RDS](/images/5-Workshop/5.3-Database-setup/5.3.1-step11-creating-status.png)