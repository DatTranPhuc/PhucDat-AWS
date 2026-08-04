---
title : "Tổng quan & Kiến trúc"
date : 2024-01-01 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

#### Tổng quan bài thực hành CenFra-MS

Trong bài lab này, bạn sẽ học cách triển khai ứng dụng backend Java đã được đóng gói container (**CenFra-MS**, hệ thống quản lý nhượng quyền và bếp trung tâm) lên một hạ tầng điện toán đám mây AWS bảo mật, có khả năng giám sát logs và có tính sẵn sàng cao.

Các bước thực hiện bao gồm thiết lập quy tắc bảo mật mạng, khởi tạo máy chủ ảo Amazon EC2, cài đặt Docker, triển khai ứng dụng backend thông qua Docker Compose, cấu hình bộ cân bằng tải Application Load Balancer (ALB), yêu cầu chứng chỉ bảo mật SSL/TLS qua AWS Certificate Manager (ACM), định tuyến tên miền bằng Route 53, phân phối nội dung qua mạng lưới CloudFront CDN và chuyển tiếp logs tập trung về CloudWatch.

#### Sơ đồ kiến trúc

Hệ thống tuân thủ theo sơ đồ kiến trúc dưới đây:

![CenFra-MS AWS Architecture](/images/2-Proposal/cenframs_architecture.png?v=2)

#### Các mục tiêu chính

* **Bước 5.3 (Triển khai EC2)**: Khởi tạo máy chủ EC2, cài đặt Docker & Docker Compose và chạy container backend CenFra-MS.
* **Bước 5.4 (Application Load Balancer)**: Thiết lập Target Group (cổng 8080) và cấu hình bộ cân bằng tải ALB để phân phối lưu lượng truy cập HTTP.
* **Bước 5.5 (Tích hợp Route 53 & CloudFront CDN)**: Định tuyến tên miền tùy chỉnh và cấu hình CDN phân phối nội dung bảo mật qua HTTPS.
* **Bước 5.6 (Giám sát Logs qua CloudWatch)**: Cấu hình đẩy logs container Docker từ EC2 về Amazon CloudWatch Logs để giám sát tập trung.
* **Bước 5.7 (Dọn dẹp tài nguyên)**: Giải phóng và xóa bỏ các tài nguyên AWS đã tạo để tránh phát sinh chi phí.
