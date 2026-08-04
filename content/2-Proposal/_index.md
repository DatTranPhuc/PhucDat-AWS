---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Deploying a Production-Style Spring Boot Application on AWS
## Building a Secure, Observable, and Automated Cloud Infrastructure for CenFra-MS

### 1. Executive Summary
The **Deploying a Production-Style Spring Boot Application on AWS** workshop demonstrates how to deploy a containerized Java backend application using a practical cloud architecture rather than relying on a simplified platform-as-a-service provider.

The workshop uses **CenFra-MS** (a franchise and central kitchen management system) as the demonstration application. The backend is developed with Spring Boot and PostgreSQL, containerized using Docker, and deployed to Amazon EC2. 

Incoming requests are resolved through Amazon Route 53, secured and routed through Amazon CloudFront CDN, distributed by an Application Load Balancer (ALB), and forwarded to the Spring Boot container running on EC2. Amazon RDS for PostgreSQL provides managed database storage, while Amazon S3 stores product images and application media. The platform also uses Amazon CloudWatch for centralized container logging and GitHub Actions for automated CI/CD. The React frontend is hosted on Vercel and communicates with the backend through a custom HTTPS domain.

---

### 2. Problem Statement
#### What is the problem?
Many student and personal projects run only on localhost or are deployed using simplified hosting platforms. While these platforms are convenient, they hide important cloud infrastructure concepts such as:
* DNS resolution
* CDN and Edge Caching
* SSL/TLS Encryption
* Load balancing
* Managed databases in private subnets
* Object storage with IAM access
* Centralized logging
* Automated CI/CD pipelines

As a result, developers may know how to build a Spring Boot application but have limited experience deploying and operating it in a realistic cloud environment. Traditional manual deployments also create several problems:
* Deployment steps are repetitive, manual, and error-prone.
* Database and application services run on the same server, raising security concerns.
* Images are stored locally on the server and disappear when the container is rebuilt.
* Logs are difficult to access and analyze without SSH access.
* AWS access keys are stored insecurely in configuration files.

#### Proposed Solution
The proposed workshop introduces a complete AWS deployment architecture for a containerized Spring Boot application. 

The solution uses:
* **User Browser** → **Amazon Route 53** → **Amazon CloudFront** → **Application Load Balancer** → **Amazon EC2 (Docker)** → **Amazon RDS PostgreSQL**.
* **Amazon S3** for persistent image and file storage.
* **Amazon CloudWatch** for container log aggregation.
* **AWS IAM Role** for secure, credential-less access from EC2.
* **GitHub Actions** for automated build and deploy CI/CD workflows.

This architecture separates the web application, backend compute, database, media storage, and monitoring responsibilities.

#### Benefits and Return on Investment (ROI)
* **Practical Learning**: Provides students with hands-on experience in building enterprise-grade cloud architectures.
* **Security & Compliance**: Uses private subnets for RDS, least-privilege IAM Roles, and HTTPS SSL certificates via ACM.
* **Automated CI/CD**: Pushing code to GitHub automatically builds the Docker image and updates the EC2 container.
* **Centralized Observability**: Developers can monitor application logs directly in CloudWatch without SSHing into the EC2 instance.
* **Cost Efficiency**: Establishes a template that can run within the AWS Free Tier or at a low budget of around **$42–$67/month** for a small-scale prototype.

---

### 3. Solution Architecture
The application follows the architecture below:

![CenFra-MS AWS Architecture](/images/2-Proposal/cenframs_architecture.png?v=2)

#### AWS Services Used
* **Amazon Route 53**: Manages public DNS records for the custom domain (e.g., `cenframs.tuandat.space`).
* **Amazon CloudFront**: Acts as the CDN, enforcing HTTPS, SSL termination, and caching static assets.
* **Application Load Balancer (ALB)**: Receives traffic from CloudFront and routes requests to the EC2 target group.
* **Amazon EC2**: Hosts the containerized Spring Boot application running via Docker Compose.
* **Amazon RDS for PostgreSQL**: Managed database instances deployed in private subnets for secure data storage.
* **Amazon S3**: Private bucket for storing upload images (e.g., recipe and ingredient photos).
* **AWS IAM**: EC2 Instance Profile role to grant S3 and CloudWatch permissions securely.
* **Amazon CloudWatch**: Centralizes stdout logs from the Docker container via the `awslogs` driver.
* **GitHub Actions**: Automates the Docker build, push to Docker Hub, and SSH deployment on EC2.

#### Component Design
* **Client Layer**: User interacts with a React frontend hosted on Vercel.
* **DNS & CDN Layer**: Route 53 routes traffic through CloudFront to handle HTTPS.
* **Load-Balancing Layer**: ALB routes requests to healthy EC2 targets using `/actuator/health` checks.
* **Compute Layer**: Spring Boot container running inside Docker on an EC2 instance.
* **Database Layer**: Managed PostgreSQL on RDS restricted to accepting connections only from the EC2 security group.
* **Storage Layer**: Persistent media assets stored in a secure Amazon S3 bucket.

---

### 4. Technical Implementation
#### Implementation Phases
The project is implemented across **4 core phases** within a 3-month (12-week) schedule:
1. **Research & Design (Month 1 / Weeks 1-4)**: Study core AWS concepts (IAM, EC2, EBS, S3) and design the network architecture.
2. **Network & Database Provisioning (Month 2 / Weeks 5-8)**: Create VPC, public/private subnets, Security Groups, Route Tables, and provision the Amazon RDS PostgreSQL instance.
3. **Application Deployment & Routing (Month 2 / Weeks 9-10)**: Containerize the Spring Boot application using Docker, launch the EC2 instance, deploy the container, and set up ALB, Route 53, and CloudFront.
4. **Integration, Monitoring & CI/CD (Month 3 / Weeks 11-12)**: Connect S3 for file upload, set up CloudWatch Logs, configure GitHub Actions workflow, and perform end-to-end testing.

#### Technical Requirements
* **Software**: Java 21, Spring Boot 3.x, Docker, Docker Compose, PostgreSQL, GitHub Actions, Vercel.
* **AWS Services**: VPC, EC2 (t3.micro), RDS PostgreSQL, S3, IAM, CloudWatch, ALB, Route 53, CloudFront.

---

### 5. Timeline & Milestones
* **Pre-Internship (Month 0)**: Initial project proposal and local setup.
* **Month 1 (Weeks 1-4)**: AWS Fundamentals & Account Setup, EC2 & EBS storage basics, S3 hosting.
* **Month 2 (Weeks 5-8)**: VPC networking design, RDS PostgreSQL provisioning, ALB setup.
* **Month 3 (Weeks 9-12)**: Route 53 & CloudFront Integration, AWS Serverless/Messaging, CenFra-MS Core 3-Tier Setup, High Availability testing, CI/CD automation & Final Report.

---

### 6. Budget Estimation
You can find the budget estimation details based on the AWS Pricing Calculator for a continuous 1-month run:

| Service | Estimated Monthly Cost | Description |
| --- | --- | --- |
| **Amazon EC2** | $8.00 – $15.00 | Single t3.micro/t2.micro instance |
| **EBS Storage & IPv4** | $4.00 – $7.00 | 20GB GP3 SSD + Public IPv4 address fee |
| **Amazon RDS PostgreSQL** | $13.00 – $20.00 | Single-AZ db.t3.micro instance (20GB Storage) |
| **Application Load Balancer** | $16.00 – $22.00 | 1 ALB (Low LCU usage) |
| **Amazon Route 53** | $0.50 | 1 Hosted Zone |
| **Amazon S3** | < $1.00 | Small storage (< 5GB) & standard requests |
| **Amazon CloudFront** | ~$0.00 | Free tier covers up to 1TB data transfer out |
| **Amazon CloudWatch** | $0.00 – $2.00 | Logs ingestion and custom dashboard |
| **Estimated Total** | **~$42.00 – $67.00/month** | Standard non-free tier operational cost |

#### Cost-Control Strategy
* Stop EC2 and RDS instances during non-working hours.
* Set CloudWatch log retention to 1 day.
* Configure AWS Budgets with email alerts set at a **$5 limit**.
* Delete the ALB and RDS instance after internship completion to avoid persistent charges.

---

### 7. Risk Assessment
#### Risk Matrix
* **Unexpected AWS Cost**: Medium probability, high impact.
* **Security Group Misconfiguration**: Medium probability, high impact.
* **EC2 Instance / Container Crash**: Low probability, medium impact.
* **Database Connection Timeout**: Low probability, high impact.

#### Mitigation & Contingency
* **Costs**: Configure AWS Budgets and Billing alarms at $5.
* **Security**: Restrict security groups (ALB only open to CloudFront; EC2 only open to ALB; RDS only open to EC2).
* **Crash**: Set Docker restart policy to `unless-stopped` and configure health checks.
* **Backups**: Enable daily automated RDS snapshots.

---

### 8. Expected Outcomes
* **Technical Output**: A fully automated, secure, and observable 3-Tier Spring Boot infrastructure on AWS.
* **Observable Logs**: Centralized container logs in CloudWatch.
* **Automated Pipeline**: A working CI/CD pipeline via GitHub Actions.
* **Security Standards**: Application accessible via HTTPS (`https://cenframs.tuandat.space`).