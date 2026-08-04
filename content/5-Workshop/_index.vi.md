---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Triển khai ứng dụng Spring Boot chuẩn Production trên AWS

#### Tổng quan

Trong bài thực hành này, bạn sẽ học cách triển khai một ứng dụng backend Spring Boot đã đóng gói container (**CenFra-MS**) trên một hạ tầng đám mây AWS bảo mật, có khả năng giám sát tập trung và có tính sẵn sàng cao.

Kiến trúc bao gồm chạy ứng dụng trên máy ảo EC2, sử dụng target group để định tuyến lưu lượng qua bộ cân bằng tải ALB, tích hợp tên miền tùy chỉnh trong Route 53 với chứng chỉ SSL được cấp phát bởi AWS Certificate Manager (ACM), phân phối nội dung qua CloudFront CDN và gom logs tập trung về Amazon CloudWatch.

#### Nội dung chính

1. [Tổng quan & Sơ đồ kiến trúc](5.1-workshop-overview/)
2. [Các bước chuẩn bị](5.2-prerequiste/)
3. [Triển khai EC2](5.3-ec2/)
4. [Application Load Balancer](5.4-load-balancer/)
5. [Tích hợp Route 53 & CloudFront CDN](5.5-route53-cloudfront/)
6. [Amazon CloudWatch Logs & Giám sát](5.6-cloudwatch/)
7. [Dọn dẹp tài nguyên CenFra-MS](5.7-cleanup/)
