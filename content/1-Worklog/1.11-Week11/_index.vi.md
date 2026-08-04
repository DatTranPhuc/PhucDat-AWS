---
title: "Worklog Tuần 11: Triển khai Dự án Kitchen Hub - Phần 1: Hạ tầng 3-Tier cốt lõi"
date: 2024-03-11
weight: 1
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

* Áp dụng kiến thức 10 tuần học AWS để bắt đầu xây dựng kiến trúc 3 tầng (3-Tier) cho **Dự án Kitchen Hub**.
* Triển khai mạng ảo `KitchenHub-VPC` (Multi-AZ) với Public Subnets và Private Subnets.
* Khởi tạo máy chủ EC2 Backend (Spring Boot API) và cơ sở dữ liệu Amazon RDS MySQL trong Private Subnet.
* Đẩy trang giao diện Dashboard UI lên Amazon S3 Static Website Hosting.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tổng hợp yêu cầu dự án Kitchen Hub và vẽ sơ đồ kiến trúc hạ tầng tổng thể <br> - Khởi tạo Custom VPC `KitchenHub-VPC` (`10.0.0.0/16`) trên 2 Availability Zones | 20/10/2025 | 20/10/2025 | [3-Tier Architecture whitepaper](https://aws.amazon.com/blogs/aws/new-whitepaper-designing-3-tier-architectures-on-aws/) |
| 3 | - Thiết lập Internet Gateway, NAT Gateway và Route Tables cho Public/Private Subnets <br> - Cấu hình chuỗi Security Groups: `ALB-SG`, `EC2-App-SG`, `RDS-DB-SG` | 21/10/2025 | 21/10/2025 | [VPC Network Topology Options](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenarios.html) |
| 4 | - Khởi tạo Amazon RDS MySQL Database instance `kitchenhub-db` trong Private DB Subnets <br> - Chạy SQL script tạo các bảng dữ liệu `orders`, `menu_items`, `branches`, `users` | 22/10/2025 | 22/10/2025 | [RDS MySQL Multi-AZ Deployments](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html) |
| 5 | - Khởi tạo EC2 Instance trong Private App Subnet <br> - Deploy ứng dụng Spring Boot Backend API, kết nối với RDS MySQL thành công | 23/10/2025 | 23/10/2025 | [AWS SDK for Java Development](https://aws.amazon.com/developer/language/java/) |
| 6 | - Tạo S3 Bucket `kitchen-hub-dashboard-ui` và bật S3 Static Website Hosting <br> - Upload code Frontend Dashboard UI và kết nối API thành công tới EC2 Backend | 24/10/2025 | 24/10/2025 | [S3 Static Website Setup](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html) |

### Kết quả đạt được tuần 11:

* Dựng thành công hạ tầng mạng VPC Multi-AZ hoàn chỉnh cho dự án Kitchen Hub.
* Kết nối thành công tầng Frontend (S3), tầng Backend (EC2) và tầng Database (RDS MySQL).
* Đảm bảo an toàn hệ thống với máy chủ Backend và Database nằm hoàn toàn trong Private Subnet.
* Đạt mốc hoàn thành mô hình 3-Tier cơ bản cho Dự án Kitchen Hub.
