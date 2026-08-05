---
title: "Các bài blogs đã đăng"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---


Tại đây sẽ là phần liệt kê, giới thiệu các blogs mà các bạn đã đăng trên [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj). Ví dụ:

###  [Blog 1 - Bóc tách kiến trúc và dữ liệu DDoS với Flow Logs trong AWS Shield Advanced](3.1-Blog1/)
Blog này phân tích chuyên sâu về tính năng attack flow logs của AWS Shield Advanced — bao gồm kiến trúc phân phối log (mô hình 3 thực thể: DeliverySource, DeliveryDestination, Delivery), các trường log schema (tcp_flags, action, srccountry), và luồng triển khai qua AWS CLI để tập trung hóa dữ liệu DDoS đa tài khoản.

###  [Blog 2 - Kiro Steering Files: Làm sao để AI luôn tuân thủ Security Standards trên AWS?](3.2-Blog2/)
Blog này khám phá cách Kiro Steering Files giải quyết vấn đề AI không hiểu tiêu chuẩn bảo mật của tổ chức — cung cấp ngữ cảnh bảo mật lâu dài để các tài nguyên AWS do AI tạo ra tự động tuân thủ mã hóa, IAM, logging, networking và compliance ngay từ đầu.

###  [Blog 3 - Xây dựng REST API Multi-Region với Aurora DSQL: Xử lý đụng độ dữ liệu và quản lý Credential](3.3-Blog3/)
Blog này trình bày cách kết hợp Spring Boot và Amazon Aurora DSQL để xây dựng REST API Multi-Region Active-Active — dùng IAM auth không cần password tĩnh, cơ chế Optimistic Concurrency Control (OCC) thay cho Lock, và @Retryable với Exponential Backoff để xử lý lỗi 40001 một cách trong suốt.