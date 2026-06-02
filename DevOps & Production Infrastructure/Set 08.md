# DevOps & Production Infrastructure: Set 8 
## Cloud Scaling, Cost Optimization (FinOps) & Disaster Recovery

## 📘 10 Conceptual Questions
1. **Horizontal vs. Vertical Scaling in the Cloud:** What is the difference between scaling up (larger instance) and scaling out (more instances)? Why is horizontal scaling via Auto Scaling Groups (ASGs) the only sustainable path for a Laravel app, and what stateful bottlenecks (sessions, file uploads) must be solved first?
2. **FinOps & Cloud Waste:** What is FinOps? What are the top 3 hidden causes of cloud bill explosions (e.g., unattached EBS volumes, over-provisioned RDS, idle Load Balancers), and how do you proactively identify and eliminate them?
3. **Spot vs. On-Demand vs. Reserved Instances:** How do these pricing models differ in cost and reliability? How can you safely use Spot instances for stateless PHP queue workers without losing jobs when AWS reclaims the server?
4. **RTO vs. RPO in Practice:** Define Recovery Time Objective (how fast you recover) and Recovery Point Objective (how much data you lose). How do these business metrics dictate your technical choices for database replication and backup frequency?
5. **Storage Lifecycle Policies:** How does object storage lifecycle management save massive amounts of money? Explain the transition tiers (Standard → Infrequent Access → Glacier/Archive) and when to apply them to Laravel backups and user uploads.
6. **Multi-AZ vs. Multi-Region:** What is the architectural difference between an Availability Zone (AZ) and a Region? When is Multi-AZ high-availability enough, and when do you actually need Multi-Region active-active or active-passive disaster recovery?
7. **Immutable Backups & Ransomware Protection:** Why are standard backups vulnerable to ransomware? How do immutable backups (WORM - Write Once, Read Many) and isolated backup accounts protect against malicious or accidental deletion?
8. **Auto-Scaling Triggers:** Why is CPU usage alone a terrible metric for scaling Laravel web or queue workers? How do you scale based on custom metrics like SQS `ApproximateNumberOfMessagesVisible` or Application Load Balancer `TargetResponseTime`?
9. **Point-in-Time Recovery (PITR):** How does PITR work in managed databases (e.g., AWS RDS)? Why are hourly snapshots insufficient, and how do Write-Ahead Logs (WAL) or binary logs enable second-granularity recovery?
10. **The Cost of Downtime vs. Resilience:** How do you justify the budget for a Multi-Region DR setup to non-technical management? Explain the ROI calculation of avoiding a 4-hour outage versus the monthly cost of redundant infrastructure.

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Auto Scaling Group (ASG) Setup** | Provision an ASG for stateless PHP workers. Configure scaling policies: scale out at 60% CPU, scale in at 30%. Attach it to an Application Load Balancer. Verify instances spawn and terminate automatically under load. |
| 2 | **Cloud Cost Analysis & Tagging** | Enable AWS Cost Explorer / GCP Billing. Implement a strict tagging policy (`Environment`, `Project`, `Owner`). Create a monthly budget and configure an SNS/Email alert when spend hits 80% of the forecast. |
| 3 | **S3 Lifecycle Policy for Backups** | Create an S3 bucket for database backups. Configure a lifecycle rule: transition objects older than 30 days to S3-Infrequent Access, and older than 90 days to Glacier. Verify the rule applies to existing and new objects. |
| 4 | **Database Point-in-Time Recovery (PITR)** | Enable automated backups on an RDS instance. Insert test data, then simulate a catastrophic deletion (drop a table). Restore the database to a Point-in-Time *exactly 1 minute before* the deletion. |
| 5 | **Spot Instance Queue Worker** | Launch an EC2 Spot Instance to run a Laravel queue worker. Configure the worker to gracefully handle `SIGTERM` (the 2-minute AWS reclaim warning) by letting the current job finish and releasing the lock. |
| 6 | **Multi-AZ Database Failover Drill** | Enable Multi-AZ on your database. Manually trigger a failover via the cloud console. Measure the exact downtime (in seconds) using a continuous `ping` or health check script. Verify DNS updates automatically. |
| 7 | **Automated Encrypted Backup Script** | Write a bash script that dumps the database, compresses it, encrypts it using GPG or AWS KMS, uploads it to S3, and deletes the local file. Schedule it via cron. |
| 8 | **Immutable Backup Vault (WORM)** | Create an S3 bucket with Object Lock enabled in "Compliance" mode for 30 days. Upload a backup file. Attempt to delete or overwrite the file via CLI and verify the operation is rejected. |
| 9 | **Custom Metric Scaling (Queue Depth)** | Publish the Laravel queue length to CloudWatch (via a cron job or package). Configure your ASG to scale out when the queue depth exceeds 100 messages, and scale in when it drops to 0. |
| 10| **Disaster Recovery Runbook** | Write a comprehensive `DISASTER_RECOVERY.md`. Include step-by-step procedures for: Total Region Failure, Database Corruption, and Accidental `terraform destroy`. Include estimated RTO/RPO for each scenario. |
