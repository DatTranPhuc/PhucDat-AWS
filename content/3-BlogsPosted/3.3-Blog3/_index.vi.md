---
title: "Blog 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Aurora DSQL: Xây dựng REST API Multi-Region Active-Active

Bài viết trên AWS Database Blog giới thiệu cách kết hợp **Spring Boot + Amazon Aurora DSQL** để xây dựng REST API **Multi-Region Active-Active**, giải quyết 3 bài toán thực tế:

**1. Không cần password tĩnh:** Dùng `DSQLConnector` xác thực bằng IAM Role, tự refresh token, mã hóa TLS — không hardcode credential.

**2. Xử lý conflict bằng OCC (Optimistic Concurrency Control):** DSQL không dùng Lock. Khi 2 request cùng commit — 1 thành công, 1 nhận lỗi `40001`. Không có Deadlock.

**3. Spring Retry + HikariCP:** Cấu hình `DsqlExceptionOverride` giữ connection khi gặp `40001`, sau đó dùng `@Retryable` + Exponential Backoff để tự retry — client vẫn nhận **HTTP 200 OK**.

**Kiến trúc:** Route 53 → ALB → EC2 (Spring Boot + HikariCP) → Aurora DSQL (us-east-1 ↔ us-west-2, synchronous replication). Khi 1 Region gặp sự cố, Route 53 tự chuyển traffic — không cần sửa code.

---

**Link bài đăng:** [Xem bài viết trên AWS Study Group Facebook](https://www.facebook.com/photo?fbid=2093845508234493&set=gm.2199940367437590&idorvanity=660548818043427)

**Nguồn tham khảo:** [Build a Spring Boot REST API with Amazon Aurora DSQL](https://aws.amazon.com/blogs/database/build-a-spring-boot-rest-api-with-amazon-aurora-dsql/)