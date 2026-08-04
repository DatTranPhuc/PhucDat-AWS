---
title : "Clean Up CenFra-MS Resources"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---
Congratulations on completing this workshop! 

In this workshop, you learned how to deploy a containerized Spring Boot backend application (**CenFra-MS**) on a robust production-style AWS architecture:
* You set up an Amazon EC2 instance and deployed the application container via Docker Compose.
* You set up an Application Load Balancer and registered EC2 instances using target groups.
* You integrated Route 53 custom domain name resolution and CloudFront CDN for HTTPS access.
* You configured container log aggregation to Amazon CloudWatch.

#### Clean Up Resources

To avoid incurring unexpected AWS charges, follow these steps to clean up all provisioned resources:

1. **Delete Route 53 Records**:
   * Navigate to the **Route 53 Console** -> **Hosted Zones**.
   * Select your hosted zone and delete the `A Record` pointing to the CloudFront distribution.

2. **Disable & Delete CloudFront Distribution**:
   * Open the **CloudFront Console**.
   * Select your distribution, click **Disable**, and wait for status to turn to disabled.
   * Once disabled, select it again and click **Delete**.

3. **Delete Application Load Balancer (ALB)**:
   * Open the **EC2 Console** -> **Load Balancers**.
   * Select `cenframs-alb` and click **Actions** -> **Delete**.
   * Navigate to **Target Groups** and delete the `cenframs-tg` target group.

4. **Terminate EC2 Instance**:
   * Navigate to **EC2 Instances**, select the Spring Boot EC2 backend server, and click **Instance state** -> **Terminate instance**.

5. **Delete RDS PostgreSQL Database**:
   * Navigate to the **RDS Console** -> **Databases**.
   * Select your database instance, click **Actions** -> **Delete**.
   * Choose not to take a final snapshot (if just for demonstration), confirm, and click **Delete**.

6. **Delete CloudWatch Log Group**:
   * Go to **CloudWatch Logs** -> **Log groups**.
   * Select `/cenfra-ms/app` and click **Actions** -> **Delete log group**.