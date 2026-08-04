---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Triển khai Ứng dụng Spring Boot chuẩn Production trên AWS
## Xây dựng Hạ tầng Đám mây Bảo mật, Giám sát và Tự động hóa cho hệ thống CenFra-MS

### 1. Tóm tắt dự án
Workshop **Triển khai Ứng dụng Spring Boot chuẩn Production trên AWS** hướng dẫn cách triển khai ứng dụng Java backend đã đóng gói container bằng cách sử dụng kiến trúc đám mây thực tế, thay vì phụ thuộc vào các dịch vụ hosting đơn giản hóa.

Ứng dụng được sử dụng trong workshop là **CenFra-MS** (Hệ thống quản lý chuỗi cửa hàng nhượng quyền và bếp trung tâm). Phần backend được phát triển bằng Spring Boot và PostgreSQL, đóng gói container bằng Docker và triển khai lên Amazon EC2. 

Các yêu cầu truy cập từ người dùng sẽ được phân giải qua Amazon Route 53, bảo mật và định tuyến thông qua mạng phân phối nội dung Amazon CloudFront CDN, phân phối bởi Application Load Balancer (ALB), và chuyển tiếp đến container Spring Boot chạy trên EC2. Amazon RDS cho PostgreSQL cung cấp cơ sở dữ liệu được quản lý hoàn toàn trong mạng riêng, trong khi Amazon S3 lưu trữ hình ảnh sản phẩm và tệp tin phương tiện. Hệ thống cũng sử dụng Amazon CloudWatch để ghi nhật ký container tập trung và GitHub Actions để tự động hóa quy trình CI/CD. Giao diện frontend viết bằng React được host trên Vercel và giao tiếp với backend qua tên miền HTTPS riêng.

---

### 2. Tuyên bố vấn đề
#### Vấn đề hiện tại là gì?
Nhiều dự án của sinh viên hoặc dự án cá nhân chỉ chạy trên localhost hoặc được triển khai bằng các nền tảng hosting đơn giản. Dù các nền tảng này tiện lợi, chúng lại ẩn đi nhiều khái niệm hạ tầng đám mây quan trọng như:
* Phân giải DNS tên miền
* CDN và Edge Caching
* Mã hóa SSL/TLS (HTTPS)
* Cân bằng tải (Load balancing)
* Cơ sở dữ liệu nằm trong subnet private bảo mật
* Lưu trữ đối tượng S3 kết hợp phân quyền IAM
* Nhật ký hệ thống tập trung (Centralized logging)
* Đường ống tự động hóa CI/CD pipeline

Kết quả là, lập trình viên có thể biết cách viết ứng dụng Spring Boot nhưng lại thiếu kinh nghiệm thực tế về cách triển khai và vận hành nó trong một môi trường đám mây thực tế. Ngoài ra, việc triển khai thủ công truyền thống cũng gây ra nhiều vấn đề:
* Các bước triển khai lặp đi lặp lại, thủ công và dễ xảy ra sai sót.
* Cơ sở dữ liệu và ứng dụng chạy chung một máy chủ, gây rủi ro lớn về bảo mật.
* Ảnh tải lên được lưu cục bộ trên máy chủ EC2 và sẽ bị mất hoàn toàn khi container được rebuild.
* Nhật ký lỗi (Logs) rất khó tiếp cận và phân tích nếu không có quyền SSH vào máy chủ.

#### Giải pháp đề xuất
Workshop đề xuất xây dựng một kiến trúc triển khai hoàn chỉnh trên AWS cho ứng dụng Spring Boot đã container hóa.

Mô hình giải pháp sử dụng:
* **Trình duyệt người dùng** → **Amazon Route 53** → **Amazon CloudFront** → **Application Load Balancer** → **Amazon EC2 (Docker)** → **Amazon RDS PostgreSQL**.
* **Amazon S3** làm kho lưu trữ hình ảnh và tệp tin bền vững.
* **Amazon CloudWatch** thu thập nhật ký log từ container.
* **AWS IAM Role** cấp quyền an toàn cho EC2 mà không cần lưu key truy cập.
* **GitHub Actions** tự động hóa quy trình build và deploy CI/CD.

Kiến trúc này phân tách rõ ràng trách nhiệm giữa ứng dụng web, máy chủ backend, cơ sở dữ liệu, kho lưu trữ phương tiện và hệ thống giám sát.

#### Lợi ích và Hiệu quả đầu tư (ROI)
* **Kinh nghiệm thực hành**: Cung cấp cho người tham gia trải nghiệm thực tế trong việc thiết kế hạ tầng đám mây chuẩn doanh nghiệp.
* **Bảo mật tối đa**: RDS đặt trong private subnet, phân quyền qua IAM Instance Profile và mã hóa HTTPS bảo mật qua ACM.
* **Tự động hóa hoàn toàn**: Chỉ cần push code lên GitHub, hệ thống tự động build Docker image và cập nhật container mới trên EC2.
* **Giám sát trực quan**: Lập trình viên có thể xem trực tiếp log ứng dụng trên CloudWatch mà không cần truy cập SSH vào EC2.
* **Tiết kiệm chi phí**: Hệ thống được thiết kế tối ưu, có thể chạy trong Free Tier hoặc với ngân sách thấp khoảng **$42–$67/tháng** cho bản thử nghiệm.

---

### 3. Kiến trúc giải pháp
Ứng dụng tuân theo mô hình kiến trúc dưới đây:

![CenFra-MS AWS Architecture](/images/2-Proposal/cenframs_architecture.png?v=2)

#### Các dịch vụ AWS sử dụng
* **Amazon Route 53**: Quản lý các bản ghi DNS công khai cho tên miền tùy chỉnh (ví dụ: `cenframs.tuandat.space`).
* **Amazon CloudFront**: Đóng vai trò CDN, thực thi kết nối HTTPS bảo mật và cache các tệp tin tĩnh.
* **Application Load Balancer (ALB)**: Tiếp nhận traffic từ CloudFront và định tuyến các API request tới nhóm máy chủ target group.
* **Amazon EC2**: Chạy ứng dụng Spring Boot backend đã đóng gói container thông qua Docker Compose.
* **Amazon RDS cho PostgreSQL**: Cơ sở dữ liệu được quản trị hoàn toàn nằm trong subnet private giúp bảo mật thông tin.
* **Amazon S3**: Lưu trữ hình ảnh sản phẩm, thực phẩm tải lên từ hệ thống.
* **AWS IAM**: Tạo role IAM gán cho EC2 giúp phân quyền truy cập S3 và CloudWatch bảo mật mà không cần ghi đè Access Key.
* **Amazon CloudWatch**: Gom log stdout từ container Docker thông qua driver `awslogs`.
* **GitHub Actions**: Tự động hóa quá trình đóng gói Docker image, đẩy lên Docker Hub và SSH cập nhật ứng dụng trên EC2.

#### Thiết kế các thành phần
* **Tầng Client**: Người dùng tương tác qua giao diện React frontend host trên Vercel.
* **Tầng DNS & CDN**: Route 53 định tuyến traffic qua CloudFront để xử lý chứng chỉ SSL HTTPS.
* **Tầng Cân bằng tải**: ALB định tuyến request tới các instance EC2 khỏe mạnh dựa trên cấu hình check endpoint `/actuator/health`.
* **Tầng Tính toán**: Ứng dụng Spring Boot chạy trong Docker trên máy chủ EC2.
* **Tầng Cơ sở dữ liệu**: RDS PostgreSQL chỉ chấp nhận kết nối cổng 5432 từ Security Group của EC2.
* **Tầng Lưu trữ**: Ảnh tải lên được đẩy trực tiếp lên S3 bucket riêng tư.

---

### 4. Triển khai kỹ thuật
#### Các giai đoạn triển khai
Dự án được triển khai qua **4 giai đoạn cốt lõi** trong lịch trình 3 tháng (12 tuần) thực tập:
1. **Nghiên cứu & Thiết kế (Tháng 1 / Tuần 1-4)**: Học tập các dịch vụ cơ bản của AWS (IAM, EC2, EBS, S3) và vẽ sơ đồ kiến trúc mạng ảo.
2. **Cấu hình Mạng & Cơ sở dữ liệu (Tháng 2 / Tuần 5-8)**: Tạo VPC, public/private subnets, Security Groups, Route Tables và cấu hình RDS PostgreSQL.
3. **Triển khai Ứng dụng & Routing (Tháng 2 / Tuần 9-10)**: Đóng gói container ứng dụng bằng Docker, khởi tạo EC2, deploy container và thiết lập ALB, Route 53, CloudFront.
4. **Tích hợp, Giám sát & CI/CD (Tháng 3 / Tuần 11-12)**: Tích hợp SDK S3 để lưu tệp, cấu hình CloudWatch Logs, viết workflow GitHub Actions và kiểm thử tích hợp toàn bộ hệ thống.

#### Yêu cầu kỹ thuật
* **Phần mềm**: Java 21, Spring Boot 3.x, Docker, Docker Compose, PostgreSQL, GitHub Actions, Vercel.
* **AWS Services**: VPC, EC2 (t3.micro), RDS PostgreSQL, S3, IAM, CloudWatch, ALB, Route 53, CloudFront.

---

### 5. Lộ trình & Mốc triển khai
* **Trước thực tập (Tháng 0)**: Đề xuất dự án ban đầu và chạy thử nghiệm code local.
* **Tháng 1 (Tuần 1-4)**: Tạo tài khoản AWS, tìm hiểu IAM, cài đặt máy chủ EC2, EBS và lưu trữ S3.
* **Tháng 2 (Tuần 5-8)**: Thiết kế mạng VPC riêng, khởi tạo RDS PostgreSQL và tìm hiểu cân bằng tải ALB.
* **Tháng 3 (Tuần 9-12)**: Định tuyến Route 53, tích hợp CDN CloudFront, tìm hiểu Lambda/Messaging, hoàn thiện hạ tầng 3-Tier CenFra-MS, chạy kiểm thử, cấu hình CI/CD và viết báo cáo.

---

### 6. Ước tính ngân sách
Bảng ước tính chi phí hạ tầng hàng tháng dựa trên AWS Pricing Calculator:

| Dịch vụ | Chi phí ước tính/tháng | Mô tả |
| --- | --- | --- |
| **Amazon EC2** | $8.00 – $15.00 | 1 instance t3.micro/t2.micro chạy liên tục |
| **EBS Storage & IPv4** | $4.00 – $7.00 | 20GB GP3 SSD + Phí địa chỉ IPv4 Public |
| **Amazon RDS PostgreSQL** | $13.00 – $20.00 | DB Single-AZ db.t3.micro (20GB Storage) |
| **Application Load Balancer** | $16.00 – $22.00 | 1 ALB (lượng LCU tối thiểu) |
| **Amazon Route 53** | $0.50 | 1 Hosted Zone quản lý tên miền |
| **Amazon S3** | < $1.00 | Dung lượng nhỏ (< 5GB) và phí request |
| **Amazon CloudFront** | ~$0.00 | Free tier hỗ trợ tới 1TB data transfer out |
| **Amazon CloudWatch** | $0.00 – $2.00 | Phí ghi log container và dashboard |
| **Tổng cộng ước tính** | **~$42.00 – $67.00/tháng** | Chi phí vận hành thông thường |

#### Chiến lược tối ưu chi phí
* Tắt máy chủ EC2 và RDS khi không sử dụng (ngoài giờ demo).
* Đặt thời hạn lưu log CloudWatch chỉ trong 1 ngày.
* Thiết lập cảnh báo ngân sách AWS Budgets gửi email khi chi phí vượt quá **$5**.
* Xóa hoàn toàn ALB và RDS sau khi hoàn thành khóa thực tập để tránh phát sinh chi phí duy trì.

---

### 7. Đánh giá rủi ro
#### Ma trận rủi ro
* **Phát sinh chi phí ngoài dự kiến**: Xác suất trung bình, ảnh hưởng cao.
* **Cấu hình sai Security Group**: Xác suất trung bình, ảnh hưởng cao.
* **Container / EC2 bị sập**: Xác suất thấp, ảnh hưởng trung bình.
* **Mất kết nối Database**: Xác suất thấp, ảnh hưởng cao.

#### Biện pháp giảm thiểu & Dự phòng
* **Chi phí**: Cấu hình cảnh báo ngân sách AWS Budgets ở ngưỡng $5.
* **Bảo mật**: Phân tầng Security Group nghiêm ngặt (Chỉ cho phép CloudFront gọi ALB; chỉ cho phép ALB gọi EC2; chỉ cho phép EC2 gọi RDS).
* **Ứng dụng**: Sử dụng cấu hình restart policy của Docker là `unless-stopped` để tự khởi động lại khi crash.
* **Dữ liệu**: Kích hoạt snapshot backup tự động hàng ngày trên RDS.

---

### 8. Kết quả kỳ vọng
* **Kết quả kỹ thuật**: Hệ thống 3-Tier hoàn chỉnh cho CenFra-MS chạy ổn định, bảo mật và tự động hóa trên AWS.
* **Nhật ký tập trung**: Logs của Docker container được gom đầy đủ về CloudWatch.
* **Triển khai tự động**: Code mới đẩy lên Git được tự động deploy thành công.
* **Tên miền bảo mật**: Hệ thống truy cập được qua HTTPS bảo mật với tên miền riêng (`https://cenframs.tuandat.space`).