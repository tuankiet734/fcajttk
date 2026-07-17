---
title: "Week 5"
date: 2026-05-18
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

# Work Log: Database Setup – Business RDS, Training RDS, Security & Connectivity Verification

> **Week 5 - May 18, 2026:** Provisioned and configured two dedicated Amazon RDS PostgreSQL database instances serving distinct purposes: a production transactional database (`fashion-rds`) for the live web application and a training-specific database (`training-db`) for the machine learning pipeline. This week also covered VPC Security Group isolation rules and connectivity validation from EC2 instances.

---

### Weekly Objectives

- Deploy the **Business Database** (`fashion-rds`): the central PostgreSQL RDS instance storing live transactional data (orders, customers, products, inventory).
- Deploy the **Training Database** (`training-db`): a separate PostgreSQL RDS instance exclusively storing processed ML feature tables output by the AWS Glue ETL pipeline.
- Configure **Security Group rules** to enforce strict network-level access controls between the databases and their respective application tiers.
- Validate direct **database connectivity** from EC2 instances using the `psql` CLI client.
- Understand the difference between Multi-AZ standby configurations and Read Replicas in terms of purpose, failover behavior, and cost.

---

### Daily Activity Log

| Day | Activity | Date |
|-----|----------|------|
| Monday | Deployed **`fashion-rds`** (Business DB): selected PostgreSQL 15.x engine, `db.t3.medium` instance class, configured the database name as `fashiondb`, master username as `dbadmin`, and placed the instance inside the private subnet group. | May 18 |
| Tuesday | Deployed **`training-db`** (Training DB): used identical PostgreSQL version and instance class but created a separate DB subnet group within the private data tier subnet, naming the database `trainingdb`. | May 19 |
| Wednesday | Configured **Security Group rules**: created `rds-fashion-sg` permitting inbound port 5432 only from the EC2 application Security Group ID, and `rds-training-sg` permitting inbound port 5432 only from the AWS Glue job Security Group and the ML-Forecast-Server Security Group. | May 20 |
| Thursday | Validated **connectivity from EC2 to `fashion-rds`**: SSH'd into the Web Application Server and ran `psql -h <fashion-rds-endpoint> -U dbadmin -d fashiondb` to confirm connection establishment and ran a `SELECT version();` query. | May 21 |
| Friday | Validated **connectivity from EC2 to `training-db`**: SSH'd into the ML-Forecast-Server and ran connectivity tests. Populated seed data into `trainingdb` for subsequent Glue ETL verification. | May 22 |

---

### Key Learning Outcomes

- **Dual-Database Pattern:** Separating the production database from the training database prevents ML workload query spikes from degrading end-user application performance, a crucial design principle for production ML systems.
- **DB Subnet Groups:** RDS instances require a **DB Subnet Group** spanning at least two Availability Zones to support Multi-AZ failover; failure to configure this results in a provisioning error.
- **Security Group Chaining:** Rather than allowing port 5432 from a CIDR range, referencing the source **Security Group ID** restricts database access exclusively to instances assigned to that group—a significantly more secure and scalable approach.
- **`psql` CLI Connectivity Test:** Confirming a connection with `psql` before deploying application code eliminates ambiguity about whether connection issues originate from the application configuration or the network layer.
- **Parameter Groups:** Customizing PostgreSQL settings (e.g., `max_connections`, `shared_buffers`) through RDS Parameter Groups allows performance tuning without direct server OS access.

---

*Source: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
