---
title : "Target Group"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Overview

In this section, a target group is created to define the backend EC2 instance that the Application Load Balancer will forward traffic to. The target group uses the **Instance** target type, the **HTTP** protocol, and port **8080**, which matches the port used by the containerized application.

The main goals of this section are:

+ Create a target group for the backend EC2 instance.
+ Register the EC2 instance as a target.
+ Verify that the target is healthy.

#### Create target group

A target group named **aws-c8n** is created with **Instance** as the target type. The protocol is **HTTP**, the target port is **8080**, and the protocol version is **HTTP1**. The health check path is `/swagger-ui/index.html#/` and the expected success code is **200**.

![Target group review](/images/5-Workshop/5.4-Load-Balancer/target-group-review.png)

#### Register EC2 target

The running **CenFra-MS** EC2 instance is selected as the backend target. The target port is set to **8080**, matching the port used by the application container.

![Register target](/images/5-Workshop/5.4-Load-Balancer/register-target.png)

#### Verify target health

After the target is registered, the target group shows **1 healthy target** and **0 unhealthy targets**. This confirms that the load balancer can forward traffic to the backend instance.

![Healthy target group](/images/5-Workshop/5.4-Load-Balancer/target-group-healthy.png)

#### Target Group summary

At the end of this section, the target group **aws-c8n** is active with one healthy EC2 instance registered on port **8080**. This target group will be used by the Application Load Balancer in the next section to route incoming HTTP traffic to the backend application.
