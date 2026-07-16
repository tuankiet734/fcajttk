---
title: "Script Python trích xuất thô"
date: 2024-07-07
weight: 2
chapter: false
pre : " <b> 5.4.2. </b> "
---

### 5.4.2. Script Python Trích xuất dữ liệu thô (`de-fashion-rds-extract`)

Tác vụ đầu tiên chạy trên môi trường **AWS Glue Python Shell** để truy vấn trực tiếp cơ sở dữ liệu PostgreSQL nghiệp vụ và kết xuất file nén Parquet lên vùng đệm Landing Zone trên S3.

#### Các bước khởi tạo và cấu hình AWS Glue Job:

1. Truy cập dịch vụ **AWS Glue** trên AWS Management Console, tại thanh menu điều hướng bên trái, nhấp chọn **ETL jobs**.

![Mở AWS Glue ETL jobs](/images/5-Workshop/5.4-Feature-extraction/5.4.2-step01-glue-console.png)

2. Đặt tên Job trong tab **Job details** là `de-fashion-rds-extract` và chọn IAM Role phù hợp là `de-fashion-glue-role`.

![Cấu hình tên và Role cho Job](/images/5-Workshop/5.4-Feature-extraction/5.4.2-step03-job-name.png)

3. Cấu hình **Python version** là `Python 3.9` hoặc phiên bản mới hơn.

![Cấu hình Python Version](/images/5-Workshop/5.4-Feature-extraction/5.4.2-step04-python-version.png)

4. Nhập mã nguồn Python dưới đây vào trình biên tập code trực tuyến (Script editor) của Glue Job:

```python
import sys
import pandas as pd
from sqlalchemy import create_engine
from datetime import datetime
from awsglue.utils import getResolvedOptions

# Đọc các tham số truyền vào từ cấu hình Glue Job
args = getResolvedOptions(sys.argv,
    ['db_host','db_port','db_name','db_user','db_pass','s3_bucket'])

# Tạo chuỗi kết nối engine PostgreSQL qua thư viện sqlalchemy
engine = create_engine(
    f"postgresql+psycopg2://{args['db_user']}:{args['db_pass']}@"
    f"{args['db_host']}:{args['db_port']}/{args['db_name']}"
)

# Truy vấn dữ liệu thô từ 3 bảng nghiệp vụ chính
transactions = pd.read_sql("SELECT * FROM transactions", engine)
products = pd.read_sql("SELECT * FROM products", engine)
stores = pd.read_sql("SELECT * FROM stores", engine)

# Xác định thư mục đích trên S3 theo ngày hiện tại
today = datetime.now().strftime("%Y-%m-%d")
base = f"s3://{args['s3_bucket']}/landing_zone/{today}"

# Xuất dữ liệu ra file nén Parquet tốc độ cao
transactions.to_parquet(f"{base}/transactions.parquet", index=False)
products.to_parquet(f"{base}/products.parquet", index=False)
stores.to_parquet(f"{base}/stores.parquet", index=False)

print("Extract thanh cong!")
```

5. Cuộn xuống phần **Advanced properties** > **Job parameters** để cấu hình các tham số truyền vào CSDL PostgreSQL và Bucket S3.

![Cấu hình Job Parameters](/images/5-Workshop/5.4-Feature-extraction/5.4.2-step06-job-parameters.png)