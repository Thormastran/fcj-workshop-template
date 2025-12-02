---
title: "Nhật ký Tuần 10"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ mang tính chất tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn, bao gồm cả cảnh báo này.
{{% /notice %}}


### Mục tiêu Tuần 10:

* AWS WorkSpaces
* CloudFront với S3 Bucket 

### Các công việc cần thực hiện trong tuần:
| Ngày | Công việc                                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Chuẩn bị triển khai Amazon WorkSpaces <br> - Truy cập WorkSpaces - Trình duyệt <br> - Truy cập WorkSpaces - WorkSpaces Client                                                                                          | 11/10/2025 | 11/10/2025      |<https://000093.awsstudygroup.com/4-access-to-amazon-workspaces/>|
| 3   | - CloudFront với S3 Bucket Origin                                              | 11/11/2025 | 11/11/2025      | <https://000094.awsstudygroup.com/> |
| 4   | - Sử dụng AWS Secrets Manager với Amazon RDS và AWS Fargate <br> - Sử dụng trên RDS  | 11/12/2025 | 11/12/2025      | <https://000096.awsstudygroup.com/3-rds-phase/> |
| 5   | - Liên tục kiểm tra và giới hạn Security Groups với AWS Firewall Manager <br>&emsp; + Security Groups <br>&emsp; + AWS Firewall Manager <br>&emsp; + Quản lý Security Group  | 11/13/2025| 11/13/2025     | <https://000097.awsstudygroup.com/> |
| 6   | - Thực hành với Amazon GuardDuty                                                                                 | 11/14/2025 | 11/14/2025     | <https://000098.awsstudygroup.com/> |


### Kết quả đạt được Tuần 10:

* Triển khai và cấu hình Amazon WorkSpaces cho hạ tầng máy tính ảo:
  * Chuẩn bị và triển khai Amazon WorkSpaces với thiết lập dịch vụ thư mục phù hợp.
  * Truy cập thành công WorkSpaces thông qua client dựa trên trình duyệt.
  * Cấu hình và kiểm tra WorkSpaces desktop client để nâng cao trải nghiệm người dùng.

* Triển khai CloudFront với S3 bucket cho phân phối nội dung toàn cầu:
  * Tạo CloudFront distribution với S3 bucket làm nguồn gốc.
  * Cấu hình chính sách cache và cài đặt phân phối để đạt hiệu suất tối ưu.
  * Kiểm tra phân phối nội dung và xác thực chức năng CDN qua các khu vực khác nhau.

* Bảo mật thông tin đăng nhập cơ sở dữ liệu bằng AWS Secrets Manager:
  * Tích hợp AWS Secrets Manager với Amazon RDS để tự động xoay vòng thông tin đăng nhập.
  * Cấu hình ứng dụng AWS Fargate để lấy secrets cơ sở dữ liệu một cách an toàn.
  * Triển khai các thực tiễn tốt nhất cho quản lý secrets trong môi trường container.

* Tổng kết: Hoàn thành workshop bảo mật và hạ tầng toàn diện bao gồm máy tính ảo, phân phối nội dung, quản lý secrets, bảo mật mạng và phát hiện mối đe dọa.


