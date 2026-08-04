---
title: "Week 11 Worklog: Kitchen Hub Project - Part 1: Core 3-Tier Infrastructure Setup"
date: 2024-03-11
weight: 1
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Apply the 10 weeks of AWS foundational knowledge to build the 3-Tier architecture for the **Kitchen Hub Project**.
* Provision `KitchenHub-VPC` (Multi-AZ) with Public and Private Subnets across 2 Availability Zones.
* Launch the EC2 Backend (Spring Boot API) and Amazon RDS MySQL Database inside Private Subnets.
* Deploy the Dashboard UI to Amazon S3 Static Website Hosting.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Consolidate Kitchen Hub project requirements and map out the overall architecture <br> - Create Custom VPC `KitchenHub-VPC` (`10.0.0.0/16`) across 2 Availability Zones | 10/20/2025 | 10/20/2025 | [3-Tier Architecture whitepaper](https://aws.amazon.com/blogs/aws/new-whitepaper-designing-3-tier-architectures-on-aws/) |
| Tue | - Set up Internet Gateway, NAT Gateway, and Route Tables for Public/Private Subnets <br> - Configure chained Security Groups: `ALB-SG`, `EC2-App-SG`, `RDS-DB-SG` | 10/21/2025 | 10/21/2025 | [VPC Network Topology Options](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenarios.html) |
| Wed | - Launch Amazon RDS MySQL instance `kitchenhub-db` in Private DB Subnets <br> - Run SQL scripts to initialize tables: `orders`, `menu_items`, `branches`, `users` | 10/22/2025 | 10/22/2025 | [RDS MySQL Multi-AZ Deployments](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html) |
| Thu | - Launch EC2 instance in Private App Subnet <br> - Deploy Spring Boot Backend API and verify DB connection to RDS MySQL | 10/23/2025 | 10/23/2025 | [AWS SDK for Java Development](https://aws.amazon.com/developer/language/java/) |
| Fri | - Create S3 Bucket `kitchen-hub-dashboard-ui` and enable S3 Static Website Hosting <br> - Upload Frontend Dashboard UI assets and verify API integration with EC2 Backend | 10/24/2025 | 10/24/2025 | [S3 Static Website Setup](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html) |

### Week 11 Achievements:

* Built a complete Multi-AZ VPC network infrastructure for the Kitchen Hub project.
* Successfully connected Frontend (S3), Backend (EC2), and Database (RDS MySQL) layers.
* Ensured high security by placing Backend servers and Database inside Private Subnets.
* Achieved the core 3-Tier architecture milestone for the Kitchen Hub Project.
