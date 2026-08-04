---
title: "Week 6 Worklog: Amazon RDS Basics (MySQL Database & Backups)"
date: 2024-02-05
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Learn Amazon RDS (Relational Database Service) fundamentals.
* Provision an Amazon RDS MySQL Instance in the cloud.
* Configure Security Groups allowing application/EC2 instances to connect on Port 3306.
* Connect a database client, run DDL/DML SQL queries, and set up Automated Backups.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Study Amazon RDS concepts: Database Engines (MySQL, PostgreSQL), DB Instance Classes & Storage Types | 09/15/2025 | 09/15/2025 | [What is Amazon RDS?](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html) |
| Tue | - Provision an Amazon RDS MySQL Instance `my-sample-mysql-db` (Free Tier, Single-AZ) <br> - Configure master username and password | 09/16/2025 | 09/16/2025 | [Creating a MySQL DB Instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.MySQL.html) |
| Wed | - Set Security Group inbound rule to allow port 3306 access <br> - Test connection from MySQL Workbench / DBeaver to the RDS Endpoint | 09/17/2025 | 09/17/2025 | [Working with RDS Security Groups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithSecurityGroups.html) |
| Thu | - Create a database `test_db`, create table `users`, and execute SQL CRUD statements (Insert, Select, Update, Delete) | 09/18/2025 | 09/18/2025 | [Connecting to a MySQL DB Instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ConnectToInstance.html) |
| Fri | - Study Automated Backups and DB Snapshots <br> - Create a manual DB Snapshot and restore a new RDS instance from it | 09/19/2025 | 09/19/2025 | [RDS Backup & Restore](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_CommonTasks.BackupRestore.html) |

### Week 6 Achievements:

* Understood how to provision and manage relational databases on Amazon RDS.
* Connected external and internal DB clients to Amazon RDS instances.
* Mastered SQL database operations on cloud-managed database instances.
* Learned automated backup configuration and snapshot-based restoration procedures.
