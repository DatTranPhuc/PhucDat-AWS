---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# AWS Shield Advanced: DDoS Analysis with Attack Flow Logs

AWS Shield Advanced introduced **attack flow logs** — capturing packet-level detail during DDoS mitigation, exported every 5 minutes, supporting JSON/Parquet formats, with a 75MB/file limit.

**3-component log delivery architecture:**

* **DeliverySource** — Shield Protection ARN
* **DeliveryDestination** — S3 (Athena), CloudWatch Logs, or Firehose (SIEM)
* **Delivery** — the logical channel linking Source ↔ Destination

**Key log fields:** `srcaddr`, `dstaddr`, `tcp_flags`, `action` (Block/Allow), `srccountry`.

**AWS CLI deployment** in 4 steps: `list-protections` → `put-delivery-source` → `put-delivery-destination` → `create-delivery`.

Highlight: supports **cross-account & cross-region centralization** — aggregating logs from multiple AWS accounts into a central S3 bucket for analysis with Athena + QuickSight, no Agent required.

---

**Post link:** [View post on AWS Study Group Facebook](https://www.facebook.com/photo/?fbid=1787438342420914&set=gm.2211111629653797&idorvanity=660548818043427)

**Reference:** [Gain visibility into DDoS attacks with flow logs in AWS Shield Advanced](https://aws.amazon.com/vi/blogs/security/gain-visibility-into-ddos-attacks-with-flow-logs-in-aws-shield-advanced/)