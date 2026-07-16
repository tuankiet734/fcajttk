---
title: "Configure Network and Security"
date: 2024-07-07
weight: 3
chapter: false
pre : " <b> 5.3.3. </b> "
---

### 5.3.3. Configure Network and Security (Security & Network Groups)

To ensure secure connections for both databases, we need to configure the Inbound Rules of the VPC Security Group. Below is the detailed step-by-step guide:

1. Log in to the AWS Console, search for the **EC2** service, and select **Security Groups** under the **Network & Security** section in the left navigation menu.

![Find Security Groups on EC2 Console](/images/5-Workshop/5.3-Database-setup/5.3.3-step01-ec2-security-groups.png)

2. Select the specific Security Group attached to your RDS instances.

![Select RDS Security Group](/images/5-Workshop/5.3-Database-setup/5.3.3-step02-select-sg.png)

3. Click on the **Inbound rules** tab at the bottom and click **Edit inbound rules**.

![Edit Inbound Rules](/images/5-Workshop/5.3-Database-setup/5.3.3-step03-edit-inbound.png)

4. Add the first rule (Rule 1): Select Type as `PostgreSQL` (port `5432`), and select Source as **My IP** to allow access from your local developer machine.

![Configure Rule 1 for My IP](/images/5-Workshop/5.3-Database-setup/5.3.3-step04-rule1-myip.png)

5. Add the second rule (Rule 2): Select Type as `PostgreSQL`, select Source as **Custom** and enter the Security Group ID of the `ML-Forecast-Server`.

![Configure Rule 2 for EC2 Server](/images/5-Workshop/5.3-Database-setup/5.3.3-step05-rule2-ec2sg.png)

6. Add the third rule (Rule 3): Select Type as `PostgreSQL`, select Source as **Custom** and enter the current Security Group's own ID to establish a self-referencing rule for AWS Glue connections.

![Configure Rule 3 for AWS Glue self-reference](/images/5-Workshop/5.3-Database-setup/5.3.3-step06-rule3-selfreference.png)

7. Review all configured rules and click the orange **Save rules** button to apply.

![Save Security Group Rules Successfully](/images/5-Workshop/5.3-Database-setup/5.3.3-step07-saved-rules.png)