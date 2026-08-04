---
title : "Overview & Architecture"
date : 2024-01-01 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

#### CenFra-MS Workshop Overview

In this workshop, you will learn how to deploy a containerized Java backend application (**CenFra-MS**, a franchise and central kitchen management system) on a secure, observable, and highly available AWS cloud architecture.

The implementation details include setting up network security rules, launching an Amazon EC2 instance, installing Docker, deploying the backend application via Docker Compose, provisioning an Application Load Balancer (ALB), requesting SSL certificates via AWS Certificate Manager (ACM), routing traffic with Route 53, distributing content via CloudFront CDN, and forwarding logs to CloudWatch.

#### Target Architecture

The application follows the architecture below:

![CenFra-MS AWS Architecture](/images/2-Proposal/cenframs_architecture.png?v=2)

#### Main Goals

* **Step 5.3 (EC2 Deployment)**: Launch an Amazon EC2 instance, install Docker & Docker Compose, and pull the CenFra-MS backend container.
* **Step 5.4 (Application Load Balancer)**: Set up a Target Group on port `8080` and provision an ALB to balance public HTTP traffic.
* **Step 5.5 (Route 53 & CloudFront CDN)**: Configure DNS resolution and CloudFront CDN with SSL termination for secure HTTPS delivery.
* **Step 5.6 (CloudWatch Logs)**: Set up log shipping from Docker containers on EC2 to Amazon CloudWatch for centralized monitoring.
* **Step 5.7 (Resource Clean Up)**: Terminate resources to prevent unnecessary AWS charges.