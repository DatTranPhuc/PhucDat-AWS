---
title: "Worklog Tuần 6: Tìm hiểu Amazon RDS (Cơ sở dữ liệu MySQL & Backup)"
date: 2024-02-05
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Tìm hiểu dịch vụ cơ sở dữ liệu quan hệ Amazon RDS (Relational Database Service).
* Khởi tạo Amazon RDS MySQL Instance trên đám mây.
* Cấu hình Security Group cho phép ứng dụng/máy chủ EC2 kết nối tới RDS (Port 3306).
* Thực hành kết nối DB client, tạo bảng dữ liệu và cấu hình sao lưu tự động (Automated Backups).

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu các khái niệm Amazon RDS: Database Engines (MySQL, PostgreSQL), DB Instance Classes & Storage Types | 15/09/2025 | 15/09/2025 | [What is Amazon RDS?](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html) |
| 3 | - Khởi tạo Amazon RDS MySQL Instance `my-sample-mysql-db` (Free Tier, Single-AZ) <br> - Đặt username/password quản trị cho cơ sở dữ liệu | 16/09/2025 | 16/09/2025 | [Creating a MySQL DB Instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.MySQL.html) |
| 4 | - Cấu hình Inbound Rule trong Security Group của RDS cho phép kết nối cổng 3306 <br> - Dùng MySQL Workbench / DBeaver kết nối thử nghiệm tới Endpoint của RDS | 17/09/2025 | 17/09/2025 | [Working with RDS Security Groups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithSecurityGroups.html) |
| 5 | - Thực hành tạo Database `test_db`, tạo bảng `users` và chạy các câu lệnh SQL CRUD (Insert, Select, Update, Delete) | 18/09/2025 | 18/09/2025 | [Connecting to a MySQL DB Instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ConnectToInstance.html) |
| 6 | - Tìm hiểu tính năng Automated Backups và DB Snapshots <br> - Thực hành tạo manual DB Snapshot và Restore thành một RDS instance mới từ Snapshot | 19/09/2025 | 19/09/2025 | [RDS Backup & Restore](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_CommonTasks.BackupRestore.html) |

### Kết quả đạt được tuần 6:

* Hiểu rõ cách khởi tạo và quản lý cơ sở dữ liệu quan hệ Amazon RDS trên Cloud.
* Kết nối thành công DB client tới Amazon RDS qua Endpoint công khai/nội bộ.
* Thực hành thuần thục các thao tác quản lý dữ liệu trên RDS.
* Nắm vững cơ chế sao lưu tự động và khôi phục cơ sở dữ liệu từ DB Snapshots.


