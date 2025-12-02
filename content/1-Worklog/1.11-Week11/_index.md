---
title: "Week 11 Worklog"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 11 Objectives:

* AWS Elastic Disaster Recovery Workshop
* Database

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Implement AutoScaling for WordPress Instance                                                                                                | 11/17/2025 | 11/17/2025      |<https://000101.awsstudygroup.com/4-asgforec2/>|
| 3   | - Initialize AMI from WebServer Instance <br>&emsp; + Launch Template <br>&emsp; + Create Load Balancer <br>&emsp; + Create Auto Scaling Group <br>&emsp; + Database <br>&emsp; + ... <br>                                              | 11/18/2025 | 11/18/2025     | <https://000101.awsstudygroup.com/4-asgforec2/4.5-createasg/> |
| 4   | - Backup and Restore Database <br> - **Practice:** <br>&emsp; + Create DB Snapshot <br>&emsp; + Restore with DB Snapshot | 11/19/2025| 11/19/2025     | <https://000101.awsstudygroup.com/5-backupandrestore/5.2-restorewithsnapshot/> |
| 5   | - Create CloudFront for Web Server                  | 11/20/2025 | 11/20/2025      | <https://000101.awsstudygroup.com/6-createcloudfront/> |
| 6   | - Introduction to Infrastructure as Code                                                                          | 11/21/2025 | 11/21/2025     | <https://000102.awsstudygroup.com/2-pre/> |


### Week 11 Achievements:

* Implemented Auto Scaling for WordPress instances to handle traffic fluctuations:
  * Configured Auto Scaling policies based on CPU utilization and other metrics.
  * Set up scaling triggers and thresholds for automatic instance provisioning.
  * Tested scaling behavior under various load conditions.

* Created scalable web infrastructure with AMI and Load Balancer:
  * Generated custom AMI from configured web server instance.
  * Designed and implemented Launch Template with proper instance configurations.
  * Deployed Application Load Balancer for traffic distribution across instances.
  * Successfully created and configured Auto Scaling Group for high availability.

* Mastered database backup and disaster recovery procedures:
  * Created automated and manual database snapshots for point-in-time recovery.
  * Practiced database restoration from snapshots with minimal downtime.
  * Implemented backup retention policies and cross-region backup strategies.
  * Validated data integrity and consistency after restoration processes.


* Overall: Completed comprehensive disaster recovery and scalability workshop covering auto scaling, database backup/restore, content delivery acceleration, and infrastructure automation foundations.
