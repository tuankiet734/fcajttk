---
title: "Week 8"
date: 2026-06-08
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

# Work Log: Forecast REST API with AWS Lambda & Daily Scheduling with Amazon EventBridge

> **Week 8 - June 8, 2026:** Transformed the trained LightGBM model into a production-grade, serverless REST API endpoint. This week involved authoring the Lambda inference function, packaging Python dependencies into a deployment layer, configuring API Gateway routing and API key authentication, and wiring an EventBridge daily cron rule to automate the full ML pipeline refresh cycle.

---

### Weekly Objectives

- Author a **Python AWS Lambda function** that downloads model artifacts from S3, loads them into memory, and executes LightGBM inference on incoming JSON prediction requests.
- Package required Python dependencies (`lightgbm`, `scikit-learn`, `pandas`, `boto3`) into a **Lambda Layer** to satisfy the 250 MB deployment package size limit.
- Deploy the Lambda function behind **Amazon API Gateway** with a secured `/predict` POST route protected by an API key.
- Configure an **Amazon EventBridge** rule with a daily cron expression to trigger the full pipeline: Glue ETL → Model Training → Model Upload.
- Validate the API end-to-end by submitting test inference requests via `curl` and verifying forecast output format.

---

### Daily Activity Log

| Day | Activity | Date |
|-----|----------|------|
| Monday | Authored the **Lambda inference handler** (`lambda_handler.py`): implemented S3 model download logic on cold start, feature vector construction from the incoming JSON payload (`store_id`, `sku`, `date`), and model `.predict()` call returning forecast demand as a JSON response. | Jun 8 |
| Tuesday | Built and published the **Lambda Layer**: created a local Python virtual environment, installed `lightgbm`, `scikit-learn`, and `pandas`, zipped the `site-packages` folder, and uploaded it as a Lambda Layer named `ml-inference-layer` in the `ap-southeast-1` region. | Jun 9 |
| Wednesday | Configured **Amazon API Gateway**: created a REST API resource structure (`/predict`), linked the POST method to the Lambda function via Lambda Proxy integration, deployed the API to a `dev` stage, and created an **API Key** with a Usage Plan limiting 1,000 requests/day. | Jun 10 |
| Thursday | Set up the **Amazon EventBridge Rule**: created a scheduled rule with the cron expression `cron(0 1 * * ? *)` (triggers daily at 01:00 UTC), configured it to invoke a Step Functions state machine (or a Lambda orchestrator) that sequentially runs Glue → EC2 training → S3 upload. | Jun 11 |
| Friday | Performed **end-to-end API validation**: used `curl -X POST` with the `x-api-key` header and a sample payload (`{"store_id":"20","sku":"FESH6946-S-","date":"2025-03-18"}`), received a valid 200 response with the predicted demand payload, and confirmed EventBridge test invocation succeeded in the Lambda execution logs. | Jun 12 |

---

### Key Learning Outcomes

- **Lambda Cold Start Optimization:** Loading large model pickle files (>50 MB) from S3 at every cold start introduces latency. Caching the model object in the Lambda global scope (`/tmp` directory) ensures the model is only downloaded once per execution environment lifecycle.
- **Lambda Layers for ML Dependencies:** Python ML libraries (`lightgbm`, `scikit-learn`) with native C extensions must be compiled for the Lambda execution environment (Amazon Linux 2). Building the layer on an Amazon Linux Docker container ensures binary compatibility.
- **API Gateway Lambda Proxy Integration:** Using proxy integration passes the full HTTP request context (headers, body, path) to Lambda as a structured event object, giving the function full control over the response structure (status code, headers, body).
- **EventBridge Cron Syntax:** AWS EventBridge uses a 6-field cron syntax (adding a `?` wildcard for day-of-week/month). Testing the trigger with the **"Send test event"** feature in the console validates rule behavior before waiting for the first scheduled execution.
- **API Key vs. Cognito Authorizers:** API Keys are suitable for server-to-server internal calls (e.g., Web Portal calling the forecast API); however, for end-user authentication, Cognito User Pool authorizers or Lambda custom authorizers provide stronger identity validation.

---

*Source: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
