---
title : "Các bước chuẩn bị"
date : 2024-01-01 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

Trước khi bắt đầu bài thực hành triển khai ứng dụng, hãy đảm bảo bạn đã hoàn thành các bước chuẩn bị sau:

---

### 1. Tài khoản AWS & Cấu hình IAM

Để triển khai hệ thống ứng dụng **CenFra-MS**, bạn cần một tài khoản IAM có quyền quản trị. Trong bài học này, chúng ta tiến hành cấu hình một IAM User Group và một IAM User được gắn kèm policy `AdministratorAccess`.

#### Bước 1: Tạo IAM User Group
1. Truy cập **IAM Console** -> **User groups** và bấm **Create group**.
2. Tạo một User Group mới có tên là `c8n`.

![IAM User Group list](/images/5-Workshop/5.2-Prerequisite/01-iam-groups.png)
*Hình 1: Tạo User Group c8n.*

#### Bước 2: Gắn Permissions Policy vào Nhóm
1. Tại giao diện nhóm `c8n`, chọn tab **Permissions** và chọn **Add permissions** -> **Attach policies**.
2. Tìm kiếm và chọn AWS-managed policy `AdministratorAccess`.

![Gắn AdministratorAccess policy](/images/5-Workshop/5.2-Prerequisite/02-group-permissions.png)
*Hình 2: Gắn policy AdministratorAccess vào User Group.*

Policy `AdministratorAccess` cho phép truy cập toàn bộ các tài nguyên AWS:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```

![Chi tiết AdministratorAccess policy](/images/5-Workshop/5.2-Prerequisite/04-policy-details.png)
*Hình 3: Chi tiết cấu hình JSON của policy AdministratorAccess.*

#### Bước 3: Tạo IAM User và thêm vào Nhóm
1. Chọn mục **Users** ở cột bên trái và bấm **Create user**.
2. Tạo người dùng mới có tên `c8n-dat`, bật các tùy chọn **Console access** và **Access keys**.
3. Thêm người dùng này vào nhóm `c8n` để tự động kế thừa quyền quản trị `AdministratorAccess`.

![Quyền của User qua nhóm](/images/5-Workshop/5.2-Prerequisite/03-user-permissions.png)
*Hình 4: Người dùng c8n-dat thừa kế quyền AdministratorAccess thông qua nhóm c8n.*

---

### 2. Hạ tầng mạng (VPC)
* Một **VPC** được thiết lập sẵn:
  * Có tối thiểu hai **Public Subnet** nằm ở các Availability Zone khác nhau để phục vụ cho Application Load Balancer.
  * Đã đính kèm **Internet Gateway** (IGW) vào VPC và cấu hình bảng định tuyến (Route Table) trỏ ra ngoài Internet.

---

### 3. Tên miền tùy chỉnh
* Một tên miền đang hoạt động và được quản lý trên **Amazon Route 53** (ví dụ: `tuandat.space`).

---

### 4. Container Image ứng dụng
* Mã nguồn ứng dụng **CenFra-MS** đã được đóng gói thành Docker image và đẩy lên Docker Hub (ví dụ: `tuandat/cenframs-backend:latest`).
* Chuẩn bị sẵn file `docker-compose.yml` để kéo (pull) image và chạy container ứng dụng.