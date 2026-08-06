---
title : "Load Balancer"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

#### Tổng quan

Trong phần này, một Application Load Balancer được cấu hình để công khai ứng dụng được lưu trữ trên EC2 thông qua một endpoint HTTP công cộng. Bộ cân bằng tải nhận các yêu cầu từ client trên cổng **80** và chuyển tiếp chúng đến target group **aws-c8n** chứa EC2 backend trên cổng **8080**.

Các mục tiêu chính của phần này bao gồm:

+ Tạo một Application Load Balancer hướng ra internet (internet-facing).
+ Cấu hình HTTP listener và các quy tắc định tuyến (routing rules).
+ Xác minh bộ cân bằng tải đang ở trạng thái hoạt động (active).

#### Chọn loại Load Balancer

AWS cung cấp nhiều loại bộ cân bằng tải. Đối với ứng dụng này, **Application Load Balancer** được chọn vì ứng dụng sử dụng lưu lượng HTTP. ALB phù hợp cho các ứng dụng web, API, microservices và các dịch vụ dựa trên container.

![Các loại load balancer](/images/5-Workshop/5.4-Load-Balancer/lb-types.png)

#### Cấu hình Application Load Balancer

Application Load Balancer được đặt tên là **c8n-aws-ALB**. Nó được cấu hình là **Internet-facing**, nghĩa là có thể nhận lưu lượng truy cập từ internet công cộng. Loại địa chỉ IP là **IPv4**.

![Cấu hình cơ bản ALB](/images/5-Workshop/5.4-Load-Balancer/alb-basic-config.png)

Bộ cân bằng tải được ánh xạ tới VPC đã chọn và 4 public subnet trên 4 Availability Zone. Điều này giúp cải thiện tính sẵn sàng vì bộ cân bằng tải có thể nhận và định tuyến lưu lượng truy cập qua nhiều zone.

![Ánh xạ mạng ALB](/images/5-Workshop/5.4-Load-Balancer/alb-network-mapping.png)

#### Cấu hình listener và định tuyến

Bộ cân bằng tải lắng nghe trên cổng **HTTP:80**. Hành động định tuyến mặc định chuyển tiếp các yêu cầu đến target group **aws-c8n** với 100% trọng số lưu lượng.

![ALB listener](/images/5-Workshop/5.4-Load-Balancer/alb-listener.png)

Trước khi tạo bộ cân bằng tải, trang xem lại xác nhận các cài đặt cuối cùng: kiểu internet-facing, loại địa chỉ IPv4, VPC đã chọn, 4 subnet, security group và HTTP listener chuyển tiếp đến một target group.

![Xem lại cài đặt ALB](/images/5-Workshop/5.4-Load-Balancer/alb-review.png)

#### Cấu hình security group cho Load Balancer

Security group của bộ cân bằng tải cho phép lưu lượng **HTTP** đi vào trên cổng **80** từ `0.0.0.0/0`. Quy tắc này cho phép người dùng trên internet truy cập ứng dụng thông qua tên DNS của ALB.

![Security group của ALB](/images/5-Workshop/5.4-Load-Balancer/alb-security-group.png)

{{% notice note %}}
Đối với môi trường sản xuất (production), các quy tắc đi vào nên được giới hạn càng nhiều càng tốt. Truy cập HTTP công khai là chấp nhận được đối với bài thực hành workshop, nhưng HTTPS và các kiểm soát truy cập nghiêm ngặt hơn được khuyến nghị cho các hệ thống thực tế.
{{% /notice %}}

#### Xác minh Application Load Balancer

Sau khi tạo, trang quản trị EC2 hiển thị **c8n-aws-ALB** ở trạng thái **Active**. Loại load balancer là **Application**, kiểu là **Internet-facing**, và AWS cấp một tên DNS để client truy cập.

![ALB đang hoạt động](/images/5-Workshop/5.4-Load-Balancer/alb-active.png)

#### Kiểm tra & Xác minh

Sau khi ALB đạt trạng thái **Active**, truy cập tên DNS của ALB trên trình duyệt (ví dụ: `http://c8n-aws-ALB-123456789.us-west-2.elb.amazonaws.com`). Trình duyệt sẽ hiển thị giao diện backend hoặc phản hồi API thành công từ EC2 target group thông qua cổng 80.

#### Tổng kết phần Load Balancer

Kết thúc phần này, Application Load Balancer đang hoạt động và sẵn sàng định tuyến lưu lượng HTTP công khai đến máy chủ EC2 backend khỏe mạnh. Kiến trúc này phân tách điểm truy cập công khai khỏi ứng dụng backend và cải thiện tính sẵn sàng bằng cách sử dụng nhiều Availability Zone.
