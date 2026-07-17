---
title: "Week 10"
date: 2026-06-22
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

# Work Log: EC2 Production Deployment, Nginx Reverse Proxy, PM2 & SSL Certificate Setup

> **Week 10 - June 22, 2026:** Deployed the Web Portal application to the cloud production environment on Amazon EC2. This week covered provisioning the production EC2 instance, installing the Node.js runtime stack, deploying the application code from GitHub, configuring PM2 for persistent process management, setting up Nginx as a reverse proxy, and provisioning a free SSL/TLS certificate with Certbot/Let's Encrypt.

---

### Weekly Objectives

- Launch the **production EC2 instance** (`fashion-web-server`) with appropriate AMI, instance type, Security Group rules, and storage configuration.
- Establish an **SSH connection** and install all required base dependencies (Node.js 20.x, Nginx, PM2, AWS CLI, PostgreSQL client).
- Clone the application repository, configure the **secure `.env` file** with production RDS endpoints, S3 bucket name, API Gateway URL, and JWT secret.
- Configure **PM2** to launch the Express.js application on boot and restart it automatically on crashes.
- Configure **Nginx** as a reverse proxy routing port 80 traffic to the local Node.js application on port 3000.
- Provision an **SSL/HTTPS certificate** using Certbot and Let's Encrypt, enabling secure HTTPS access.

---

### Daily Activity Log

| Day | Activity | Date |
|-----|----------|------|
| Monday | Launched the **production EC2 instance**: selected `t3.large` (2 vCPUs, 8 GiB RAM) instance type, `Ubuntu Server 22.04 LTS` AMI, created key pair `fashion-key.pem`, and configured Security Group `web-app-sg` with inbound rules for SSH (port 22), HTTP (port 80), HTTPS (port 443), and internal VPC access for Node.js (port 3000) and FastAPI (port 8000). Allocated 20 GiB gp3 SSD storage. | Jun 22 |
| Tuesday | Connected via SSH and installed all **base dependencies**: updated system packages (`apt update && apt upgrade -y`), installed Nginx, added Node.js 20.x repository and installed Node, installed PM2 globally (`npm install -g pm2`), and installed AWS CLI and PostgreSQL client tools. | Jun 23 |
| Wednesday | Cloned the application repository into `/home/ubuntu/app` and created the **production `.env` file** using Nano: configured `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, `DB_PASSWORD` for `training-db`, `AWS_REGION`, `S3_BUCKET_NAME`, `API_GATEWAY_URL`, `JWT_SECRET`, and `NODE_ENV=production`. Ran `npm install --production` to install application dependencies. | Jun 24 |
| Thursday | Configured **PM2**: launched the Express.js application with `pm2 start app.js --name "fashion-web" --env production`, verified the process displayed `online` status in `pm2 list`, ran `pm2 startup` to register the PM2 daemon with systemd, and saved the process list with `pm2 save`. | Jun 25 |
| Friday | Configured **Nginx reverse proxy**: created site configuration at `/etc/nginx/sites-available/fashion-app` with proxy_pass to `http://localhost:3000`, activated the configuration via symlink, removed the default config, verified syntax with `nginx -t`, and restarted Nginx. Provisioned the **SSL certificate** using `certbot --nginx -d your-domain.com` and verified automated renewal with `certbot renew --dry-run`. | Jun 26 |

---

### Key Learning Outcomes

- **t3.large for Web Portal Workloads:** The Portal renders interactive Plotly.js charts and Mapbox GL JS 3D maps on the server side; `t3.micro` instances proved insufficient (OOM errors), making `t3.large` (8 GiB RAM) the minimum viable instance class.
- **PM2 Startup Integration:** Running `pm2 startup` generates a `sudo env PATH=...` command that must be copied and executed manually to register PM2 with systemd. Without this step, the application will not restart after an EC2 reboot or a kernel update.
- **Nginx Proxy Buffering:** Setting `proxy_read_timeout 90` in the Nginx config prevents connection drops during high-latency Plotly data fetch requests (Lambda cold start can add 3–5 seconds to the first inference call).
- **Let's Encrypt Auto-Renewal:** Certbot installs a systemd timer (`certbot.timer`) that automatically renews certificates before the 90-day expiry. The `--dry-run` flag simulates the renewal process without consuming API rate limits.
- **Security Group Dependency Order:** EC2 Security Group rules must be configured before launching the instance. Retroactively editing Security Group rules after deployment requires verifying that running Nginx processes reflect the updated firewall rules without requiring an application restart.

---

*Source: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
