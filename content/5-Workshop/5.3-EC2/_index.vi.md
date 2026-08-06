---
title : "EC2"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Tổng quan

Trong phần này, một máy chủ ảo Amazon EC2 sẽ được chuẩn bị để làm backend server cho ứng dụng. Máy chủ này lưu trữ dịch vụ **CenFra-MS** đã được đóng gói container và sau này sẽ trở thành đích định tuyến (backend target) cho Application Load Balancer.

Các mục tiêu chính của phần này bao gồm:

+ Xem xét Security Group của EC2.
+ Kiểm tra cấu hình EC2 instance.
+ Khởi động EC2 instance.
+ Kết nối tới EC2 instance.
+ Cài đặt Docker và chuẩn bị môi trường chạy ứng dụng.

#### Security Group của EC2

Security Group của EC2 kiểm soát lưu lượng truy cập được phép đi tới instance. Trong cấu hình này, các rule inbound bao gồm **HTTP**, **SSH**, **RDP**, và **Custom TCP 8080**. Cổng 8080 được sử dụng bởi container ứng dụng.

![EC2 security group](/images/5-Workshop/5.3-EC2/ec2-security-group.png)

#### Thông tin tổng quan về EC2 instance

Instance backend được đặt tên là **CenFra-MS**. Máy chủ sử dụng loại instance **t3.micro** và được triển khai trong VPC và subnet đã chọn. Bảng tóm tắt thông tin cũng hiển thị địa chỉ IPv4 public, IPv4 private, tên DNS public và tên DNS private.

![EC2 instance summary](/images/5-Workshop/5.3-EC2/ec2-summary.png)

#### Khởi động EC2 instance

Sau khi khởi động, console EC2 hiển thị trạng thái của máy chủ **CenFra-MS** là **Running** (Đang chạy). Điều này xác nhận rằng server đã sẵn sàng để triển khai ứng dụng.

![EC2 running state](/images/5-Workshop/5.3-EC2/ec2-running.png)

#### Kết nối và cài đặt Docker

Instance được truy cập thông qua SSH. Giao diện dòng lệnh (terminal) hiển thị hệ điều hành đang chạy là **Amazon Linux 2023**. Docker được cài đặt để chạy ứng dụng trong môi trường container hóa.

![Install Docker](/images/5-Workshop/5.3-EC2/docker-install.png)

#### Chuẩn bị tệp Docker Compose

Một thư mục làm việc được tạo cho ứng dụng và tệp `docker-compose.yml` được chuẩn bị. Docker Compose định nghĩa dịch vụ ứng dụng, image sử dụng, tệp môi trường, cơ chế tự khởi động lại và ánh xạ cổng cho ứng dụng.

![Prepare Docker Compose](/images/5-Workshop/5.3-EC2/docker-compose-file.png)


#### Hỗ trợ IAM role

Một IAM role có thể được tạo cho EC2 bằng cách chọn **AWS service** làm loại thực thể tin cậy (trusted entity type) và **EC2** làm trường hợp sử dụng dịch vụ. Điều này cho phép các EC2 instance gọi các dịch vụ AWS khác một cách an toàn mà không cần lưu trữ Access Key dài hạn trên máy chủ.

![IAM EC2 role](/images/5-Workshop/5.3-EC2/iam-ec2-role.png)

#### Kiểm tra & Xác minh

Sau khi khởi chạy container bằng Docker Compose, xác minh ứng dụng hoạt động bình thường bằng cách chạy lệnh curl hoặc mở trình duyệt:

```bash
curl -i http://localhost:8080/actuator/health
```

Kết quả phản hồi mong đợi (`HTTP/1.1 200 OK`):
```json
{
  "status": "UP"
}
```

#### Tóm tắt phần EC2

Kết thúc phần này, EC2 instance đã hoạt động và sẵn sàng cho ứng dụng. Ứng dụng chạy trên cổng **8080**, là cổng sẽ được sử dụng sau này khi đăng ký máy chủ vào target group của bộ cân bằng tải.
