---
title : "Load Balancer"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

#### Tổng quan

Trong phần này, một bộ cân bằng tải Application Load Balancer (ALB) sẽ được cấu hình để công khai ứng dụng chạy trên EC2 thông qua một endpoint HTTP công cộng. Bộ cân bằng tải tiếp nhận các yêu cầu từ máy khách trên cổng **80** và chuyển tiếp chúng đến target group **aws-c8n** chứa máy chủ EC2 backend trên cổng **8080**.

Các mục tiêu chính của phần này bao gồm:

+ Khởi tạo một Application Load Balancer hướng internet (internet-facing).
+ Cấu hình listener HTTP và các quy tắc định tuyến (routing rules).
+ Xác minh bộ cân bằng tải ở trạng thái hoạt động (active).

#### Chọn loại Load Balancer

AWS cung cấp một số loại load balancer. Đối với hệ thống này, **Application Load Balancer** được chọn vì ứng dụng sử dụng giao thức HTTP. ALB phù hợp cho các ứng dụng web, API, microservices và các dịch vụ chạy trên container.

![Load balancer types](/images/5-Workshop/5.4-Load-Balancer/lb-types.png)

#### Cấu hình Application Load Balancer

Application Load Balancer được đặt tên là **c8n-aws-ALB**. Nó được cấu hình ở chế độ **Internet-facing** (hướng Internet), nghĩa là nó có thể tiếp nhận lưu lượng truy cập từ mạng internet công cộng. Loại địa chỉ IP là **IPv4**.

![ALB basic configuration](/images/5-Workshop/5.4-Load-Balancer/alb-basic-config.png)

Bộ cân bằng tải được ánh xạ tới VPC được chọn và bốn public subnet trên bốn Availability Zones khác nhau. Điều này giúp nâng cao tính sẵn sàng vì bộ cân bằng tải có thể tiếp nhận và định tuyến lưu lượng qua nhiều zone.

![ALB network mapping](/images/5-Workshop/5.4-Load-Balancer/alb-network-mapping.png)

#### Cấu hình listener và định tuyến

Bộ cân bằng tải lắng nghe trên cổng **HTTP:80**. Hành động định tuyến mặc định sẽ chuyển tiếp (forward) các yêu cầu tới target group **aws-c8n** với trọng số lưu lượng là 100%.

![ALB listener](/images/5-Workshop/5.4-Load-Balancer/alb-listener.png)

Trước khi khởi tạo bộ cân bằng tải, trang xem lại (review) xác nhận các thiết lập cuối cùng: chế độ internet-facing, loại IP IPv4, VPC đã chọn, bốn subnets, security group và listener HTTP chuyển tiếp đến một target group.

![ALB review](/images/5-Workshop/5.4-Load-Balancer/alb-review.png)

#### Cấu hình Security Group cho Load Balancer

Security Group của load balancer cho phép lưu lượng truy cập đầu vào **HTTP** trên cổng **80** từ mọi địa chỉ (`0.0.0.0/0`). Quy tắc này cho phép người dùng trên internet truy cập ứng dụng thông qua tên miền DNS của ALB.

![ALB security group](/images/5-Workshop/5.4-Load-Balancer/alb-security-group.png)

{{% notice note %}}
Đối với môi trường chạy thực tế (production), các quy tắc đầu vào nên được hạn chế tối đa. Việc cho phép truy cập HTTP công cộng là chấp nhận được đối với một bài thực hành workshop, nhưng HTTPS và các cơ chế kiểm soát truy cập chặt chẽ hơn được khuyến nghị cho các hệ thống thực tế.
{{% /notice %}}

#### Xác minh Application Load Balancer

Sau khi khởi tạo, console EC2 hiển thị trạng thái của **c8n-aws-ALB** là **Active**. Loại bộ cân bằng tải là **Application**, chế độ là **Internet-facing**, và AWS cấp một tên miền DNS (DNS name) để máy khách truy cập.

![Active ALB](/images/5-Workshop/5.4-Load-Balancer/alb-active.png)

#### Tổng kết phần Load Balancer

Kết thúc phần này, Application Load Balancer đã hoạt động và sẵn sàng định tuyến lưu lượng HTTP công cộng tới EC2 backend target khỏe mạnh. Kiến trúc này phân tách điểm truy cập công cộng khỏi máy chủ ứng dụng phía sau và cải thiện tính sẵn sàng bằng cách sử dụng nhiều Availability Zones.
