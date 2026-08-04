---
title: "Week 8 Worklog: Amazon CloudWatch & CloudTrail Monitoring"
date: 2024-02-19
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* Understand Amazon CloudWatch for resource performance monitoring and AWS CloudTrail for security auditing.
* Monitor EC2 system metrics such as CPU Utilization, Disk I/O, and Network In/Out.
* Configure CloudWatch Alarms to send automated email alerts during high-utilization incidents.
* Enable AWS CloudTrail to track administrative API activity across the AWS account.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Study CloudWatch fundamentals: Metrics, Dashboards, Logs, and Alarms <br> - Contrast CloudWatch (performance monitoring) with CloudTrail (activity auditing) | 09/29/2025 | 09/29/2025 | [What Is Amazon CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) |
| Tue | - Open CloudWatch Metrics console and inspect EC2 CPU Utilization graphs <br> - Create a simple custom CloudWatch Dashboard displaying key instance metrics | 09/30/2025 | 09/30/2025 | [CloudWatch Dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html) |
| Wed | - Provision an Amazon SNS Topic `my-email-alerts` and subscribe an email address <br> - Create a CloudWatch Alarm triggering when EC2 CPU > 80% for 5 minutes, publishing to SNS | 10/01/2025 | 10/01/2025 | [Create a CloudWatch Alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) |
| Thu | - Install CloudWatch Agent on EC2 to stream system log files (`/var/log/messages`) into CloudWatch Log Groups | 10/02/2025 | 10/02/2025 | [Installing the CloudWatch Agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) |
| Fri | - Create a Trail in AWS CloudTrail <br> - Practice searching Event History in CloudTrail to audit who created or deleted EC2/S3 resources | 10/03/2025 | 10/03/2025 | [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) |

### Week 8 Achievements:

* Understood cloud resource metrics and monitoring using Amazon CloudWatch.
* Automated incident notification workflows via CloudWatch Alarms and Amazon SNS.
* Streamed system and server log files to CloudWatch Log Groups.
* Audited AWS administrative action history using AWS CloudTrail.
