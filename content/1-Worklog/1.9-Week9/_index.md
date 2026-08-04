---
title: "Week 9 Worklog: Amazon Route 53 DNS & CloudFront CDN"
date: 2024-02-26
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

* Learn Amazon Route 53 DNS domain management and Amazon CloudFront Content Delivery Network (CDN).
* Manage DNS record types (A Record, CNAME Record, Alias Record).
* Create a CloudFront Distribution to cache S3 Bucket assets and reduce user latency.
* Configure free SSL/TLS certificates via AWS Certificate Manager (ACM) to enable HTTPS.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Study Amazon Route 53 DNS concepts: Hosted Zones, A/CNAME/Alias Records, and Routing Policies | 10/06/2025 | 10/06/2025 | [What is Amazon Route 53?](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html) |
| Tue | - Create a Route 53 Hosted Zone and point Alias A Records to EC2 and S3 Static Website Endpoints | 10/07/2025 | 10/07/2025 | [Routing traffic to AWS resources](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-routing-hub.html) |
| Wed | - Study Amazon CloudFront CDN architecture: Edge Locations, Origins, Caching & Time-To-Live (TTL) | 10/08/2025 | 10/08/2025 | [What is Amazon CloudFront?](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html) |
| Thu | - Create CloudFront Distribution pointing to an S3 Static Website Bucket <br> - Test website access via CloudFront domain (`*.cloudfront.net`) and measure response times | 10/09/2025 | 10/09/2025 | [CloudFront S3 Origins](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/DownloadDistS3AndCustomOrigins.html) |
| Fri | - Request a free SSL/TLS certificate in AWS Certificate Manager (ACM) <br> - Attach ACM certificate to CloudFront and enable automatic HTTP-to-HTTPS redirection | 10/10/2025 | 10/10/2025 | [AWS Certificate Manager Overview](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html) |

### Week 9 Achievements:

* Understood domain name resolution principles using Amazon Route 53.
* Leveraged Amazon CloudFront CDN to accelerate static asset distribution.
* Managed SSL/TLS certificates and secured web applications with HTTPS encryption.
* Reduced origin server load via Edge Location caching.
