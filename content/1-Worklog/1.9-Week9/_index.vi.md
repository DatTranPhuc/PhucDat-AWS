---
title: "Worklog Tuần 9: Tìm hiểu Amazon Route 53 (DNS) & CloudFront CDN"
date: 2024-02-26
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

* Tìm hiểu dịch vụ quản lý tên miền Amazon Route 53 và mạng phân phối nội dung Amazon CloudFront.
* Quản lý các bản ghi DNS (A Record, CNAME Record, Alias Record).
* Khởi tạo CloudFront Distribution để cache dữ liệu từ S3 Bucket và tăng tốc truy cập.
* Cấu hình SSL/TLS Certificate với AWS Certificate Manager (ACM) để bật kết nối HTTPS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu dịch vụ DNS Amazon Route 53: Hosted Zones, A/CNAME/Alias Records và Routing Policies | 06/10/2025 | 06/10/2025 | [What is Amazon Route 53?](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html) |
| 3 | - Tạo Route 53 Hosted Zone và thực hành trỏ bản ghi A Record / Alias Record về máy chủ EC2 / S3 Static Web | 07/10/2025 | 07/10/2025 | [Routing traffic to AWS resources](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-routing-hub.html) |
| 4 | - Tìm hiểu Amazon CloudFront CDN: Edge Locations, Origin, Caching & Time-To-Live (TTL) | 08/10/2025 | 08/10/2025 | [What is Amazon CloudFront?](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html) |
| 5 | - Khởi tạo CloudFront Distribution trỏ Origin vào S3 Static Website Bucket <br> - Thử nghiệm truy cập qua domain CloudFront (`*.cloudfront.net`) và kiểm tra tốc độ tải trang | 09/10/2025 | 09/10/2025 | [CloudFront S3 Origins](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/DownloadDistS3AndCustomOrigins.html) |
| 6 | - Yêu cầu chứng chỉ SSL/TLS miễn phí trên AWS Certificate Manager (ACM) <br> - Gán chứng chỉ ACM vào CloudFront Distribution và bật chế độ tự động chuyển hướng HTTP sang HTTPS | 10/10/2025 | 10/10/2025 | [AWS Certificate Manager Overview](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html) |

### Kết quả đạt được tuần 9:

* Hiểu rõ nguyên lý hoạt động của hệ thống phân giải tên miền Amazon Route 53.
* Biết cách dùng Amazon CloudFront CDN giúp tối ưu hóa tốc độ phân phối nội dung tĩnh.
* Quản lý chứng chỉ SSL/TLS và bảo mật trang web bằng HTTPS mã hóa an toàn.
* Giảm tải cho gốc máy chủ (Origin) nhờ cơ chế Caching tại các Edge Locations.


