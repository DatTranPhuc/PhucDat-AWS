---
title: "Worklog Tuần 7: Tìm hiểu Elastic Load Balancer (ALB & Target Groups)"
date: 2024-02-12
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Tìm hiểu giải pháp cân bằng tải Elastic Load Balancing (ELB) trên AWS.
* Phân biệt các loại Load Balancer: Application Load Balancer (ALB), Network Load Balancer (NLB).
* Khởi tạo Application Load Balancer (ALB), Target Groups và cấu hình Health Checks.
* Thực hành phân phối lưu lượng truy cập qua nhiều máy chủ EC2.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu các khái niệm ELB: Listeners, Rules, Target Groups, Health Checks & Cross-Zone Load Balancing | 22/09/2025 | 22/09/2025 | [What is Elastic Load Balancing?](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html) |
| 3 | - Khởi tạo 2 máy chủ EC2 Web Server (`EC2-Web-1` và `EC2-Web-2`) ở 2 Availability Zones khác nhau | 23/09/2025 | 23/09/2025 | [EC2 Getting Started Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html) |
| 4 | - Khởi tạo Target Group `web-servers-tg` và đăng ký 2 máy chủ EC2 vừa tạo vào Target Group <br> - Cấu hình đường dẫn Health Check `/index.html` (Interval: 30s) | 24/09/2025 | 24/09/2025 | [ELB Target Groups](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html) |
| 5 | - Khởi tạo Application Load Balancer `My-Demo-ALB` trong Public Subnets <br> - Cấu hình Listener HTTP (port 80) chuyển hướng lưu lượng vào Target Group `web-servers-tg` | 25/09/2025 | 25/09/2025 | [Getting Started with ALB](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/application-load-balancer-getting-started.html) |
| 6 | - Truy cập vào DNS Name của ALB trên trình duyệt và F5 liên tục để kiểm tra ALB cân bằng tải giữa 2 EC2 <br> - Thử nghiệm ngắt 1 EC2 và quan sát ALB tự nhận biết máy chủ lỗi qua Health Check | 26/09/2025 | 26/09/2025 | [Target group health checks](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/target-group-health-checks.html) |

### Kết quả đạt được tuần 7:

* Hiểu rõ cơ chế hoạt động cân bằng tải lớp 7 (Application Layer) với AWS ALB.
* Tạo và quản lý Target Groups cùng cơ chế kiểm tra sức khỏe Health Checks.
* Phân phối lưu lượng truy cập đồng đều tới nhiều máy chủ web.
* Đảm bảo tính khả dụng của ứng dụng khi một trong các máy chủ gặp sự cố.


