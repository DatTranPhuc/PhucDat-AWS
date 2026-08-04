---
title : "Load Balancer"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Overview

In this section, an Application Load Balancer is configured to expose the EC2-hosted application through a public HTTP endpoint. The load balancer receives client requests on port **80** and forwards them to a target group that contains the backend EC2 instance on port **8080**.

The main goals of this section are:

+ Create a target group for the backend EC2 instance.
+ Register the EC2 instance as a target.
+ Verify that the target is healthy.
+ Create an internet-facing Application Load Balancer.
+ Configure HTTP listener and routing rules.
+ Verify that the load balancer is active.

#### Create target group

A target group named **aws-c8n** is created with **Instance** as the target type. The protocol is **HTTP**, the target port is **8080**, and the protocol version is **HTTP1**. The health check path is `/swagger-ui/index.html#/` and the expected success code is **200**.

![Target group review](/images/5-Workshop/5.4-Load-Balancer/target-group-review.png)

#### Register EC2 target

The running **CenFra-MS** EC2 instance is selected as the backend target. The target port is set to **8080**, matching the port used by the application container.

![Register target](/images/5-Workshop/5.4-Load-Balancer/register-target.png)

#### Verify target health

After the target is registered, the target group shows **1 healthy target** and **0 unhealthy targets**. This confirms that the load balancer can forward traffic to the backend instance.

![Healthy target group](/images/5-Workshop/5.4-Load-Balancer/target-group-healthy.png)

#### Choose load balancer type

AWS provides several load balancer types. For this workload, **Application Load Balancer** is selected because the application uses HTTP traffic. ALB is suitable for web applications, APIs, microservices, and container-based services.

![Load balancer types](/images/5-Workshop/5.4-Load-Balancer/lb-types.png)

#### Configure Application Load Balancer

The Application Load Balancer is named **c8n-aws-ALB**. It is configured as **Internet-facing**, which means it can receive traffic from the public internet. The IP address type is **IPv4**.

![ALB basic configuration](/images/5-Workshop/5.4-Load-Balancer/alb-basic-config.png)

The load balancer is mapped to the selected VPC and four public subnets across four Availability Zones. This improves availability because the load balancer can receive and route traffic through multiple zones.

![ALB network mapping](/images/5-Workshop/5.4-Load-Balancer/alb-network-mapping.png)

#### Configure listener and routing

The load balancer listens on **HTTP:80**. The default routing action forwards requests to the **aws-c8n** target group with 100% traffic weight.

![ALB listener](/images/5-Workshop/5.4-Load-Balancer/alb-listener.png)

Before creating the load balancer, the review page confirms the final settings: internet-facing scheme, IPv4 address type, selected VPC, four subnets, security group, and HTTP listener forwarding to one target group.

![ALB review](/images/5-Workshop/5.4-Load-Balancer/alb-review.png)

#### Configure load balancer security group

The load balancer security group allows inbound **HTTP** traffic on port **80** from `0.0.0.0/0`. This rule allows users on the internet to access the application through the ALB DNS name.

![ALB security group](/images/5-Workshop/5.4-Load-Balancer/alb-security-group.png)

{{% notice note %}}
For production environments, inbound rules should be restricted as much as possible. Public HTTP access is acceptable for a workshop demonstration, but HTTPS and stricter access controls are recommended for real systems.
{{% /notice %}}

#### Verify Application Load Balancer

After creation, the EC2 console shows **c8n-aws-ALB** with the **Active** state. The load balancer type is **Application**, the scheme is **Internet-facing**, and AWS assigns a DNS name for client access.

![Active ALB](/images/5-Workshop/5.4-Load-Balancer/alb-active.png)

#### Load Balancer summary

At the end of this section, the Application Load Balancer is active and ready to route public HTTP traffic to the healthy EC2 backend target. This architecture separates the public entry point from the backend application instance and improves availability by using multiple Availability Zones.
