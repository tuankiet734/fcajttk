---
title: "Week 2"
date: 2026-04-27
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

# Work Log: Workshop Planning, Architecture Design & AWS Theoretical Foundations

> **Week 2 - April 27, 2026:** Shifted from individual cloud tasks to structured research and planning for the capstone workshop project. This week was dedicated to understanding core AWS architectural patterns, studying the services needed, and documenting the system design blueprint.

---

### Weekly Objectives

- Study the high-level architecture of the Fashion Retail Web App & ML Pipeline that will be built in the workshop.
- Research the purpose and capabilities of each AWS service involved: CloudFront, ALB, EC2, RDS, Glue, Lambda, S3, and EventBridge.
- Understand the network segmentation strategy (public subnet, private subnet, VPC CIDR blocks).
- Draft the initial infrastructure blueprint and identify inter-service dependencies.

---

### Daily Activity Log

| Day | Activity | Date |
|-----|----------|------|
| Monday | Studied the workshop overview document. Mapped out the two primary system zones: **Web Application & API Layer** and the **Machine Learning Pipeline**. | Apr 27 |
| Tuesday | Researched Amazon CloudFront CDN caching mechanisms and how it integrates with an Application Load Balancer as an origin. | Apr 28 |
| Wednesday | Studied AWS Auto Scaling Group launch templates, scaling policies (target tracking), and health check grace periods in relation to EC2 instances behind an ALB. | Apr 29 |
| Thursday | Reviewed Amazon RDS PostgreSQL architecture (primary instance, Multi-AZ, read replicas) and VPC Security Group inbound/outbound rule design for database access isolation. | Apr 30 |
| Friday | Created the first draft of the architecture diagram, listing all components, data flow arrows, and subnet assignments for the workshop infrastructure. | May 1 |

---

### Key Learning Outcomes

- **Architecture Zoning:** Understood why separating the user-facing web layer from the ML pipeline is critical for security isolation and independent scaling.
- **CDN Strategy:** CloudFront reduces origin load by caching static assets (HTML, CSS, JS, product images) at edge locations geographically close to users.
- **Auto Scaling Benefits:** EC2 Auto Scaling Groups automatically adjust capacity in response to real demand metrics (CPU utilization, request count), eliminating manual intervention.
- **RDS Isolation:** Database instances must reside inside private subnets, with Security Group rules permitting inbound port 5432 traffic **only** from the application-tier Security Group ID—not from public IP ranges.

---

*Source: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
