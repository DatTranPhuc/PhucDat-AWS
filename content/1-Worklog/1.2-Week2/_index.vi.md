---
title: "Worklog Tuần 2: Tìm hiểu Amazon EC2 (Tạo máy chủ, Kết nối SSH & User Data)"
date: 2024-01-08
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Tìm hiểu dịch vụ máy chủ ảo Amazon EC2 (Elastic Compute Cloud).
* Khởi tạo máy chủ EC2 (Ubuntu/Amazon Linux), tạo Key Pair và cấu hình Security Groups.
* Thực hành kết nối SSH từ máy tính cá nhân vào máy chủ EC2.
* Tự động hóa cài đặt Web Server (Nginx / Apache) sử dụng EC2 User Data script.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu lý thuyết Amazon EC2: Instance Types (`t3.micro`), Elastic IP, Key Pairs & Security Groups | 18/08/2025 | 18/08/2025 | [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/) <br> [EC2 Security Groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html) |
| 3 | - Khởi tạo EC2 Instance (Amazon Linux 2023) trên Default VPC <br> - Tạo Key Pair `my-ec2-key.pem` và cấu hình Security Group mở cổng SSH (22) | 19/08/2025 | 19/08/2025 | [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) <br> [Security group rules](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/security-group-rules.html) |
| 4 | - Thực hành kết nối SSH vào máy chủ EC2 qua Terminal (`ssh -i my-ec2-key.pem ec2-user@<Public-IP>`) <br> - Thực hành lệnh Linux cơ bản: cập nhật OS, kiểm tra dung lượng ổ đĩa, CPU, RAM | 20/08/2025 | 20/08/2025 | [Connect to Linux instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) |
| 5 | - Khởi tạo EC2 mới và sử dụng User Data Script tự động cài Nginx: `#!/bin/bash sudo yum update -y && sudo yum install -y nginx && sudo systemctl start nginx` <br> - Thêm Inbound Rule HTTP (port 80) trong Security Group và kiểm tra giao diện Nginx trên trình duyệt | 21/08/2025 | 21/08/2025 | [EC2 User Data scripts](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html) |
| 6 | - Thực hành Stop, Start, Terminate EC2 instance và quan sát sự thay đổi của Public IP address <br> - Gán Elastic IP (Static Public IP) cho EC2 instance để giữ nguyên IP | 22/08/2025 | 22/08/2025 | [Elastic IP Addresses](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) |

### Kết quả đạt được tuần 2:

* Nắm vững cách khởi tạo và quản lý vòng đời của máy chủ ảo Amazon EC2.
* Làm chủ kỹ năng kết nối SSH an toàn vào máy chủ Linux trên Cloud.
* Biết cách tự động hóa cài đặt phần mềm khi khởi tạo máy chủ qua EC2 User Data script.
* Phân biệt rõ sự khác nhau giữa Public IP thông thường và Elastic IP.


