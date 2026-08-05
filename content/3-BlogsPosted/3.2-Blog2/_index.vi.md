---
title: "Blog 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Kiro Steering Files & AWS Security Standards

![Steering Files giúp AI tuân thủ tiêu chuẩn bảo mật AWS](/images/3-BlogsPosted/blog2-kiro-steering.jpg)

**Vấn đề:** AI không hiểu tiêu chuẩn bảo mật nội bộ của tổ chức. Khi yêu cầu tạo S3 bucket, AI có thể bỏ qua encryption, versioning, hay VPC endpoint restrictions.

**Giải pháp — Kiro Steering Files:** Định nghĩa một lần các security rules cố định, AI sẽ tự tham chiếu trong mọi tác vụ.

```markdown
# AWS Security Standards
## IAM: Least privilege, no AdministratorAccess
## S3: Encryption by default, block public access, enable versioning
## Logging: CloudTrail + AWS Config must be enabled
## Networking: No 0.0.0.0/0 unless justified
## Compliance: CIS AWS Foundations Benchmark
```

**Kết quả thực tế:** Thay vì chỉ tạo `Type: AWS::S3::Bucket` trống, AI tự động bổ sung Encryption, Versioning, Bucket Policy, Public Access Block ngay từ đầu — giảm misconfiguration, đặc biệt hữu ích trong môi trường **Multi-Account AWS**.

Steering Files là 1 trong 5 kỹ thuật dùng Kiro + Amazon Q để cải thiện Security Posture, cùng với: Security Finding Analysis, Automated Remediation, Security Review, SCP Generation.

---

**Link bài đăng:** [Xem bài viết trên AWS Study Group Facebook](https://www.facebook.com/photo/?fbid=2034832500720323&set=gm.2176154606482833&idorvanity=660548818043427)

**Nguồn tham khảo:** [Five ways to use Kiro and Amazon Q - AWS Security Blog](https://aws.amazon.com/blogs/security/five-ways-to-use-kiro-and-amazon-q-developer-to-improve-your-aws-security-posture/)