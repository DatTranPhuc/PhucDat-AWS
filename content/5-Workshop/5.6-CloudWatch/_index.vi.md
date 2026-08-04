---
title: "Cấu hình Amazon CloudWatch Logs"
date: 2026-07-28
weight: 6
chapter: false
pre: "<b> 5.6. </b>"
---

# Giám sát log ứng dụng EC2 bằng Amazon CloudWatch

#### Tổng quan

Amazon CloudWatch được sử dụng để thu thập, lưu trữ và theo dõi dữ liệu vận hành được tạo ra từ các tài nguyên và ứng dụng trên AWS. Trong phần triển khai này, CloudWatch Logs được cấu hình để tập trung log của ứng dụng **CenFra-MS** đang chạy trên một Amazon EC2 instance.

Quy trình cấu hình gồm các công việc chính sau:

- Tạo IAM Role dành cho Amazon EC2.
- Gắn policy `CloudWatchAgentServerPolicy`.
- Gắn IAM Role vào EC2 instance.
- Tạo CloudWatch Log Group và Log Stream.
- Kiểm tra log của ứng dụng đã được gửi thành công lên CloudWatch Logs.

Luồng hoạt động của hệ thống:

```text
Ứng dụng CenFra-MS trên EC2
            |
            v
IAM Role có CloudWatchAgentServerPolicy
            |
            v
CloudWatch Log Group: ec2-c8n-clW
            |
            v
Log Stream: cenfra-app
            |
            v
Các sự kiện log của ứng dụng
```

{{% notice info %}}
EC2 instance và các tài nguyên CloudWatch trong bài được cấu hình tại Region **US West (Oregon)**, mã Region là `us-west-2`. IAM là dịch vụ toàn cục, trong khi EC2 và CloudWatch là các dịch vụ phụ thuộc Region.
{{% /notice %}}

---

## 1. Tạo IAM Role cho Amazon EC2

Truy cập AWS Management Console và đi đến:

```text
IAM → Roles → Create role
```

Tại mục **Trusted entity type**, chọn **AWS service**.

Trong phần **Service or use case**, chọn **EC2**, sau đó tiếp tục chọn trường hợp sử dụng **EC2**.

Việc cấu hình trusted entity cho phép dịch vụ Amazon EC2 đảm nhận IAM Role và sử dụng các quyền được gắn với role đó.

![Chọn Amazon EC2 làm trusted entity](/images/5-Workshop/cloudwatch/01-select-trusted-entity.png)

*Hình 1: Chọn Amazon EC2 làm trusted entity cho IAM Role.*

Chọn **Next** để chuyển sang bước cấu hình quyền.

---

## 2. Gắn quyền CloudWatch cho IAM Role

Tại trang **Add permissions**, chọn **Use existing policy** và tìm kiếm policy:

```text
CloudWatchAgentServerPolicy
```

Chọn AWS-managed policy có tên `CloudWatchAgentServerPolicy`.

![Gắn CloudWatchAgentServerPolicy](/images/5-Workshop/cloudwatch/02-add-cloudwatch-policy.png)

*Hình 2: Gắn CloudWatchAgentServerPolicy vào IAM Role.*

Policy này cung cấp các quyền cần thiết để CloudWatch Agent chạy trên EC2 instance có thể gửi metric và log của ứng dụng lên Amazon CloudWatch.

Sau khi chọn policy, nhấn **Next**.

---

## 3. Kiểm tra và tạo IAM Role

Tại bước cuối, kiểm tra lại trusted entity và permission policy.

Trust policy cho phép dịch vụ Amazon EC2 đảm nhận role:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      }
    }
  ]
}
```

Kiểm tra trong phần permission policy summary phải có policy:

```text
CloudWatchAgentServerPolicy
```

![Kiểm tra cấu hình IAM Role](/images/5-Workshop/cloudwatch/03-review-iam-role.png)

*Hình 3: Kiểm tra EC2 trust policy và CloudWatch permission policy.*

IAM Role được sử dụng trong bài có tên:

```text
ec2-cloudWatch
```

Chọn **Create role** để hoàn tất việc tạo IAM Role.

---

## 4. Gắn IAM Role vào EC2 instance

Truy cập:

```text
EC2 → Instances
```

Chọn EC2 instance đang chạy ứng dụng. Trong bài này, instance có tên **CenFra-MS**.

Tại menu của instance, chọn:

```text
Actions → Security → Modify IAM role
```

![Mở chức năng Modify IAM role](/images/5-Workshop/cloudwatch/04-modify-iam-role-menu.png)

*Hình 4: Mở chức năng Modify IAM role trên trang quản lý EC2 instance.*

Tại trang **Modify IAM role**, chọn role vừa tạo:

```text
ec2-cloudWatch
```

![Chọn IAM Role CloudWatch](/images/5-Workshop/cloudwatch/05-select-iam-role.png)

*Hình 5: Chọn IAM Role ec2-cloudWatch cho EC2 instance CenFra-MS.*

Nhấn **Update IAM role** để hoàn tất.

Sau khi role được gắn, các ứng dụng hoặc monitoring agent chạy trên instance có thể nhận temporary credentials thông qua EC2 instance profile. Vì vậy, không cần lưu AWS Access Key trực tiếp trên máy chủ.

---

## 5. Tạo CloudWatch Log Group

Mở Amazon CloudWatch và truy cập:

```text
CloudWatch → Logs → Log management → Create log group
```

Cấu hình Log Group với các giá trị sau:

| Thuộc tính | Giá trị |
|---|---|
| Log group name | `ec2-c8n-clW` |
| Retention setting | `1 day` |
| Log class | `Standard` |
| KMS key | Không cấu hình |
| Deletion protection | Tắt |

![Tạo CloudWatch Log Group](/images/5-Workshop/cloudwatch/06-create-log-group.png)

*Hình 6: Tạo CloudWatch Log Group có tên ec2-c8n-clW.*

Thời gian lưu trữ log được đặt là một ngày vì đây là môi trường thực hành. Thiết lập này giúp các log thử nghiệm không bị lưu quá lâu và hạn chế chi phí không cần thiết.

Trong môi trường production, thời gian lưu log nên được lựa chọn dựa trên yêu cầu vận hành, bảo mật, tuân thủ và chi phí.

Nhấn **Create** để tạo Log Group.

---

## 6. Tạo Log Stream

Mở Log Group vừa tạo và chọn **Create log stream**.

Nhập tên Log Stream:

```text
cenfra-app
```

![Tạo Log Stream cho ứng dụng](/images/5-Workshop/cloudwatch/07-create-log-stream.png)

*Hình 7: Tạo Log Stream cenfra-app.*

Log Group là nơi tập hợp các log thuộc cùng một ứng dụng hoặc workload.

Log Stream đại diện cho chuỗi các log event được gửi từ một nguồn cụ thể, ví dụ:

- Một EC2 instance.
- Một container.
- Một tiến trình ứng dụng.
- Một file log trên máy chủ.

Nhấn **Create** để hoàn tất việc tạo Log Stream.

---

## 7. Kiểm tra Log Group và Log Stream

Quay lại Log Group:

```text
ec2-c8n-clW
```

Trong phần **Log streams**, kiểm tra Log Stream:

```text
cenfra-app
```

![Kiểm tra Log Stream](/images/5-Workshop/cloudwatch/08-verify-log-stream.png)

*Hình 8: Kiểm tra Log Stream cenfra-app đã được tạo và nhận dữ liệu.*

Nếu cột **Last event time** hiển thị thời gian gần nhất, điều đó cho thấy CloudWatch đã nhận được log event từ ứng dụng.

---

## 8. Kiểm tra log của ứng dụng

Mở Log Stream `cenfra-app` để xem các log event đã được thu thập.

Log Stream hiển thị thông tin khởi động và hoạt động của ứng dụng Spring Boot, bao gồm:

- Quá trình khởi động ứng dụng Spring Boot.
- Tomcat được khởi động trên port `8080`.
- Spring Data JPA khởi tạo các repository.
- HikariCP khởi tạo database connection pool.
- Thông tin PostgreSQL JDBC Driver.
- Quá trình khởi tạo Hibernate.
- Cấu hình Spring Security.
- Quá trình khởi tạo các endpoint của ứng dụng.

![Kiểm tra log khởi động Spring Boot](/images/5-Workshop/cloudwatch/09-spring-boot-log-events.png)

*Hình 9: Các sự kiện khởi động của Spring Boot được gửi đến CloudWatch Logs.*

Các sự kiện tiếp theo cho thấy ứng dụng đã kết nối với PostgreSQL và hoàn tất quá trình khởi tạo.

![Kiểm tra log cơ sở dữ liệu và ứng dụng](/images/5-Workshop/cloudwatch/10-application-log-events.png)

*Hình 10: Log của PostgreSQL, Hibernate, Tomcat và ứng dụng trên CloudWatch Logs.*

Tại trang Log Events, có thể thực hiện các thao tác:

- Tìm kiếm các từ khóa như `ERROR`, `WARN` hoặc tên exception.
- Lọc sự kiện theo khoảng thời gian.
- Theo dõi log mới gần thời gian thực bằng **Start tailing**.
- Phân tích log nâng cao bằng CloudWatch Logs Insights.
- Tạo metric filter từ các mẫu log.
- Dùng metric filter để tạo CloudWatch Alarm.

---

## Kết quả

Amazon CloudWatch Logs đã được cấu hình thành công cho EC2 workload **CenFra-MS**.

Sau khi hoàn thành, hệ thống đạt được các kết quả sau:

1. EC2 instance có IAM Role với quyền gửi dữ liệu giám sát lên CloudWatch.
2. IAM Role `ec2-cloudWatch` đã được gắn vào instance `CenFra-MS`.
3. Log Group `ec2-c8n-clW` được sử dụng để lưu log của ứng dụng.
4. Log Stream `cenfra-app` đã nhận được các log event.
5. Có thể kiểm tra các log liên quan đến Spring Boot, Tomcat, Hibernate và PostgreSQL trực tiếp trên AWS Management Console.

Việc tập trung log trên CloudWatch giúp quá trình giám sát và xử lý lỗi thuận tiện hơn mà không cần liên tục kết nối trực tiếp vào EC2 instance.

Các log này cũng có thể được sử dụng để xây dựng:

- CloudWatch Logs Insights query.
- Metric Filter.
- CloudWatch Dashboard.
- CloudWatch Alarm.
- Amazon SNS notification.

---

## Lưu ý khi triển khai trong môi trường production

Khi áp dụng cho môi trường production, nên cân nhắc các điểm sau:

- Áp dụng nguyên tắc **least privilege** cho IAM Role.
- Thiết lập thời gian lưu log phù hợp với yêu cầu của hệ thống.
- Sử dụng AWS KMS nếu cần quản lý khóa mã hóa riêng cho log.
- Tạo metric filter cho các mẫu quan trọng như `ERROR`, `WARN` hoặc đăng nhập thất bại.
- Tạo CloudWatch Alarm và Amazon SNS notification cho các sự cố nghiêm trọng.
- Không ghi password, token, connection string hoặc dữ liệu nhạy cảm vào log.
