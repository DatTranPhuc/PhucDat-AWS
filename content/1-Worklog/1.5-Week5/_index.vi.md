---
title: "Worklog Tuần 5: Tìm hiểu Amazon VPC (Mạng ảo, Subnet & Security Group)"
date: 2024-01-29
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Tìm hiểu dịch vụ mạng ảo Amazon VPC (Virtual Private Cloud).
* Thực hành khởi tạo Custom VPC, Public Subnet, Private Subnet và Route Tables.
* Cấu hình Internet Gateway (IGW) và NAT Gateway cho phép truy cập Internet an toàn.
* Tìm hiểu và so sánh Security Groups (Stateful) với Network ACLs (Stateless).

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu các khái niệm mạng AWS: VPC, CIDR notation (`10.0.0.0/16`), Subnets, IP Routing | 08/09/2025 | 08/09/2025 | [What is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) |
| 3 | - Khởi tạo Custom VPC `My-Lab-VPC` (`10.0.0.0/16`) <br> - Chia 1 Public Subnet (`10.0.1.0/24`) và 1 Private Subnet (`10.0.2.0/24`) | 09/09/2025 | 09/09/2025 | [VPC Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html) |
| 4 | - Gắn Internet Gateway (IGW) vào Custom VPC <br> - Định tuyến Route Table cho Public Subnet ra Internet thông qua IGW | 10/09/2025 | 10/09/2025 | [Internet Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) |
| 5 | - Khởi tạo NAT Gateway trong Public Subnet <br> - Định tuyến Route Table cho Private Subnet kết nối ra Internet chiều đi qua NAT Gateway | 11/09/2025 | 11/09/2025 | [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) |
| 6 | - Khởi tạo EC2 trong Public Subnet và EC2 trong Private Subnet <br> - Thử nghiệm ping/SSH từ Public EC2 sang Private EC2 và kiểm tra an ninh với Security Groups | 12/09/2025 | 12/09/2025 | [Security Groups & NACLs](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html) |

### Kết quả đạt được tuần 5:

* Hiểu rõ kiến trúc mạng nội bộ ảo Amazon VPC trên AWS.
* Tự xây dựng mô hình mạng hoàn chỉnh có Public Subnet và Private Subnet.
* Phân biệt vai trò của Internet Gateway và NAT Gateway.
* Cấu hình bảo mật mạng đa tầng với Security Groups và Network ACLs.


