---
title: "Worklog Tuần 8: Tìm hiểu Amazon CloudWatch & CloudTrail (Giám sát & Ghi log)"
date: 2024-02-19
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Tìm hiểu dịch vụ theo dõi hệ thống Amazon CloudWatch và nhật ký kiểm vết an ninh AWS CloudTrail.
* Đọc và theo dõi các chỉ số (Metrics) hệ thống như CPU Utilization, Network In/Out của EC2.
* Cấu hình CloudWatch Alarms tự động gửi Email cảnh báo khi tài nguyên bị quá tải.
* Tìm hiểu và kích hoạt AWS CloudTrail ghi nhận lịch sử các thao tác trên tài khoản AWS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu các khái niệm CloudWatch: Metrics, Dashboards, Logs & Alarms <br> - Phân biệt giữa CloudWatch (Giám sát hiệu năng) và CloudTrail (Audit hành động) | 29/09/2025 | 29/09/2025 | [What Is Amazon CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) |
| 3 | - Mở giao diện CloudWatch Metrics, quan sát biểu đồ CPU Utilization của máy chủ EC2 <br> - Tạo CloudWatch Dashboard đơn giản tập trung các biểu đồ theo dõi | 30/09/2025 | 30/09/2025 | [CloudWatch Dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html) |
| 4 | - Khởi tạo Amazon SNS Topic `my-email-alerts` và subscribe địa chỉ Email cá nhân <br> - Tạo CloudWatch Alarm cảnh báo khi EC2 CPU Utilization > 80% trong 5 phút và gửi tin nhắn về SNS | 01/10/2025 | 01/10/2025 | [Create a CloudWatch Alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) |
| 5 | - Cài đặt CloudWatch Agent trên máy chủ EC2 để đẩý file log hệ thống (`/var/log/messages`) về CloudWatch Log Groups | 02/10/2025 | 02/10/2025 | [Installing the CloudWatch Agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) |
| 6 | - Khởi tạo Trail trên AWS CloudTrail <br> - Thực hành xem Event History trên CloudTrail để tìm lại ai đã tạo/xóa tài nguyên EC2/S3 | 03/10/2025 | 03/10/2025 | [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) |

### Kết quả đạt được tuần 8:

* Nắm vững cách theo dõi và đo đạc chỉ số sức khỏe tài nguyên trên AWS với CloudWatch.
* Tự động hóa cảnh báo sự cố hạ tầng qua Email bằng CloudWatch Alarms & Amazon SNS.
* Biết cách tập trung log ứng dụng/máy chủ về CloudWatch Log Groups.
* Hiểu cách kiểm vết lịch sử thao tác quản trị tài khoản qua AWS CloudTrail.


