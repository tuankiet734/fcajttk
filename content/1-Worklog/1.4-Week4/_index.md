---
title: "Week 4"
date: 2026-05-11
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

# Work Log: Network Setup – CloudFront, ALB, EC2 Web Servers, Target Groups & Auto Scaling

> **Week 4 - May 11, 2026:** Began hands-on provisioning of the complete network infrastructure layer for the Fashion Retail system. This week covered configuring the CloudFront CDN distribution, deploying Application Load Balancers, registering Target Groups, launching EC2 web server instances, and setting up Auto Scaling Groups with health-check-based scaling policies.

---

### Weekly Objectives

- Configure an **Amazon CloudFront Distribution** pointing to the External ALB as its primary origin.
- Deploy an **External Application Load Balancer** for public-facing HTTP/HTTPS traffic distribution.
- Launch an **EC2 Web Server instance** (`Web-Application-Server`) using a custom AMI with the application pre-installed.
- Register the web server with a **Target Group** and attach it to the ALB listener rules.
- Configure an **Auto Scaling Group** (ASG) with minimum/maximum capacity and target tracking scaling policies.
- Interconnect all network layers to ensure end-to-end request flow: CloudFront → ALB → Target Group → EC2 ASG.

---

### Daily Activity Log

| Day | Activity | Date |
|-----|----------|------|
| Monday | Created the **Amazon CloudFront Distribution**: configured origin domain to point to the External ALB DNS name, disabled caching for dynamic API paths using Cache-Control headers, and enabled HTTPS-only viewer protocol policy. | May 11 |
| Tuesday | Provisioned the **External Application Load Balancer** inside the public subnet: configured listeners on port 80 (HTTP) and port 443 (HTTPS), set up an SSL certificate from ACM, and enabled access logging to an S3 bucket. | May 12 |
| Wednesday | Launched the **EC2 Web Server** (`Web-Application-Server`): selected `t3.medium` instance type, configured the network to the public subnet, assigned the public-facing Security Group (ports 80, 443, 22), and applied a custom user-data bootstrap script. | May 13 |
| Thursday | Created and configured the **Target Group**: registered the web server EC2 instance, set the health check path to `/health`, configured healthy/unhealthy threshold counts, and validated health status in the console. | May 14 |
| Friday | Configured the **Auto Scaling Group** for the web tier: created a launch template from the tested AMI, set desired=2, min=1, max=4 capacity, and attached a Target Tracking Scaling Policy based on ALB request count per target (threshold: 500 RPS). | May 15 |

---

### Key Learning Outcomes

- **CloudFront Origin Configuration:** Setting the ALB DNS name as the CloudFront origin allows the CDN to route cache misses directly to the load balancer without exposing the EC2 instances' public IPs.
- **ALB Listener Rules:** HTTP-to-HTTPS redirect rules ensure all user traffic is encrypted from the first request, preventing man-in-the-middle attacks on unencrypted sessions.
- **Target Group Health Checks:** Configuring accurate health check endpoints (`/health` returning HTTP 200) is critical—an incorrect path causes the ASG to permanently cycle instances in and out of service.
- **Launch Templates vs. Launch Configurations:** Launch Templates support versioning and mixed instance types (Spot + On-Demand combinations), making them the recommended approach for new ASG deployments.
- **Service Interconnection Verification:** End-to-end testing by hitting the CloudFront URL and tracing request routing through X-Ray confirmed that all hops (CloudFront → ALB → Target Group → EC2) functioned correctly.

---

*Source: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
