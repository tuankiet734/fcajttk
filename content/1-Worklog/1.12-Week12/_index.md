---
title: "Week 12"
date: 2026-07-06
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

# Work Log: Final Documentation, Workshop Reporting & Program Wrap-Up

> **Week 12 - July 6, 2026:** Dedicated the final week of the First Cloud AI Journey program to completing all outstanding documentation, writing the comprehensive project technical report, reviewing learnings across all 12 weeks, and preparing the final presentation for program facilitators. This week marks the formal conclusion of the internship, ending on **July 12, 2026**.

---

### Weekly Objectives

- Complete all **Hugo workshop documentation** pages for the published workshop site, ensuring every section is fully written, accurate, and consistent with the deployed implementation.
- Write the **final technical project report** summarizing architecture decisions, implementation challenges, performance results, and lessons learned across all 10 workshop phases.
- Conduct a **12-week retrospective review**: identify the most impactful skills acquired, the most challenging obstacles overcome, and areas for continued development.
- Prepare and submit all required **program deliverables**: worklog entries, workshop documentation site, project demo video link, and the final report.
- Reflect on the **end-to-end AWS cloud workflow** practiced throughout the program—from account setup through serverless ML deployment—and outline a personal roadmap for AWS certification.

---

### Daily Activity Log

| Day | Activity | Date |
|-----|----------|—---|
| Monday | Finalized and published all **Hugo Workshop documentation sections**: completed the remaining subsection pages for Network Setup, Database Setup, Feature Extraction, Model Training, Model API, Web Portal, and EC2 Deployment; verified all internal links, code block syntax, and diagram references were accurate. | Jul 6 |
| Tuesday | Drafted the **Technical Architecture Report**: documented the system architecture rationale (dual-zone design, dual-database pattern, serverless inference), detailed the AWS services utilized and their configurations, and summarized performance benchmarks (Lambda p99 inference latency, LightGBM MAPE scores, ASG scaling event logs). | Jul 7 |
| Wednesday | Wrote the **ML Pipeline Report**: detailed the feature engineering methodology (lag features, rolling averages, velocity metrics), LightGBM hyperparameter tuning process, model evaluation results per store and SKU segment, and recommendations for improving forecast accuracy (additional features, alternative algorithms, automated hyperparameter optimization). | Jul 8 |
| Thursday | Completed the **12-Week Worklog**: finalized all weekly entry summaries, cross-referenced activities against the original program schedule, ensured consistent formatting across all entries, and integrated the project demo video link into the Week 11 documentation. | Jul 9 |
| Friday | Performed a final **self-assessment review** against program learning objectives, organized all project assets (code repository, report files, demo video, workshop site URL) into a submission package and conducted an internal quality check. | Jul 11 |
| Saturday | Submitted all **final program deliverables**: uploaded the Hugo workshop site URL, the Google Drive demo video link, the completed worklog, and the technical report to the program submission portal. Completed program sign-off. **Official program end date: July 12, 2026.** | Jul 12 |

---

### 12-Week Program Retrospective

**Skills Acquired:**
- End-to-end cloud infrastructure provisioning on AWS (VPC, EC2, RDS, S3, CloudFront, ALB, ASG).
- Serverless computing architecture design and implementation (Lambda, API Gateway, EventBridge).
- Big data ETL pipeline development with AWS Glue (PySpark, JDBC connections, job bookmarks).
- Machine learning model training, evaluation, and production deployment on cloud infrastructure.
- Full-stack web application development with Node.js, JWT authentication, TOTP MFA, and RBAC access control.
- Infrastructure cost management through budget alerting, resource cleanup discipline, and right-sizing strategies.

**Key Challenges Overcome:**
- **Glue JDBC Security Group Configuration:** Discovering that Glue ENI CIDR ranges must be explicitly allowed in the RDS Security Group (not just the application Security Group) required understanding Glue's underlying network architecture.
- **Lambda Layer Binary Compatibility:** Building Python ML library layers (LightGBM with C extensions) on a non-Amazon-Linux development machine produced incompatible binaries; switching to Docker-based layer builds resolved the issue.
- **RDS Deletion Dependency Chain:** Attempting to delete the RDS cluster before its constituent instances consistently triggered `DependencyViolation` errors, requiring the correct instance-first, cluster-second deletion sequence.
- **PM2 Startup Systemd Integration:** The two-step PM2 startup process (running `pm2 startup` and then manually executing the generated `sudo env PATH=...` command) was not immediately intuitive but is essential for post-reboot persistence.

**AWS Certification Roadmap:**
1. **AWS Cloud Practitioner (CLF-C02)** — Foundation validation (already prepared through Week 1–3 activities).
2. **AWS Solutions Architect Associate (SAA-C03)** — Next target (6–8 weeks of dedicated study, focusing on VPC, IAM, storage, database, and compute deep-dives).
3. **AWS Machine Learning Specialty (MLS-C01)** — Long-term goal after completing the SAA, building on the ML pipeline experience from Weeks 6–8.

---

### Program Summary & Final Takeaways

- **Total AWS Services Used:** 12 distinct services across compute, storage, networking, data, ML, and security domains.
- **Total Workshop Phases Completed:** 10 phases (Architecture → Network → Database → ETL → Training → API → Portal → Deployment → Cleanup → Demo).
- **Total Credits Invested:** $200 in program credits effectively utilized across all workshop phases and guided tasks.
- **Most Valuable Lesson:** The most impactful insight from this program was that **cloud architecture is fundamentally about managing dependencies**—between services, between security boundaries, between data tiers, and between deletion sequences. Every configuration decision carries downstream implications that must be planned before the first resource is provisioned.

---

*Source: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
