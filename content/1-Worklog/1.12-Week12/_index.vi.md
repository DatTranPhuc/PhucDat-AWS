---
title: "Worklog Tuần 12: Triển khai Dự án Kitchen Hub - Phần 2: Cân bằng tải, Kiểm thử & Báo cáo"
date: 2024-03-18
weight: 1
chapter: false
pre: " <b> 1.12. </b> "
---

### Mục tiêu tuần 12:

* Hoàn thiện kiến trúc Nâng cao cho Kitchen Hub: Tích hợp Application Load Balancer (ALB), Auto Scaling Group (ASG) & CloudFront CDN.
* Thực hiện kiểm thử tích hợp toàn diện (End-to-End Testing) và kiểm tra khả năng phục hồi sự cố.
* Tối ưu hóa chi phí vận hành trên AWS Cost Explorer và dọn dẹp các tài nguyên thử nghiệm.
* Tổng kết thành quả, cập nhật đầy đủ thông tin báo cáo thực tập trên trang Hugo và xuất bản báo cáo hoàn chỉnh.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Triển khai Application Load Balancer (`KitchenHub-ALB`) tiếp nhận traffic công khai <br> - Cấu hình Auto Scaling Group (`KitchenHub-ASG`) tự động co giãn EC2 Backend trong 2 Private Subnets | 27/10/2025 | 27/10/2025 | [Attach Load Balancer to ASG](https://docs.aws.amazon.com/autoscaling/ec2/userguide/attach-load-balancer-asg.html) |
| 3 | - Tạo CloudFront Distribution tăng tốc phân phối dữ liệu cho Dashboard UI (S3) và Backend API (ALB) <br> - Cấu hình SSL/TLS HTTPS mã hóa kết nối | 28/10/2025 | 28/10/2025 | [Using ELB as CloudFront Origin](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-elb-as-origin.html) |
| 4 | - Kiểm thử toàn diện luồng End-to-End: Trình duyệt -> CloudFront CDN -> ALB -> EC2 Spring Boot -> RDS MySQL & S3 Media <br> - Thử nghiệm ngắt 1 EC2 để xác nhận ALB & ASG tự động khắc phục lỗi | 29/10/2025 | 29/10/2025 | [Fault Tolerance in AWS](https://aws.amazon.com/blogs/architecture/fault-tolerance-in-aws/) |
| 5 | - Đánh giá chi phí trên AWS Cost Explorer, tối ưu tài nguyên và dọn dẹp các tài nguyên thừa | 30/10/2025 | 30/10/2025 | [AWS Cost Explorer Guide](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html) |
| 6 | - Kiểm tra lại toàn bộ trang web báo cáo Hugo, biên dịch lệnh `hugo --minify` <br> - Hoàn thành báo cáo tổng kết 12 tuần thực tập dự án Kitchen Hub trên AWS | 31/10/2025 | 31/10/2025 | [Hugo Documentation](https://gohugo.io/documentation/) |

### Kết quả đạt được tuần 12:

* Hoàn thành 100% việc xây dựng hệ thống **Kitchen Hub Architecture on AWS** chuẩn kiến trúc 3 tầng Enterprise.
* Hệ thống đạt khả năng chịu lỗi cao (Fault Tolerant), tự động co giãn (Auto Scaling) và bảo mật mã hóa HTTPS.
* Tối ưu hóa chi phí sử dụng dịch vụ đám mây AWS hiệu quả.
* Hoàn thành xuất sắc báo cáo thực tập 12 tuần trên trang Hugo static report.
