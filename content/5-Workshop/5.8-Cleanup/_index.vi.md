---
title : "Dọn dẹp tài nguyên CenFra-MS"
date : 2024-01-01
weight : 8
chapter : false
pre : " <b> 5.8. </b> "
---
Xin chúc mừng bạn đã hoàn thành xong workshop này!

Trong workshop này, bạn đã học được cách triển khai ứng dụng Java Spring Boot backend đóng gói container (**CenFra-MS**) trên một hạ tầng kiến trúc AWS chuẩn production:
* Bạn đã cài đặt máy chủ ảo Amazon EC2 và triển khai container ứng dụng bằng Docker Compose.
* Bạn đã cấu hình Application Load Balancer và đăng ký các máy chủ EC2 thông qua Target Groups.
* Bạn đã tích hợp thành công Route 53 (DNS) với CloudFront CDN để cấu hình truy cập HTTPS.
* Bạn đã gom logs container Docker tập trung về CloudWatch Logs để giám sát.

#### Dọn dẹp tài nguyên

Để tránh phát sinh các chi phí ngoài mong muốn trên tài khoản AWS, vui lòng thực hiện các bước dọn dẹp tài nguyên dưới đây:

1. **Xóa bản ghi Route 53**:
   * Truy cập **Route 53 Console** -> **Hosted Zones**.
   * Chọn hosted zone của bạn và xóa bản ghi `A Record` trỏ đến CloudFront distribution.

2. **Vô hiệu hóa & Xóa CloudFront Distribution**:
   * Truy cập **CloudFront Console**.
   * Chọn distribution của bạn, bấm **Disable** và chờ cho trạng thái chuyển sang disabled.
   * Sau đó chọn lại distribution và bấm **Delete** để xóa.

3. **Xóa Application Load Balancer (ALB)**:
   * Truy cập **EC2 Console** -> **Load Balancers**.
   * Chọn `cenframs-alb` và bấm **Actions** -> **Delete**.
   * Chọn mục **Target Groups** và tiến hành xóa target group `cenframs-tg`.

4. **Xóa EC2 Instance**:
   * Truy cập danh sách **EC2 Instances**, chọn máy chủ Spring Boot backend EC2, bấm **Instance state** -> **Terminate instance**.

5. **Xóa Amazon RDS PostgreSQL Database**:
   * Truy cập **RDS Console** -> **Databases**.
   * Chọn database instance của bạn, chọn **Actions** -> **Delete**.
   * Chọn không tạo snapshot cuối cùng (nếu chỉ dùng để thực hành), tích chọn xác nhận và chọn **Delete**.

6. **Xóa CloudWatch Log Group**:
   * Truy cập **CloudWatch Logs** -> **Log groups**.
   * Chọn `/cenfra-ms/app` và chọn **Actions** -> **Delete log group**.