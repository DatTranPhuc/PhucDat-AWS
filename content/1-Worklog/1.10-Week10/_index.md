---
title: "Week 10 Worklog: AWS Serverless & Messaging Basics (Lambda, SQS, SNS)"
date: 2024-03-04
weight: 1
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

* Understand Serverless computing concepts and asynchronous messaging on AWS.
* Create an AWS Lambda function (Python/Node.js) to run event-driven code without server management.
* Practice creating an Amazon SQS (Simple Queue Service) message queue.
* Provision an Amazon SNS (Simple Notification Service) topic for pub/sub notifications.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Compare Serverless vs Server-based paradigms <br> - Study AWS Lambda architecture: Triggers, IAM Execution Roles, Memory/Timeout limits | 10/13/2025 | 10/13/2025 | [Serverless on AWS](https://aws.amazon.com/serverless/) |
| Tue | - Create a Python Lambda function processing JSON payloads <br> - Configure an S3 Event Source Trigger to execute Lambda automatically when new files land in S3 | 10/14/2025 | 10/14/2025 | [What is AWS Lambda?](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) |
| Wed | - Study Amazon SQS: Standard Queue vs FIFO Queue <br> - Create an S3 SQS Queue `my-demo-queue` and test message sending/receiving via AWS Console | 10/15/2025 | 10/15/2025 | [What is Amazon SQS?](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) |
| Thu | - Study Amazon SNS: Topics & Subscriptions <br> - Create SNS Topic `my-demo-topic`, register an Email Subscription, and verify confirmation link | 10/16/2025 | 10/16/2025 | [What is Amazon SNS?](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) |
| Fri | - Test SNS + SQS Fanout pattern (publishing messages from SNS Topic directly into SQS Queue) <br> - Consolidate Weeks 1-10 fundamental knowledge to prepare for applied Capstone project | 10/17/2025 | 10/17/2025 | [SNS Common Scenarios (Fanout)](https://docs.aws.amazon.com/sns/latest/dg/sns-common-scenarios.html) |

### Week 10 Achievements:

* Mastered Serverless event-driven computing with AWS Lambda.
* Understood asynchronous message queuing using Amazon SQS.
* Configured pub/sub alert notification workflows using Amazon SNS.
* Completed 10 weeks of AWS foundational learning, ready to implement the Kitchen Hub Capstone Project.
