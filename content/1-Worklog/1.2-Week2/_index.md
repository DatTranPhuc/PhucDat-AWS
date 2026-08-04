---
title: "Week 2 Worklog: Amazon EC2 Basics (Instances, SSH & User Data)"
date: 2024-01-08
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

* Understand Amazon EC2 (Elastic Compute Cloud) virtual server concepts.
* Launch EC2 instances (Amazon Linux / Ubuntu), generate Key Pairs, and configure Security Groups.
* Practice connecting to EC2 instances via SSH from the local computer.
* Automate Nginx web server installation using EC2 User Data scripts.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Study Amazon EC2 fundamentals: Instance Types (`t3.micro`), Elastic IP, Key Pairs & Security Groups | 08/18/2025 | 08/18/2025 | [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/) <br> [EC2 Security Groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html) |
| Tue | - Launch an EC2 Instance (Amazon Linux 2023) in the Default VPC <br> - Generate Key Pair `my-ec2-key.pem` and set Security Group inbound rule for SSH (port 22) | 08/19/2025 | 08/19/2025 | [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) <br> [Security group rules](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/security-group-rules.html) |
| Wed | - SSH into the EC2 instance from terminal (`ssh -i my-ec2-key.pem ec2-user@<Public-IP>`) <br> - Run basic Linux administration commands to verify OS version, disk, CPU, and RAM | 08/20/2025 | 08/20/2025 | [Connect to Linux instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) |
| Thu | - Launch an EC2 instance with User Data script: `#!/bin/bash sudo yum update -y && sudo yum install -y nginx && sudo systemctl start nginx` <br> - Add HTTP (port 80) inbound rule and verify Nginx landing page in browser | 08/21/2025 | 08/21/2025 | [EC2 User Data scripts](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html) |
| Fri | - Test EC2 lifecycle actions (Stop, Start, Terminate) and observe Public IP address changes <br> - Associate an Elastic IP (static Public IP) with the instance to maintain a fixed address | 08/22/2025 | 08/22/2025 | [Elastic IP Addresses](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) |

### Week 2 Achievements:

* Mastered Amazon EC2 instance provisioning and lifecycle management.
* Learned how to establish secure SSH terminal connections to cloud Linux instances.
* Automated server software bootstrap configuration using User Data scripts.
* Understood the operational difference between dynamic Public IPs and static Elastic IPs.
