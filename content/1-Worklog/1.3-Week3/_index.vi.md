---
title: "Worklog Tuần 3: Tìm hiểu Amazon EBS Storage & Tạo Custom AMI"
date: 2024-01-15
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Tìm hiểu các loại ổ đĩa lưu trữ khối Amazon EBS (Elastic Block Store).
* Thực hành gắn (Attach), định dạng (Format), Mount ổ đĩa EBS mới vào máy chủ EC2.
* Thực hành tạo EBS Snapshot để sao lưu dữ liệu ổ đĩa.
* Thực hành đóng gói Custom AMI (Amazon Machine Image) để tái sử dụng máy chủ.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu lý thuyết Amazon EBS Volume Types: `gp3`, `io2`, `st1`, `sc1` <br> - Phân biệt giữa Root Volume và Additional EBS Volume | 25/08/2025 | 25/08/2025 | [EBS Volume Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html) |
| 3 | - Tạo thêm 1 EBS Volume (10GB `gp3`) trong cùng Availability Zone với EC2 <br> - Attach EBS Volume vào EC2 instance và dùng các lệnh Linux (`lsblk`, `mkfs.ext4`, `mount`) để mở rộng ổ đĩa | 26/08/2025 | 26/08/2025 | [Attach & Format EBS Volume](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html) |
| 4 | - Thực hành nạp dữ liệu mẫu vào ổ EBS mới mount <br> - Tạo EBS Snapshot để sao lưu trạng thái ổ đĩa <br> - Tạo EBS Volume mới từ Snapshot vừa tạo | 27/08/2025 | 27/08/2025 | [EBS Snapshots](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html) |
| 5 | - Cấu hình máy chủ EC2 hoàn chỉnh (Cài JDK, Node.js, Web App) <br> - Tạo Custom AMI `My-Custom-WebServer-AMI` từ EC2 instance này | 28/08/2025 | 28/08/2025 | [Create an EBS-backed AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/creating-an-ami-ebs.html) |
| 6 | - Khởi tạo 1 máy chủ EC2 mới hoàn toàn từ Custom AMI vừa tạo <br> - Kiểm tra máy chủ mới đã có sẵn toàn bộ phần mềm và cấu hình từ trước mà không cần cài lại | 29/08/2025 | 29/08/2025 | [Launch Instance from AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance-from-ami.html) |

### Kết quả đạt được tuần 3:

* Hiểu rõ cách hoạt động và nâng cấp dung lượng lưu trữ với Amazon EBS.
* Làm chủ kỹ năng Mount ổ đĩa mới trên môi trường máy chủ Linux.
* Nắm vững quy trình sao lưu và khôi phục dữ liệu bằng EBS Snapshots.
* Biết cách tự tạo Custom AMI giúp nhân bản máy chủ nhanh chóng.


