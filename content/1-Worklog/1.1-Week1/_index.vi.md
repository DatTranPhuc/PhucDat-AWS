---
title: "Worklog Tuần 1: Tổng quan AWS, Tạo tài khoản & Cấu hình IAM cơ bản"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

* Tìm hiểu tổng quan về điện toán đám mây (Cloud Computing) và hạ tầng toàn cầu của AWS.
* Khởi tạo tài khoản AWS Free Tier và thiết lập các biện pháp an toàn tài khoản.
* Tìm hiểu và cấu hình dịch vụ AWS IAM cơ bản (Users, Groups, Policies).
* Cài đặt công cụ dòng lệnh AWS CLI trên máy tính cá nhân.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu Cloud Computing: IaaS, PaaS, SaaS và ưu điểm của điện toán đám mây <br> - Đọc tài liệu AWS Global Infrastructure: Regions, Availability Zones (AZs), Edge Locations | 11/08/2025 | 11/08/2025 | [What is Cloud Computing?](https://aws.amazon.com/what-is-cloud-computing/) <br> [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/) |
| 3 | - Tạo tài khoản AWS Free Tier <br> - Kích hoạt bảo mật 2 lớp (MFA) cho Root Account <br> - Cấu hình Billing Alert & AWS Budgets nhận email cảnh báo khi chi phí vượt $5 | 12/08/2025 | 12/08/2025 | [AWS Free Tier](https://aws.amazon.com/free/) <br> [Managing Costs with AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) |
| 4 | - Tìm hiểu dịch vụ AWS IAM (Identity and Access Management) <br> - Tạo IAM User, IAM Group `Admins` và gắn IAM Policy `AdministratorAccess` | 13/08/2025 | 13/08/2025 | [What is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) |
| 5 | - Cài đặt phần mềm AWS CLI v2 trên máy tính cá nhân <br> - Tạo Access Key / Secret Access Key cho IAM User <br> - Chạy lệnh `aws configure` và kiểm tra lệnh `aws sts get-caller-identity` | 14/08/2025 | 14/08/2025 | [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) <br> [Configuration basics](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html) |
| 6 | - Tổng kết bài học Tuần 1 <br> - Lập kế hoạch thực hành dịch vụ tính toán máy chủ ảo Amazon EC2 cho tuần tiếp theo | 15/08/2025 | 15/08/2025 | [Amazon EC2 Overview](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) |

### Kết quả đạt được tuần 1:

* Hiểu rõ khái niệm căn bản về điện toán đám mây và hạ tầng hạ tầng AWS.
* Tạo và bảo vệ tài khoản AWS thành công với MFA và cảnh báo ngân sách AWS Budgets.
* Tạo IAM User riêng làm việc thay cho Root Account theo đúng chuẩn an ninh.
* Cài đặt thành công AWS CLI và kết nối tới tài khoản AWS thành công.


