---
title: "Python Raw Extraction Script"
date: 2024-07-07
weight: 2
chapter: false
pre : " <b> 5.4.2. </b> "
---

### 5.4.2. Python Raw Extraction Script (`de-fashion-rds-extract`)

The initial extraction task is deployed on **AWS Glue Python Shell** to query PostgreSQL tables and save the data in compressed Parquet files to the S3 Landing Zone.

#### Step-by-Step Creation and Configuration of AWS Glue Job:

1. Access the **AWS Glue** service on the AWS Management Console, and select **ETL jobs** on the left navigation menu.

![Open AWS Glue ETL jobs](/images/5-Workshop/5.4-Feature-extraction/5.4.2-step01-glue-console.png)

2. Set the Job name in the **Job details** tab as `de-fashion-rds-extract` and select the appropriate IAM Role `de-fashion-glue-role`.

![Configure Job Name and Role](/images/5-Workshop/5.4-Feature-extraction/5.4.2-step03-job-name.png)

3. Configure the **Python version** to `Python 3.9` or later.

![Configure Python Version](/images/5-Workshop/5.4-Feature-extraction/5.4.2-step04-python-version.png)

4. Enter the following Python source code into the Glue Job online script editor:

```python
import sys
import pandas as pd
from sqlalchemy import create_engine
from datetime import datetime
from awsglue.utils import getResolvedOptions

# Retrieve job parameters
args = getResolvedOptions(sys.argv,
    ['db_host','db_port','db_name','db_user','db_pass','s3_bucket'])

# Create PostgreSQL database engine connection via sqlalchemy
engine = create_engine(
    f"postgresql+psycopg2://{args['db_user']}:{args['db_pass']}@"
    f"{args['db_host']}:{args['db_port']}/{args['db_name']}"
)

# Query business tables
transactions = pd.read_sql("SELECT * FROM transactions", engine)
products = pd.read_sql("SELECT * FROM products", engine)
stores = pd.read_sql("SELECT * FROM stores", engine)

# Define destination S3 path prefixed by today's date
today = datetime.now().strftime("%Y-%m-%d")
base = f"s3://{args['s3_bucket']}/landing_zone/{today}"

# Output as compressed Parquet files
transactions.to_parquet(f"{base}/transactions.parquet", index=False)
products.to_parquet(f"{base}/products.parquet", index=False)
stores.to_parquet(f"{base}/stores.parquet", index=False)

print("Extract thanh cong!")
```

5. Scroll down to **Advanced properties** > **Job parameters** to configure database connection parameters and your S3 bucket.

![Configure Job Parameters](/images/5-Workshop/5.4-Feature-extraction/5.4.2-step06-job-parameters.png)