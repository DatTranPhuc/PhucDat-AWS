---
title: "Worklog Tuần 4: Tìm hiểu Amazon S3 & Host Website tĩnh (Static Web)"
date: 2024-01-22
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Tìm hiểu dịch vụ lưu trữ đối tượng Amazon S3 (Simple Storage Service).
* Khởi tạo S3 Bucket, upload/download tệp tin và quản lý phân quyền S3 Object.
* Khởi chạy tính năng S3 Static Website Hosting để host trang web HTML/CSS/JS.
* Tìm hiểu các tính năng nâng cao: S3 Versioning, S3 Bucket Policy & Lifecycle Rules.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu các khái niệm Amazon S3: Buckets, Objects, Keys, Storage Classes (Standard, Intelligent-Tiering, Glacier) | 01/09/2025 | 01/09/2025 | [What is Amazon S3?](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) |
| 3 | - Khởi tạo 1 S3 Bucket mẫu (`my-sample-s3-bucket-2025`) <br> - Thực hành upload file ảnh, tài liệu và phân quyền đọc Public cho Object | 02/09/2025 | 02/09/2025 | [Creating a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-bucket.html) |
| 4 | - Bật tính năng S3 Static Website Hosting trên S3 Bucket <br> - Upload bộ source code trang web tĩnh (HTML/CSS/JS) và kiểm tra truy cập trang web qua S3 Endpoint URL | 03/09/2025 | 03/09/2025 | [Hosting a static website on S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html) |
| 5 | - Thực hành viết S3 Bucket Policy cấp quyền truy cập công khai cho trang web tĩnh <br> - Cấu hình CORS (Cross-Origin Resource Sharing) để cho phép gọi API | 04/09/2025 | 04/09/2025 | [S3 Bucket Policy Examples](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example-bucket-policies.html) |
| 6 | - Kích hoạt S3 Versioning để lưu trữ nhiều phiên bản tệp tin chống ghi đè/xóa nhầm <br> - Tạo Lifecycle Rule tự động chuyển file cũ sang tầng lưu trữ S3 Glacier sau 30 ngày | 05/09/2025 | 05/09/2025 | [Object Versioning](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) <br> [Managing Object Lifecycles](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html) |

### Kết quả đạt được tuần 4:

* Nắm vững kiến trúc và cách quản lý lưu trữ dữ liệu đối tượng với Amazon S3.
* Triển khai thành công website tĩnh trên Amazon S3 với chi phí cực thấp.
* Hiểu rõ cách viết S3 Bucket Policy và cấu hình phân quyền an toàn.
* Tự động hóa quá trình tối ưu chi phí lưu trữ lâu dài bằng S3 Lifecycle Rules.


