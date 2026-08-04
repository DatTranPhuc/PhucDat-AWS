---
title : "Tích hợp Route 53 & CloudFront CDN"
date : 2024-01-01 
weight : 5 
chapter : false
pre : " <b> 5.5. </b> "
---

#### Bước 1: Cấu hình Route 53 Hosted Zone

1. Tại **Route 53 Console**, chọn mục **Hosted zones** và chọn **Create hosted zone**.
2. Nhập tên miền tùy chỉnh của bạn (ví dụ: `tuandat.space`), đặt loại là **Public hosted zone** và chọn **Create**.
3. Cập nhật các bản ghi Name Servers (NS) do Route 53 cung cấp lên trình quản lý tên miền của nhà đăng ký của bạn.

![Route 53 Hosted Zone](/images/5-Workshop/route53/Screenshot%202026-07-28%20113639.png)
*Tạo Route 53 Hosted Zone.*

![Nhập tên Hosted Zone](/images/5-Workshop/route53/Screenshot%202026-07-28%20114628.png)
*Nhập tên miền `tuandat.space` và chọn **Public hosted zone** trước khi tạo Hosted Zone.*

![Cấu hình Nameserver trên Namecheap](/images/5-Workshop/route53/namecheap-route53-nameservers.png)
*Tại Namecheap, chọn **Custom DNS** và thay nameserver bằng các nameserver `awsdns-*` do Route 53 cung cấp. Đây là bước ủy quyền DNS cho Route 53; không phải bản ghi CNAME của CloudFront.*

![Kiểm tra NS và SOA records](/images/5-Workshop/route53/Screenshot%202026-07-28%20114519.png)
*Sau khi tạo Hosted Zone, kiểm tra hai record mặc định **NS** và **SOA**. Các nameserver trong record NS là nameserver cần dùng cho domain.*

![Giao diện thêm DNS record](/images/5-Workshop/route53/Screenshot%202026-07-28%20114945.png)
*Giao diện Route 53 để chọn loại record và nhập thông tin DNS khi cần thêm record mới.*

---

#### Bước 2: Yêu cầu chứng chỉ SSL ACM cấp phát HTTPS

1. Truy cập **AWS Certificate Manager (ACM)** và bấm **Request certificate**.
2. Chọn **Request a public certificate** và chọn **Next**.
3. Nhập tên miền (ví dụ: `cenframs.tuandat.space` và tên miền phụ dạng wildcard `*.tuandat.space`).
4. Chọn phương thức xác thực **DNS validation** và bấm **Request**.
5. Sau khi yêu cầu được tạo, bấm vào chi tiết chứng chỉ và chọn **Create records in Route 53** để tự động cấu hình bản ghi xác thực tên miền.

---

#### Bước 3: Cấu hình CloudFront Distribution

1. Mở **CloudFront Console** và bấm **Create distribution**.
2. Tại phần **Origin domain**, chọn DNS của Application Load Balancer (ALB) đã tạo trước đó.
3. Cấu hình hành vi cache (**Default cache behavior**):
   * **Viewer protocol policy**: Chọn **Redirect HTTP to HTTPS** (để tự chuyển hướng bảo mật).
   * **Allowed HTTP methods**: Chọn `GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE` để hỗ trợ toàn bộ các phương thức gọi API REST.
   * **Cache key and query requests**: Chọn **Cache policy: CachingDisabled** để đảm bảo CloudFront không cache dữ liệu API mà luôn chuyển tiếp request động về backend EC2.
4. Cấu hình cài đặt nâng cao (**Settings**):
   * Nhập tên miền phụ của bạn vào mục **Alternate domain name (CNAME)** (ví dụ: `cenframs.tuandat.space`).
   * Chọn chứng chỉ bảo mật tương ứng trong mục **Custom SSL certificate** đã được ACM cấp phát.
5. Bấm **Create distribution** và chờ cho trạng thái chuyển sang hoạt động (**Enabled**).

#### Bước 3.1: Bắt đầu tạo distribution

Chọn **Single website or app**. Đặt tên distribution để dễ quản lý, ví dụ `c8n-aws-clf`.

![Tạo CloudFront distribution](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20115634.png)
*Màn hình khởi tạo distribution và đặt tên cho CloudFront.*

#### Bước 3.2: Chọn Application Load Balancer làm origin

Chọn **Elastic Load Balancer**, sau đó chọn đúng DNS của ALB đang forward tới Target Group backend. Không chọn S3 vì backend CenFra-MS là API chạy sau ALB.

![Chọn loại origin là Elastic Load Balancer](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20115958.png)
*CloudFront hỗ trợ nhiều loại origin; với backend này, origin là ALB.*

![Nhập ALB origin](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120815.png)
*Chọn Elastic Load Balancer và nhập DNS ALB, ví dụ `...elb.amazonaws.com`.*

#### Bước 3.3: Cấu hình kết nối từ CloudFront đến ALB

Với ALB đang lắng nghe HTTP port `80`, chọn **Customize origin settings**, **HTTP only**, port `80`. HTTPS phía người dùng vẫn được xử lý ở CloudFront; kết nối nội bộ CloudFront → ALB dùng HTTP theo cấu hình thực tế của workshop.

![Cấu hình origin settings](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120220.png)
*Đặt protocol kết nối đến origin là HTTP only và port là 80.*

![Kiểm tra origin connection settings](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120820.png)
*Giữ Origin Shield tắt và kiểm tra timeout/connection settings trước khi chuyển sang cache behavior.*

#### Bước 3.4: Cấu hình HTTPS và phương thức API

Chọn **Customize cache settings**, sau đó:

* **Viewer protocol policy**: `Redirect HTTP to HTTPS`.
* **Allowed HTTP methods**: `GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE`.
* **Cache policy**: `CachingDisabled` vì API có JWT, dữ liệu động và dữ liệu theo user.
* **Origin request policy**: `AllViewerExceptHostHeader` để chuyển tiếp query string, cookie và headers cần thiết nhưng loại Host header không phù hợp với ALB origin.

![Viewer protocol và HTTP methods](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120433.png)
*Bật chuyển hướng HTTP → HTTPS và cho phép toàn bộ method REST API.*

![Cache policy và origin request policy](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120832.png)
*Tắt cache API bằng `CachingDisabled` và chọn `AllViewerExceptHostHeader`.*

![Xác nhận cache behavior](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120837.png)
*Kiểm tra lần cuối cache policy, origin request policy và các method trước khi tiếp tục.*

![Thiết lập cache settings hoàn chỉnh](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20120826.png)
*Màn hình tổng hợp cho thấy đã chọn custom cache settings và redirect HTTPS.*

#### Bước 3.5: Review và tạo distribution

Kiểm tra origin là ALB, cache policy là `CachingDisabled`, viewer protocol là redirect HTTPS và các method API đã đủ. Sau đó chọn **Create distribution**.

![Review cấu hình distribution](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20121314.png)
*Review cấu hình trước khi tạo distribution: ALB origin, HTTPS redirect, API methods và cache disabled.*

#### Bước 3.6: Gắn custom domain và ACM certificate

Nếu tạo distribution trước rồi mới gắn domain, mở distribution → **Add domain** và nhập `cenframs.tuandat.space`.

![Nhập custom domain](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20124937.png)
*Nhập domain backend sẽ phục vụ qua CloudFront.*

![CloudFront yêu cầu TLS certificate](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20124945.png)
*CloudFront yêu cầu chứng chỉ ACM ở region `us-east-1`; đây là yêu cầu bắt buộc cho custom domain.*

![Chọn ACM certificate](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20125101.png)
*Chọn certificate bao phủ `*.tuandat.space` sau khi DNS validation hoàn tất.*

![Review custom domain và certificate](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20125109.png)
*Kiểm tra domain và certificate trước khi chọn **Add domains**.*

![CloudFront cập nhật thành công](/images/5-Workshop/cloudfront/Screenshot%202026-07-28%20125414.png)
*CloudFront cập nhật distribution thành công và hiển thị bản ghi DNS cần trỏ về distribution.*

---

#### Bước 4: Trỏ tên miền phụ về CloudFront qua Route 53

CDN được sử dụng thông qua Route 53: Namecheap chỉ giữ vai trò đăng ký domain và ủy quyền nameserver, Route 53 quản lý DNS record, còn CloudFront nhận traffic CDN.

1. Quay trở lại **Route 53 Hosted Zone** của bạn.
2. Bấm **Create record**. Nhập tên bản ghi con là `cenframs`, chọn **A record** và tạo thêm **AAAA record** nếu muốn hỗ trợ IPv6.
3. Bật công tắc **Alias** (Bản ghi bí danh).
4. Tại mục **Route traffic to**, chọn **Alias to CloudFront distribution**, chọn distribution CloudFront của bạn và chọn **Create records**. Không trỏ record này trực tiếp về ALB.
5. Bây giờ bạn đã có thể truy cập backend Spring Boot một cách an toàn qua đường dẫn HTTPS: `https://cenframs.tuandat.space/actuator/health`.

---

#### Bước 5: Kiểm thử ứng dụng & Xác minh Lưu trữ tệp tin trên S3

Để xác minh toàn bộ kiến trúc đang hoạt động chính xác (Route 53 -> CloudFront CDN -> ALB -> EC2 Backend -> RDS PostgreSQL & Amazon S3), hãy truy cập ứng dụng giao diện và thực hiện các thao tác kiểm thử.

##### 5.1. Truy cập Trang Đăng nhập của ứng dụng
Truy cập đường dẫn `https://cenfra-ms.tuandat.space/login`. Bạn sẽ thấy màn hình đăng nhập tập trung của hệ thống **Pizza Five Guys - Central kitchen management** hiển thị thành công.

![Trang đăng nhập ứng dụng](/images/5-Workshop/5.5-Route53-CloudFront/01-login-page.png)
*Hình 5: Giao diện đăng nhập hệ thống Pizza Five Guys.*

##### 5.2. Trang Tổng quan quản lý (Dashboard)
Nhập thông tin tài khoản và đăng nhập. Hệ thống sẽ chuyển hướng bạn đến trang Dashboard tổng quan hiển thị các thông tin về tồn kho, đơn hàng hôm nay và danh sách sản phẩm lấy từ cơ sở dữ liệu RDS PostgreSQL.

![Trang tổng quan Dashboard](/images/5-Workshop/5.5-Route53-CloudFront/02-dashboard.png)
*Hình 6: Giao diện tổng quan hệ thống Bếp trung tâm.*

##### 5.3. Thêm sản phẩm mới và tải ảnh lên S3
1. Truy cập mục **Sản phẩm** (`/manager/products`).
2. Chọn **Thêm sản phẩm**. Nhập tên sản phẩm `Sprite lon`, danh mục `Prepared Food`, đơn vị tính `pack`, đơn giá `20000`.
3. Chọn một ảnh tải lên và chọn **Thêm sản phẩm**.

![Hộp thoại thêm sản phẩm](/images/5-Workshop/5.5-Route53-CloudFront/03-add-product.png)
*Hình 7: Form nhập thông tin sản phẩm và tải ảnh minh họa.*

##### 5.4. Xác minh sản phẩm tạo thành công
Sản phẩm vừa tạo sẽ xuất hiện ngay trong danh sách quản lý sản phẩm của hệ thống, xác nhận dữ liệu đã được ghi thành công xuống database PostgreSQL.

![Danh sách sản phẩm mới thêm](/images/5-Workshop/5.5-Route53-CloudFront/04-product-list.png)
*Hình 8: Danh sách sản phẩm cập nhật món Sprite lon thành công.*

##### 5.5. Xác minh tệp tin ảnh lưu trữ trên Amazon S3
Nhấp chuột phải vào ảnh sản phẩm vừa thêm và mở trong tab mới. Bạn sẽ thấy địa chỉ URL của ảnh chỉ trực tiếp về S3 bucket của bạn (`https://aws-c8n-s3.s3.us-west-2.amazonaws.com/products/...`). Điều này chứng minh ứng dụng backend đã kết nối và lưu trữ file tĩnh thành công lên Amazon S3.

![URL ảnh trên S3](/images/5-Workshop/5.5-Route53-CloudFront/05-s3-image.png)
*Hình 9: Ảnh sản phẩm được lưu trữ và tải trực tiếp từ Amazon S3 bucket.*
