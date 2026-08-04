---
title: "Week 5 Worklog: Amazon VPC Networking Basics (Subnets & Security Groups)"
date: 2024-01-29
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Learn Amazon VPC (Virtual Private Cloud) networking fundamentals.
* Practice creating a Custom VPC, Public Subnets, Private Subnets, and Route Tables.
* Attach an Internet Gateway (IGW) and provision a NAT Gateway for outbound connectivity.
* Understand the security role of Security Groups (Stateful) vs Network ACLs (Stateless).

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Study AWS VPC concepts: CIDR notation (`10.0.0.0/16`), Subnets, Route Tables, and IP Routing | 09/08/2025 | 09/08/2025 | [What is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) |
| Tue | - Create Custom VPC `My-Lab-VPC` (`10.0.0.0/16`) <br> - Provision 1 Public Subnet (`10.0.1.0/24`) and 1 Private Subnet (`10.0.2.0/24`) | 09/09/2025 | 09/09/2025 | [VPC Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html) |
| Wed | - Attach Internet Gateway (IGW) to the VPC <br> - Configure Route Table for Public Subnet to route internet traffic through IGW | 09/10/2025 | 09/10/2025 | [Internet Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) |
| Thu | - Provision a NAT Gateway in the Public Subnet <br> - Configure Route Table for Private Subnet to allow outbound internet access via NAT Gateway | 09/11/2025 | 09/11/2025 | [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) |
| Fri | - Launch EC2 instances in Public and Private Subnets <br> - Test SSH jump-box connectivity and verify Security Group rules | 09/12/2025 | 09/12/2025 | [Security Groups & NACLs](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html) |

### Week 5 Achievements:

* Understood Amazon VPC virtual networking concepts.
* Successfully built a custom network with Public and Private Subnets.
* Differentiated operational roles between Internet Gateways and NAT Gateways.
* Secured cloud network traffic using Security Groups and Network ACLs.
