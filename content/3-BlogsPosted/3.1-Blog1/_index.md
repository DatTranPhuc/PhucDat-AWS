---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# AWS Shield Advanced: DDoS Analysis with Attack Flow Logs

![Reference Architecture: DDoS visibility with AWS Shield Advanced flow logs](/images/3-BlogsPosted/blog1-ddos-shield.jpg)

AWS Shield Advanced introduced **attack flow logs** — capturing packet-level detail during DDoS mitigation, exported every 5 minutes, supporting JSON/Parquet formats, with a 75MB/file limit.

**3-component log delivery architecture:**

* **DeliverySource** — Shield Protection ARN
* **DeliveryDestination** — S3 (Athena), CloudWatch Logs, or Firehose (SIEM)
* **Delivery** — the logical channel linking Source ↔ Destination

**Key log fields:** `srcaddr`, `dstaddr`, `tcp_flags`, `action` (Block/Allow), `srccountry`.

**AWS CLI deployment** in 4 steps: `list-protections` → `put-delivery-source` → `put-delivery-destination` → `create-delivery`:

```bash
# 1. Get Shield Protection ARN
aws shield list-protections

# 2. Register Delivery Source
aws logs put-delivery-source --name ShieldProtectionSource --resource-arn <SHIELD_PROTECTION_ARN> --log-event-type DDoSAttackFlowLogs

# 3. Register Delivery Destination (S3)
aws logs put-delivery-destination --name ShieldLogsDestination --delivery-destination-configuration destinationResourceArn=<S3_BUCKET_ARN>

# 4. Create Delivery Channel
aws logs create-delivery --delivery-source-name ShieldProtectionSource --delivery-destination-name ShieldLogsDestination
```

Highlight: supports **cross-account & cross-region centralization** — aggregating logs from multiple AWS accounts into a central S3 bucket for analysis with Athena + QuickSight, no Agent required.

---

**Post link:** [View post on AWS Study Group Facebook](https://www.facebook.com/photo/?fbid=1787438342420914&set=gm.2211111629653797&idorvanity=660548818043427)

**Reference:** [Gain visibility into DDoS attacks with flow logs in AWS Shield Advanced](https://aws.amazon.com/vi/blogs/security/gain-visibility-into-ddos-attacks-with-flow-logs-in-aws-shield-advanced/)