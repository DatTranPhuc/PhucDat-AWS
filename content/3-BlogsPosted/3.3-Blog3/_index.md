---
title: "Blog 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Aurora DSQL: Building a Multi-Region Active-Active REST API

![Multi-Region Active-Active Architecture: Route 53 → ALB → EC2 → Aurora DSQL](/images/3-BlogsPosted/blog3-aurora-dsql.jpg)

An AWS Database Blog article introduces how to combine **Spring Boot + Amazon Aurora DSQL** to build a **Multi-Region Active-Active** REST API, solving 3 real-world problems:

**1. No static passwords:** Uses `DSQLConnector` for IAM Role authentication, auto-refreshes tokens, encrypts with TLS — no hardcoded credentials.

**2. Conflict handling via OCC (Optimistic Concurrency Control):** DSQL uses no Locks. When 2 requests commit simultaneously — 1 succeeds, 1 receives a `40001` error. No Deadlocks.

**3. Spring Retry + HikariCP:** Configure `DsqlExceptionOverride` to retain the connection on `40001`, then use `@Retryable` + Exponential Backoff to auto-retry — client still receives **HTTP 200 OK**.

**Architecture:** Route 53 → ALB → EC2 (Spring Boot + HikariCP) → Aurora DSQL (us-east-1 ↔ us-west-2, synchronous replication). When 1 Region fails, Route 53 automatically shifts traffic — no code changes needed.

---

**Post link:** [View post on AWS Study Group Facebook](https://www.facebook.com/photo?fbid=2093845508234493&set=gm.2199940367437590&idorvanity=660548818043427)

**Reference:** [Build a Spring Boot REST API with Amazon Aurora DSQL](https://aws.amazon.com/blogs/database/build-a-spring-boot-rest-api-with-amazon-aurora-dsql/)