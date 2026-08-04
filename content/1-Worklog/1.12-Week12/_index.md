---
title: "Week 12 Worklog: Kitchen Hub Project - Part 2: High Availability, Testing & Final Report"
date: 2024-03-18
weight: 1
chapter: false
pre: " <b> 1.12. </b> "
---

### Week 12 Objectives:

* Finalize Advanced Infrastructure for Kitchen Hub: Integrate Application Load Balancer (ALB), Auto Scaling Group (ASG), and CloudFront CDN.
* Execute comprehensive End-to-End integration testing and fault tolerance verification.
* Optimize operational costs using AWS Cost Explorer and clean up temporary lab resources.
* Summarize achievements, update all sections of the Hugo internship report, and publish the final report.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Deploy Application Load Balancer (`KitchenHub-ALB`) for public traffic entry <br> - Configure Auto Scaling Group (`KitchenHub-ASG`) to auto-scale EC2 Backend instances across 2 Private Subnets | 10/27/2025 | 10/27/2025 | [Attach Load Balancer to ASG](https://docs.aws.amazon.com/autoscaling/ec2/userguide/attach-load-balancer-asg.html) |
| Tue | - Create CloudFront Distribution to accelerate delivery for Dashboard UI (S3) and Backend API (ALB) <br> - Configure SSL/TLS HTTPS encryption | 10/28/2025 | 10/28/2025 | [Using ELB as CloudFront Origin](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-elb-as-origin.html) |
| Wed | - Execute complete End-to-End testing: Browser -> CloudFront CDN -> ALB -> EC2 Spring Boot -> RDS MySQL & S3 Media Storage <br> - Simulate EC2 instance outage to verify ALB and ASG auto-healing | 10/29/2025 | 10/29/2025 | [Fault Tolerance in AWS](https://aws.amazon.com/blogs/architecture/fault-tolerance-in-aws/) |
| Thu | - Evaluate costs on AWS Cost Explorer, optimize resource utilization, and clean up unneeded lab resources | 10/30/2025 | 10/30/2025 | [AWS Cost Explorer Guide](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html) |
| Fri | - Verify Hugo report site layout, build production bundle with `hugo --minify` <br> - Publish the final 12-week Kitchen Hub on AWS Internship Report | 10/31/2025 | 10/31/2025 | [Hugo Documentation](https://gohugo.io/documentation/) |

### Week 12 Achievements:

* Completed 100% of the **Kitchen Hub Architecture on AWS** 3-Tier Enterprise Web Application implementation.
* Delivered a Fault Tolerant, Auto Scaling, and HTTPS-secured cloud infrastructure.
* Optimized operational spending and cloud resource usage efficiently.
* Successfully completed and published the comprehensive 12-week Internship Report website on Hugo.
