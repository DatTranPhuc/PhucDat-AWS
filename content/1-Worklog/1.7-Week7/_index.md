---
title: "Week 7 Worklog: Elastic Load Balancing (ALB Basics & Health Checks)"
date: 2024-02-12
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Understand Elastic Load Balancing (ELB) solutions on AWS.
* Differentiate between Application Load Balancer (ALB) and Network Load Balancer (NLB).
* Create an Application Load Balancer (ALB), Target Groups, and configure Health Checks.
* Practice distributing web traffic across multiple EC2 instances.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Study ELB concepts: Listeners, Rules, Target Groups, Health Checks & Cross-Zone Load Balancing | 09/22/2025 | 09/22/2025 | [What is Elastic Load Balancing?](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html) |
| Tue | - Launch 2 EC2 Web Server instances (`EC2-Web-1` and `EC2-Web-2`) in different Availability Zones | 09/23/2025 | 09/23/2025 | [EC2 Getting Started Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html) |
| Wed | - Create a Target Group `web-servers-tg` and register both EC2 instances <br> - Set Health Check path to `/index.html` (Interval: 30 seconds) | 09/24/2025 | 09/24/2025 | [ELB Target Groups](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html) |
| Thu | - Create Application Load Balancer `My-Demo-ALB` in Public Subnets <br> - Configure HTTP Listener (port 80) routing rules to target group `web-servers-tg` | 09/25/2025 | 09/25/2025 | [Getting Started with ALB](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/application-load-balancer-getting-started.html) |
| Fri | - Access ALB DNS Name in browser and refresh repeatedly to observe load balancing across instances <br> - Stop one EC2 instance to verify ALB Health Check automatic failover detection | 09/26/2025 | 09/26/2025 | [Target group health checks](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/target-group-health-checks.html) |

### Week 7 Achievements:

* Understood Layer 7 Application Load Balancing operations with AWS ALB.
* Created Target Groups and configured active Health Check monitoring.
* Distributed web traffic evenly across multiple backend compute instances.
* Ensured application fault tolerance during single-instance server outages.
