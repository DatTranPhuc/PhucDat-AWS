---
title : "Target Group"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Tổng quan

Trong phần này, một target group được tạo để xác định EC2 instance backend mà Application Load Balancer sẽ chuyển tiếp lưu lượng truy cập đến. Target group sử dụng loại target là **Instance**, giao thức **HTTP** và cổng **8080**, khớp với cổng mà container ứng dụng đang lắng nghe.

Các mục tiêu chính của phần này bao gồm:

+ Tạo một target group cho máy chủ EC2 backend.
+ Đăng ký EC2 instance vào target group.
+ Xác minh rằng target ở trạng thái khỏe mạnh (healthy).

#### Tạo target group

Một target group có tên **aws-c8n** được tạo với loại target là **Instance**. Giao thức là **HTTP**, cổng đích là **8080**, và phiên bản giao thức là **HTTP1**. Đường dẫn kiểm tra sức khỏe (health check path) là `/swagger-ui/index.html#/` và mã thành công mong đợi là **200**.

![Target group review](/images/5-Workshop/5.4-Load-Balancer/target-group-review.png)

#### Đăng ký EC2 target

Máy chủ EC2 **CenFra-MS** đang chạy được chọn làm backend target. Cổng đích được thiết lập là **8080**, khớp với cổng được container ứng dụng sử dụng.

![Register target](/images/5-Workshop/5.4-Load-Balancer/register-target.png)

#### Xác minh sức khỏe target

Sau khi target được đăng ký, target group hiển thị **1 healthy target** (1 đích khỏe mạnh) và **0 unhealthy targets** (0 đích không khỏe mạnh). Điều này xác nhận rằng bộ cân bằng tải có thể chuyển tiếp lưu lượng truy cập đến máy chủ backend thành công.

![Healthy target group](/images/5-Workshop/5.4-Load-Balancer/target-group-healthy.png)

#### Tổng kết phần Target Group

Kết thúc phần này, target group **aws-c8n** đang hoạt động với một EC2 instance khỏe mạnh được đăng ký trên cổng **8080**. Target group này sẽ được sử dụng bởi Application Load Balancer ở phần tiếp theo để định tuyến lưu lượng HTTP đến ứng dụng backend.
