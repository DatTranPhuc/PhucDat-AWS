---
title: "Worklog Tuần 10: Tìm hiểu AWS Serverless & Messaging (Lambda, SQS, SNS)"
date: 2024-03-04
weight: 1
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

* Tìm hiểu khái niệm mô hình điện toán Serverless và các dịch vụ Messaging trên AWS.
* Khởi tạo AWS Lambda function (Python/Node.js) để chạy code mà không cần quản lý máy chủ.
* Thực hành tạo Amazon SQS (Message Queue) để chứa hàng chờ thông điệp bất đồng bộ.
* Khởi tạo Amazon SNS (Pub/Sub Notification) để phát tin nhắn thông báo.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu mô hình Serverless vs Server-based <br> - Đọc tài liệu dịch vụ AWS Lambda: Triggers, Execution Role, Memory/Timeout limits | 13/10/2025 | 13/10/2025 | [Serverless on AWS](https://aws.amazon.com/serverless/) |
| 3 | - Tạo AWS Lambda function đơn giản bằng Python (xử lý dữ liệu JSON) <br> - Cấu hình Event Source Trigger từ S3 Bucket (tự động chạy Lambda khi có file mới tải lên S3) | 14/10/2025 | 14/10/2025 | [What is AWS Lambda?](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) |
| 4 | - Tìm hiểu dịch vụ Amazon SQS (Simple Queue Service): Standard Queue vs FIFO Queue <br> - Tạo SQS Queue `my-demo-queue` và thử nghiệm gửi/nhận tin nhắn từ AWS Console | 15/10/2025 | 15/10/2025 | [What is Amazon SQS?](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) |
| 5 | - Tìm hiểu dịch vụ Amazon SNS (Simple Notification Service): Topics & Subscriptions <br> - Tạo SNS Topic `my-demo-topic`, đăng ký Email Subscription và nhận email xác nhận | 16/10/2025 | 16/10/2025 | [What is Amazon SNS?](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) |
| 6 | - Thực hành kết hợp SNS + SQS: Phát thông điệp từ SNS Topic vào SQS Queue (Fanout Pattern) <br> - Tổng kết kiến thức 10 tuần học AWS căn bản để chuẩn bị triển khai Dự án thực tế | 17/10/2025 | 17/10/2025 | [SNS Common Scenarios (Fanout)](https://docs.aws.amazon.com/sns/latest/dg/sns-common-scenarios.html) |

### Kết quả đạt được tuần 10:

* Nắm rõ nguyên lý tính toán Serverless với AWS Lambda.
* Hiểu mô hình hàng chờ thông điệp bất đồng bộ với Amazon SQS.
* Thiết lập hệ thống phát thông báo tự động với Amazon SNS.
* Sẵn sàng đầy đủ hành trang kiến thức cơ bản về AWS để bắt đầu xây dựng Dự án Kitchen Hub ở 2 tuần cuối.


