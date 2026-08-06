---
title : "Tích hợp Route 53 & CloudFront CDN"
date : 2024-01-01 
weight : 6 
5: chapter : false
pre : " <b> 5.6. </b> "
---

#### Bước 1: Cấu hình Route 53 Hosted Zone

1. Trong **Route 53 Console**, chọn **Hosted zones** và nhấn **Create hosted zone**.
2. Nhập tên tên miền tùy chỉnh của bạn (ví dụ: `tuandat.space`), đặt Type là **Public hosted zone**, và nhấn **Create**.
3. Cập nhật các bản ghi Name Server (NS) tại nhà cung cấp tên miền của bạn với các AWS Name Server được liệt kê trong Route 53.

![Route 53 Hosted Zone](/images/5-Workshop/route53/Screenshot%202026-07-28%20113639.png)
*Tạo Route 53 Hosted Zone.*

![Nhập tên Hosted Zone](/images/5-Workshop/route53/Screenshot%202026-07-28%20114628.png)
*Nhập `tuandat.space`, chọn **Public hosted zone**, và tạo Hosted Zone.*

![Cấu hình Nameservers trên Namecheap](/images/5-Workshop/route53/namecheap-route53-nameservers.png)
*Trên Namecheap, chọn **Custom DNS** và thay thế nameservers bằng các nameserver `awsdns-*` được cung cấp bởi Route 53.*

![Kiểm tra bản ghi NS và SOA](/images/5-Workshop/route53/Screenshot%202026-07-28%20114519.png)
*Sau khi tạo Hosted Zone, xác minh các bản ghi mặc định **NS** và **SOA**.*

![Trình chỉnh sửa bản ghi DNS Route 53](/images/5-Workshop/route53/Screenshot%202026-07-28%20114945.png)
*Sử dụng trình chỉnh sửa bản ghi Route 53 để chọn loại bản ghi và nhập giá trị DNS.*

---

#### Bước 2: Yêu cầu Chứng chỉ ACM cho HTTPS

1. Chuyển đến **AWS Certificate Manager (ACM)** và nhấn **Request certificate**.
2. Chọn **Request a public certificate** và nhấn **Next**.
3. Nhập tên miền của bạn (ví dụ: `cenframs.tuandat.space` và `*.tuandat.space`).
4. Chọn phương thức xác thực **DNS validation** và nhấn **Request**.
5. Sau khi yêu cầu, nhấn **Create records in Route 53** trong chi tiết tên miền để tự động xác thực quyền sở hữu.

---

#### Bước 3: Thiết lập CloudFront Distribution

1. Mở **CloudFront Console** và nhấn **Create distribution**.
2. Tại mục **Origin domain**, chọn tên DNS của Application Load Balancer.
3. Tại mục **Default cache behavior**:
   * Đặt **Viewer protocol policy** thành **Redirect HTTP to HTTPS**.
   * Đặt **Allowed HTTP methods** thành `GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE` (để hỗ trợ API).
   * Tại mục **Cache key and query requests**, chọn **Cache policy: CachingDisabled** để đảm bảo các yêu cầu API được định tuyến động đến backend thay vì bị cache.
4. Tại mục **Settings**:
   * Thêm tên miền tùy chỉnh vào **Alternate domain name (CNAME)** (ví dụ: `cenframs.tuandat.space`).
   * Trong **Custom SSL certificate**, chọn chứng chỉ ACM SSL đã yêu cầu ở bước trước.
5. Nhấn **Create distribution** và chờ trạng thái chuyển sang **Enabled**.

#### Bước 3.1: Khởi tạo distribution

Chọn **Single website or app** và đặt tên rõ ràng cho distribution như `c8n-aws-clf`.

![Tạo CloudFront distribution](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20115634.png)
*Tạo distribution và đặt tên.*

#### Bước 3.2: Chọn Origin là Application Load Balancer

Chọn **Elastic Load Balancer** và chọn tên DNS của ALB đang chuyển tiếp lưu lượng đến Target Group backend. Không chọn S3 vì CenFra-MS là một API động đằng sau ALB.

![Chọn loại origin Elastic Load Balancer](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20115958.png)
*Chọn ELB làm loại origin.*

![Nhập ALB origin](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120815.png)
*Chọn ALB và nhập tên DNS `...elb.amazonaws.com`.*

#### Bước 3.3: Cấu hình kết nối CloudFront đến ALB

Đối với ALB lắng nghe trên cổng HTTP `80`, chọn **Customize origin settings**, **HTTP only**, và cổng `80`. HTTPS của Viewer được chấm dứt tại CloudFront; kết nối từ CloudFront đến ALB tuân theo cấu hình HTTP ALB hiện tại.

![Cấu hình cài đặt origin](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120220.png)
*Sử dụng HTTP only và cổng 80 cho kết nối origin.*

![Xem lại cài đặt kết nối origin](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120820.png)
*Giữ Origin Shield tắt và xem lại các cài đặt kết nối.*

#### Bước 3.4: Cấu hình HTTPS và các phương thức API

Chọn **Customize cache settings**, sau đó thiết lập:

* **Viewer protocol policy**: `Redirect HTTP to HTTPS`.
* **Allowed HTTP methods**: `GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE`.
* **Cache policy**: `CachingDisabled` vì API sử dụng JWT, dữ liệu động và phản hồi riêng cho từng người dùng.
* **Origin request policy**: `AllViewerExceptHostHeader` để chuyển tiếp các tham số viewer cần thiết trong khi loại bỏ Host header không tương thích.

![Chính sách giao thức Viewer và phương thức HTTP](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120433.png)
*Chuyển hướng HTTP sang HTTPS và cho phép tất cả phương thức REST API.*

![Chính sách cache và origin request](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120832.png)
*Tắt cache API và chọn `AllViewerExceptHostHeader`.*

![Xác nhận hành vi cache](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120837.png)
*Xác minh cache policy, origin request policy và phương thức API.*

![Hoàn tất cài đặt cache](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120826.png)
*Cài đặt cache tùy chỉnh và chuyển hướng HTTPS đã được chọn.*

#### Bước 3.5: Xem lại và tạo

Xác nhận ALB origin, `CachingDisabled`, chuyển hướng HTTPS, và tất cả các phương thức API cần thiết, sau đó chọn **Create distribution**.

![Xem lại cấu hình distribution](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20121314.png)
*Xem lại origin, HTTPS, phương thức API và cài đặt cache trước khi tạo.*

#### Bước 3.6: Thêm domain tùy chỉnh và chứng chỉ ACM

Nếu distribution được tạo trước khi thêm domain, mở nó và chọn **Add domain**. Nhập `cenframs.tuandat.space`.

![Nhập domain tùy chỉnh](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20124937.png)
*Nhập hostname backend được phục vụ bởi CloudFront.*

![CloudFront yêu cầu chứng chỉ TLS](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20124945.png)
*CloudFront yêu cầu chứng chỉ ACM ở vùng `us-east-1` cho domain tùy chỉnh.*

![Chọn chứng chỉ ACM](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20125101.png)
*Chọn chứng chỉ bao phủ `*.tuandat.space` sau khi hoàn tất xác thực DNS.*

![Xem lại domain tùy chỉnh và chứng chỉ](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20125109.png)
*Xem lại domain và chứng chỉ trước khi chọn **Add domains**.*

![Cập nhật CloudFront thành công](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20125414.png)
*Cập nhật distribution thành công và CloudFront hiển thị các bản ghi DNS để trỏ đến.*

---

#### Bước 4: Trỏ Domain Tùy Chỉnh đến CloudFront qua Route 53

CDN được sử dụng thông qua Route 53: Namecheap vẫn là nhà đăng ký tên miền và điểm ủy quyền nameserver, Route 53 quản lý các bản ghi DNS, và CloudFront nhận lưu lượng CDN.

1. Quay lại **Route 53 Hosted Zone** của bạn.
2. Nhấn **Create record**. Đặt tên bản ghi là `cenframs`, chọn **A record**, và thêm **AAAA record** nếu cần hỗ trợ IPv6.
3. Bật công tắc **Alias**.
4. Tại mục **Route traffic to**, chọn **Alias to CloudFront distribution**, chọn CloudFront distribution của bạn, và nhấn **Create records**. Không trỏ bản ghi này trực tiếp đến ALB.
5. Bây giờ bạn có thể truy cập ứng dụng Spring Boot một cách an toàn tại `https://cenframs.tuandat.space/actuator/health`.

---

#### Bước 5: Kiểm Tra Ứng Dụng & Xác Minh S3 Media Storage

Để xác minh toàn bộ kiến trúc đang hoạt động chính xác (Route 53 -> CloudFront CDN -> ALB -> EC2 Backend -> RDS PostgreSQL & Amazon S3), truy cập ứng dụng frontend và thực hiện các thao tác thực tế.

##### 5.1. Truy cập Trang Đăng Nhập
Trúng cập `https://cenfra-ms.tuandat.space/login`. Bạn sẽ thấy màn hình đăng nhập tập trung cho **Pizza Five Guys Central Kitchen Management**.

![Trang đăng nhập tập trung](/images/5-Workshop/5.5-Route53-CloudFront/01-login-page.png)
*Hình 5: Cổng đăng nhập Pizza Five Guys Central Kitchen Management.*

##### 5.2. Dashboard Quản Lý
Nhập thông tin và đăng nhập. Ứng dụng sẽ chuyển hướng bạn đến bảng điều khiển quản lý, nơi bạn có thể thấy trạng thái sản phẩm, tóm tắt đơn hàng và số lượng tồn kho thời gian thực được lấy từ cơ sở dữ liệu RDS PostgreSQL.

![Dashboard quản lý](/images/5-Workshop/5.5-Route53-CloudFront/02-dashboard.png)
*Hình 6: Tổng quan dashboard hệ thống bếp trung tâm.*

##### 5.3. Thêm Sản Phẩm Mới và Tải Ảnh Lên S3
1. Truy cập màn hình **Quản lý sản phẩm** (`/manager/products`).
2. Nhấn **Thêm sản phẩm mới**. Đặt tên là `Sprite lon`, danh mục `Prepared Food`, đơn vị `pack`, và giá `20000`.
3. Chọn một hình ảnh để tải lên và nhấn **Thêm sản phẩm**.

![Hộp thoại thêm sản phẩm](/images/5-Workshop/5.5-Route53-CloudFront/03-add-product.png)
*Hình 7: Form tạo sản phẩm kèm tải lên hình ảnh.*

##### 5.4. Xác Minh Sản Phẩm Đã Được Tạo Thành Công
Sản phẩm được hiển thị trong dashboard và danh mục sản phẩm, xác nhận đã ghi thành công vào cơ sở dữ liệu.

![Danh sách sản phẩm](/images/5-Workshop/5.5-Route53-CloudFront/04-product-list.png)
*Hình 8: Danh mục quản lý sản phẩm hiển thị sản phẩm mới thêm.*

##### 5.5. Xác Minh File Media Được Lưu Trữ Trên S3
Nhấp chuột phải vào ảnh sản phẩm và mở trong thẻ mới. Bạn sẽ thấy ảnh sản phẩm được lưu trữ trực tiếp trên Amazon S3 bucket (`https://aws-c8n-s3.s3.us-west-2.amazonaws.com/products/...`). Điều này xác nhận ứng dụng backend tải lên và phục vụ ảnh thành công bằng AWS S3.

![URL ảnh trên S3](/images/5-Workshop/5.5-Route53-CloudFront/05-s3-image.png)
*Hình 9: Ảnh sản phẩm được tải lên và phục vụ thành công từ Amazon S3.*
