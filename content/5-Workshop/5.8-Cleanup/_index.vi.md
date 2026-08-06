---
title : "Dọn Dẹp Tài Nguyên CenFra-MS"
date : 2024-01-01
weight : 8
chapter : false
pre : " <b> 5.8. </b> "
---
Chúc mừng bạn đã hoàn thành workshop! 

Trong workshop này, bạn đã học cách triển khai ứng dụng backend Spring Boot đã container hóa (**CenFra-MS**) trên kiến trúc AWS theo chuẩn sản xuất:
* Bạn đã thiết lập một Amazon EC2 instance và triển khai ứng dụng container thông qua Docker Compose.
* Bạn đã thiết lập Application Load Balancer và đăng ký EC2 instance thông qua target group.
* Bạn đã tích hợp tên miền tùy chỉnh với Route 53 và CloudFront CDN để truy cập HTTPS.
* Bạn đã cấu hình tập trung log container lên Amazon CloudWatch.

#### Dọn Dẹp Tài Nguyên

Để tránh phát sinh chi phí AWS ngoài ý muốn, hãy thực hiện theo các bước sau để dọn dẹp tất cả tài nguyên đã tạo:

1. **Xóa Bản Ghi Route 53**:
   * Truy cập **Route 53 Console** -> **Hosted Zones**.
   * Chọn hosted zone của bạn và xóa bản ghi `A Record` trỏ đến CloudFront distribution.

2. **Vô Hiệu Hóa & Xóa CloudFront Distribution**:
   * Mở **CloudFront Console**.
   * Chọn distribution của bạn, nhấn **Disable**, và chờ cho đến khi trạng thái chuyển sang disabled.
   * Khi đã disabled, chọn lại distribution đó và nhấn **Delete**.

3. **Xóa Application Load Balancer (ALB)**:
   * Mở **EC2 Console** -> **Load Balancers**.
   * Chọn `cenframs-alb` và nhấn **Actions** -> **Delete**.
   * Truy cập **Target Groups** và xóa target group `cenframs-tg`.

4. **Chấm Dứt (Terminate) EC2 Instance**:
   * Truy cập **EC2 Instances**, chọn máy chủ EC2 backend Spring Boot, và nhấn **Instance state** -> **Terminate instance**.

5. **Xóa Cơ Sở Dữ Liệu RDS PostgreSQL**:
   * Truy cập **RDS Console** -> **Databases**.
   * Chọn database instance của bạn, nhấn **Actions** -> **Delete**.
   * Chọn không tạo final snapshot (nếu chỉ làm thử nghiệm), xác nhận và nhấn **Delete**.

6. **Xóa CloudWatch Log Group**:
   * Chuyển đến **CloudWatch Logs** -> **Log groups**.
   * Chọn `/cenfra-ms/app` và nhấn **Actions** -> **Delete log group**.