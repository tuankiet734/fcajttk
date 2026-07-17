---
title: "Week 11"
date: 2026-06-29
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

# Work Log: End-to-End System Testing, Resource Cleanup & Project Video Demo

> **Week 11 - June 29, 2026:** Performed comprehensive end-to-end integration testing of the fully deployed Fashion Retail system, validating user role permissions, ML forecast accuracy, inventory alert triggers, and system resilience. This week also completed systematic decommissioning of all provisioned AWS resources to eliminate ongoing charges, and produced the project demonstration video capturing the live system in operation.

---

### Weekly Objectives

- Execute **end-to-end role-based access testing**: validate that each of the 7 user roles experiences the correct dashboard view, tab restrictions, and data mutation permissions.
- Verify the **ML inference pipeline**: confirm that clicking a store on the Mapbox map fetches a valid forecast from the Lambda API and renders accurate Plotly charts.
- Validate **inventory shortage alert logic**: confirm that the red alert banner appears when forecasted demand exceeds the current warehouse stock level.
- Execute the **resource cleanup sequence** in the correct dependency order to terminate all provisioned AWS resources without orphaned charges.
- Record and publish the **project demonstration video** showcasing all completed features of the Fashion Retail Portal.

---

### Daily Activity Log

| Day | Activity | Date |
|-----|----------|------|
| Monday | Performed **role-based access testing**: logged in as Sales Staff (confirmed restricted sidebar with only *Transactions* and *Products*), Store Manager (confirmed *Discounts* and *Employees* edit access within assigned store boundary), and Director (confirmed full Dashboard access with all management tabs visible). | Jun 29 |
| Tuesday | Validated the **ML forecast pipeline**: clicked multiple store markers on the Mapbox map, confirmed the Lambda `/predict` API returned 200 status with valid demand payloads, and verified that the Plotly.js Actual vs. Forecasted line chart rendered correctly with plausible trend alignment. Triggered the EventBridge cron rule manually and confirmed the full ETL → Training → S3 upload cycle completed successfully. | Jun 30 |
| Wednesday | Tested **inventory shortage alerts**: artificially reduced warehouse stock levels in the database to below the next-week forecasted demand for selected SKUs, confirmed the red shortage banner appeared in the portal UI, and reset stock levels afterward. Verified MFA login flow with Google Authenticator on a physical device. | Jul 1 |
| Thursday | Executed the **resource cleanup sequence**: deleted Lambda functions and API Gateway stages, removed Glue jobs and Data Catalog tables, terminated EC2 instances (ML-Forecast-Server, fashion-web-server), deleted Auto Scaling Groups and Launch Templates, removed ALBs and Target Groups, deleted CloudFront distribution, and terminated both RDS instances (waited for `deleted` status before removing DB subnet groups and Parameter Groups). Emptied and deleted the S3 bucket. | Jul 2 |
| Friday | Recorded the **project demonstration video**: captured a full walkthrough of the live system including the login flow with 2FA, the Mapbox store map, the LightGBM forecast chart, the RBAC role switching demonstration, the inventory shortage alert trigger, and the multi-language switcher (Vietnamese, English, Chinese). Uploaded the video to Google Drive and embedded the shareable link in the documentation. | Jul 3 |

---

### Key Learning Outcomes

- **Role Permission Testing Methodology:** Testing each role against a pre-defined permission matrix checklist (checking visible tabs, accessible API routes, and data mutation capabilities) is the only reliable method to confirm RBAC correctness; visual inspection alone is insufficient.
- **Resource Cleanup Dependency Order:** AWS resources have strict deletion dependencies. The correct cleanup sequence is: Lambda → API Gateway → Glue → EC2 instances → ASG → ALB → CloudFront → RDS instances → DB Subnet Groups → Security Groups → VPC components. Deviating from this order triggers `DependencyViolation` errors.
- **Manual EventBridge Trigger for Pipeline Testing:** The "Send test event" feature in the EventBridge console triggers the cron rule immediately rather than waiting for the scheduled time, enabling rapid pipeline validation without modifying the cron expression.
- **Video Demo as Project Evidence:** A recorded demo video provides stakeholders with a timestamped, reproducible record of system functionality at project completion—particularly important for workshop deliverables where the live environment will be decommissioned.
- **S3 Bucket Deletion Prerequisites:** An S3 bucket cannot be deleted while it contains objects. Running `aws s3 rm s3://bucket-name --recursive` before `aws s3 rb s3://bucket-name` ensures a clean, error-free deletion sequence.

---

*Source: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
