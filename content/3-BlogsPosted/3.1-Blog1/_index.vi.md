---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# AWS Shield Advanced: Phân tích DDoS với Attack Flow Logs

![Kiến trúc tham khảo: Phân tích DDoS với flow logs trong AWS Shield Advanced](/images/3-BlogsPosted/blog1-ddos-shield.jpg)

AWS Shield Advanced vừa bổ sung tính năng **attack flow logs** — cho phép ghi lại chi tiết từng gói tin trong quá trình DDoS mitigation, xuất log mỗi 5 phút, hỗ trợ định dạng JSON/Parquet, giới hạn 75MB/file.

**3 thành phần kiến trúc phân phối log:**

* **DeliverySource** — ARN của Shield Protection
* **DeliveryDestination** — S3 (Athena), CloudWatch Logs, hoặc Firehose (SIEM)
* **Delivery** — kênh kết nối Source ↔ Destination

**Các trường log quan trọng:** `srcaddr`, `dstaddr`, `tcp_flags`, `action` (Block/Allow), `srccountry`.

**Triển khai qua AWS CLI** với 4 bước: `list-protections` → `put-delivery-source` → `put-delivery-destination` → `create-delivery`.

Điểm nổi bật: hỗ trợ **cross-account & cross-region centralization** — gom log từ nhiều AWS account về một S3 bucket trung tâm để phân tích bằng Athena + QuickSight, không cần cài Agent.

---

**Link bài đăng:** [Xem bài viết trên AWS Study Group Facebook](https://www.facebook.com/photo/?fbid=1787438342420914&set=gm.2211111629653797&idorvanity=660548818043427)

**Nguồn tham khảo:** [Gain visibility into DDoS attacks with flow logs in AWS Shield Advanced](https://aws.amazon.com/vi/blogs/security/gain-visibility-into-ddos-attacks-with-flow-logs-in-aws-shield-advanced/)