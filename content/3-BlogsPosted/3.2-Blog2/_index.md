---
title: "Blog 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Kiro Steering Files & AWS Security Standards

**Problem:** AI doesn't understand your organization's internal security standards. When asked to create an S3 bucket, it may skip encryption, versioning, or VPC endpoint restrictions.

**Solution — Kiro Steering Files:** Define security rules once as a fixed context; AI will automatically reference them in every task.

```markdown
# AWS Security Standards
## IAM: Least privilege, no AdministratorAccess
## S3: Encryption by default, block public access, enable versioning
## Logging: CloudTrail + AWS Config must be enabled
## Networking: No 0.0.0.0/0 unless justified
## Compliance: CIS AWS Foundations Benchmark
```

**Real-world result:** Instead of generating a bare `Type: AWS::S3::Bucket`, AI automatically adds Encryption, Versioning, Bucket Policy, and Public Access Block from the start — reducing misconfigurations, especially useful in **Multi-Account AWS** environments.

Steering Files is 1 of 5 techniques for using Kiro + Amazon Q to improve Security Posture, alongside: Security Finding Analysis, Automated Remediation, Security Review, and SCP Generation.

---

**Post link:** [View post on AWS Study Group Facebook](https://www.facebook.com/photo/?fbid=2034832500720323&set=gm.2176154606482833&idorvanity=660548818043427)

**Reference:** [Five ways to use Kiro and Amazon Q - AWS Security Blog](https://aws.amazon.com/blogs/security/five-ways-to-use-kiro-and-amazon-q-developer-to-improve-your-aws-security-posture/)