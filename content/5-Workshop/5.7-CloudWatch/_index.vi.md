---
title: "Cấu hình Amazon CloudWatch Logs"
date: 2026-07-28
weight: 7
chapter: false
pre: "<b> 5.7. </b>"
---

# Giám Sát Log Ứng Dụng EC2 với Amazon CloudWatch

#### Tổng quan

Amazon CloudWatch được sử dụng để thu thập, lưu trữ và xem xét dữ liệu vận hành do các tài nguyên AWS và ứng dụng tạo ra. Trong triển khai này, CloudWatch Logs được cấu hình để tập trung hóa log của ứng dụng **CenFra-MS** đang chạy trên một máy chủ Amazon EC2.

Các nhiệm vụ chính trong triển khai này bao gồm:

- Tạo IAM role cho Amazon EC2.
- Gắn managed policy `CloudWatchAgentServerPolicy`.
- Gán IAM role cho EC2 instance.
- Tạo CloudWatch log group và log stream.
- Xác minh rằng log của ứng dụng được gửi thành công đến CloudWatch Logs.

Luồng cấu hình được thể hiện dưới đây:

```text
Ứng dụng CenFra-MS trên EC2
            |
            v
IAM role với CloudWatchAgentServerPolicy
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
EC2 instance và các tài nguyên CloudWatch trong bài lab này được cấu hình ở Region **US West (Oregon)** (`us-west-2`). IAM là một dịch vụ toàn cầu của AWS, trong khi các tài nguyên EC2 và CloudWatch mang tính riêng biệt theo từng Region.
{{% /notice %}}

---

## 1. Tạo IAM role cho Amazon EC2

Mở AWS Management Console và truy cập:

```text
IAM → Roles → Create role
```

Tại mục **Trusted entity type**, chọn **AWS service**. Tại mục **Service or use case**, chọn **EC2**, sau đó chọn use case **EC2**.

Mối quan hệ tin cậy này cho phép dịch vụ EC2 đảm nhận (assume) IAM role và sử dụng các quyền hạn được gắn kèm.

![Chọn Amazon EC2 làm trusted entity](/images/5-Workshop/cloudwatch/01-select-trusted-entity.png)

*Hình 1: Chọn Amazon EC2 làm trusted entity cho IAM role.*

Chọn **Next** để tiếp tục đến cấu hình quyền hạn.

---

## 2. Gắn quyền CloudWatch

Trên trang **Add permissions**, chọn **Use existing policy** và tìm kiếm:

```text
CloudWatchAgentServerPolicy
```

Chọn AWS-managed policy có tên `CloudWatchAgentServerPolicy`.

![Gắn CloudWatchAgentServerPolicy](/images/5-Workshop/cloudwatch/02-add-cloudwatch-policy.png)

*Hình 2: Gắn AWS-managed policy CloudWatchAgentServerPolicy.*

Policy này cung cấp các quyền cần thiết cho CloudWatch agent chạy trên EC2 instance để xuất dữ liệu giám sát và log ứng dụng lên Amazon CloudWatch.

Chọn **Next** sau khi chọn policy.

---

## 3. Xem lại và tạo IAM role

Trang cuối cùng, xem lại trusted entity và permission policy.

Trust policy xác định Amazon EC2 là principal có thể assume role:

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

Xác nhận rằng `CloudWatchAgentServerPolicy` xuất hiện trong bản tóm tắt quyền hạn.

![Xem lại cấu hình IAM role](/images/5-Workshop/cloudwatch/03-review-iam-role.png)

*Hình 3: Xem lại EC2 trust policy và CloudWatch permission policy.*

IAM role được sử dụng trong triển khai này có tên:

```text
ec2-cloudWatch
```

Chọn **Create role** để hoàn tất cấu hình IAM role.

---

## 4. Gắn IAM role vào EC2 instance

Truy cập:

```text
EC2 → Instances
```

Chọn EC2 instance chạy ứng dụng. Trong triển khai này, tên instance là **CenFra-MS**.

Từ menu của instance, chọn:

```text
Actions → Security → Modify IAM role
```

![Mở tùy chọn Modify IAM role](/images/5-Workshop/cloudwatch/04-modify-iam-role-menu.png)

*Hình 4: Mở tùy chọn Modify IAM role từ trang EC2 instance.*

Trên trang **Modify IAM role**, chọn role đã tạo trước đó:

```text
ec2-cloudWatch
```

![Chọn CloudWatch IAM role](/images/5-Workshop/cloudwatch/05-select-iam-role.png)

*Hình 5: Chọn role ec2-cloudWatch cho instance CenFra-MS.*

Chọn **Update IAM role**.

Sau khi cập nhật, ứng dụng hoặc agent giám sát chạy trên instance có thể lấy temporary credentials thông qua EC2 instance profile. Do đó, không cần lưu trữ trực tiếp access keys lâu dài trên máy chủ.

---

## 5. Tạo CloudWatch log group

Mở Amazon CloudWatch và truy cập:

```text
CloudWatch → Logs → Log management → Create log group
```

Cấu hình log group với các giá trị sau:

| Thiết lập | Giá trị |
|---|---|
| Log group name | `ec2-c8n-clW` |
| Retention setting | `1 day` |
| Log class | `Standard` |
| KMS key | Không cấu hình |
| Deletion protection | Disabled |

![Tạo CloudWatch log group](/images/5-Workshop/cloudwatch/06-create-log-group.png)

*Hình 6: Tạo CloudWatch log group ec2-c8n-clW.*

Thời gian lưu trữ 1 ngày được chọn cho môi trường thử nghiệm để tránh lưu trữ log thử nghiệm lâu hơn mức cần thiết. Đối với môi trường sản xuất, thời gian lưu trữ nên được chọn theo yêu cầu vận hành, tuân thủ và chi phí.

Chọn **Create** để tạo log group.

---

## 6. Tạo log stream

Mở log group vừa tạo và chọn **Create log stream**.

Nhập tên log stream sau:

```text
cenfra-app
```

![Tạo log stream cho ứng dụng](/images/5-Workshop/cloudwatch/07-create-log-stream.png)

*Hình 7: Tạo cenfra-app log stream.*

Log group là container chứa các log thuộc về cùng một ứng dụng hoặc workload. Log stream đại diện cho một chuỗi các sự kiện log từ một nguồn cụ thể, chẳng hạn như một EC2 instance, container, quy trình ứng dụng hoặc file log.

Chọn **Create** để hoàn tất cấu hình log stream.

---

## 7. Xác minh log group và log stream

Quay lại log group `ec2-c8n-clW`.

Mục **Log streams** sẽ hiển thị stream mới được tạo:

```text
cenfra-app
```

![Xác minh log stream](/images/5-Workshop/cloudwatch/08-verify-log-stream.png)

*Hình 8: Xác minh cenfra-app log stream có sẵn.*

Sự xuất hiện của giá trị gần đây trong cột **Last event time** cho biết CloudWatch đã nhận được các sự kiện log cho stream.

---

## 8. Xem xét các sự kiện log của ứng dụng

Mở log stream `cenfra-app` để kiểm tra các sự kiện thu thập được.

Log stream chứa thông tin khởi động và runtime từ ứng dụng Spring Boot, bao gồm:

- Khởi tạo ứng dụng Spring Boot.
- Khởi động Embedded Tomcat trên cổng `8080`.
- Khởi tạo Spring Data JPA repository.
- Khởi động HikariCP database connection pool.
- Thông tin PostgreSQL JDBC driver và cơ sở dữ liệu.
- Khởi tạo Hibernate.
- Cấu hình Spring Security.
- Khởi tạo các endpoint ứng dụng.

![Xem log khởi động Spring Boot](/images/5-Workshop/cloudwatch/09-spring-boot-log-events.png)

*Hình 9: Các sự kiện khởi động Spring Boot được gửi đến CloudWatch Logs.*

Các sự kiện bổ sung xác nhận rằng ứng dụng đã kết nối với PostgreSQL và hoàn thành quá trình khởi tạo.

![Xem các sự kiện ứng dụng và cơ sở dữ liệu](/images/5-Workshop/cloudwatch/10-application-log-events.png)

*Hình 10: Các sự kiện Database, Hibernate, Tomcat và ứng dụng trong CloudWatch Logs.*

Trang xem sự kiện log cũng có thể được sử dụng để:

- Tìm kiếm các từ khóa như `ERROR`, `WARN`, hoặc tên exception.
- Lọc sự kiện theo khoảng thời gian tùy chỉnh.
- Xem trực tiếp (tail) các sự kiện mới theo thời gian thực.
- Mở log group trong CloudWatch Logs Insights để truy vấn nâng cao.
- Tạo metric filter từ các mẫu log khớp.

---

## Kết quả

Amazon CloudWatch Logs đã được cấu hình thành công cho workload EC2 **CenFra-MS**.

Thiết lập hoàn chỉnh mang lại các kết quả sau:

1. EC2 instance có một IAM role với quyền xuất dữ liệu giám sát.
2. IAM role được gắn vào instance `CenFra-MS`.
3. Log group CloudWatch `ec2-c8n-clW` lưu trữ các log của ứng dụng.
4. Log stream `cenfra-app` nhận các sự kiện từ ứng dụng.
5. Các sự kiện liên quan đến Spring Boot, Tomcat, Hibernate và PostgreSQL có thể được xem từ AWS Management Console.

Tập trung hóa log trong CloudWatch giúp khắc phục sự cố ứng dụng dễ dàng hơn mà không cần liên tục kết nối trực tiếp vào máy chủ EC2. Các sự kiện thu thập được sau này có thể được dùng với Logs Insights, metric filters, dashboards, alarms và các dịch vụ thông báo để xây dựng giải pháp giám sát hoàn chỉnh hơn.

---

## Ghi chú cho triển khai sản xuất (Production)

Đối với môi trường sản xuất, hãy cân nhắc các cải tiến sau:

- Áp dụng nguyên tắc quyền tối thiểu (least privilege) thay vì cấp các quyền không cần thiết.
- Cấu hình thời gian lưu trữ (retention) phù hợp với yêu cầu của tổ chức.
- Sử dụng mã hóa AWS KMS khi cần bổ sung kiểm soát mã hóa log.
- Thêm CloudWatch metric filters cho các mẫu quan trọng như `ERROR` và các lần xác thực thất bại.
- Tạo CloudWatch alarm và thông báo Amazon SNS cho các điều kiện quan trọng.
- Tránh ghi mật khẩu, token, chuỗi kết nối hoặc thông tin nhạy cảm khác vào log ứng dụng.
