---
title: "Verify Connections"
date: 2024-07-07
weight: 4
chapter: false
pre : " <b> 5.3.4. </b> "
---

### 5.3.4. Database Connection Verification and Table Setup

Once network and database configurations are complete, we verify database connection access using DBeaver and initialize the baseline table schemas.

#### CLI Connection (Reference):
Connect to the business database via CLI:
```bash
psql -h fashion-rds.c7846wiue0od.ap-southeast-1.rds.amazonaws.com -p 5432 -U dbadmin -d fashiondb
```
Connect to the training database via CLI:
```bash
psql -h training-db.c7846wiue0od.ap-southeast-1.rds.amazonaws.com -p 5432 -U dbadmin -d fashiondb
```

---

#### Step-by-Step DBeaver Connection and Table Initialization:

1. Open DBeaver, navigate to **Database** > **New Database Connection**, and select the **PostgreSQL** driver.

![New Connection in DBeaver](/images/5-Workshop/5.3-Database-setup/5.3.4-step03-dbeaver-newconn.png)

2. Enter the connection settings: Host endpoint, Database name (`fashiondb`), Username (`dbadmin`), and your password.

![Configure Connection Details](/images/5-Workshop/5.3-Database-setup/5.3.4-step04-dbeaver-config.png)

3. Click **Test Connection...** and verify that the connection status is **Connected**.

![Test Connection Success](/images/5-Workshop/5.3-Database-setup/5.3.4-step05-dbeaver-testconnection.png)

4. Perform the same process for the remaining database, ensuring DBeaver lists both database connections in the navigation panel.

![Both DB Connections Ready](/images/5-Workshop/5.3-Database-setup/5.3.4-step06-dbeaver-both-connected.png)

5. Open a SQL Editor in DBeaver and execute the following SQL DDL script on the business database (`fashion-rds`) to create baseline schemas:

```sql
-- 1. Products table
CREATE TABLE products (
    product_id VARCHAR(50) PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10, 2)
);

-- 2. Stores table
CREATE TABLE stores (
    store_id VARCHAR(50) PRIMARY KEY,
    store_name VARCHAR(100),
    city VARCHAR(50)
);

-- 3. Historical transactions table
CREATE TABLE transactions (
    transaction_id VARCHAR(50) PRIMARY KEY,
    store_id VARCHAR(50) REFERENCES stores(store_id),
    product_id VARCHAR(50) REFERENCES products(product_id),
    date DATE,
    qty INT
);
```

![Run SQL to create tables and check table structures](/images/5-Workshop/5.3-Database-setup/5.3.4-step08-create-tables.png)