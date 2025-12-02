---
title: "Nhật ký Tuần 11"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ mang tính chất tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn, bao gồm cả cảnh báo này.
{{% /notice %}}


### Mục tiêu Tuần 11:

* AWS Elastic Disaster Recovery Workshop
* Cơ sở dữ liệu

### Các công việc cần thực hiện trong tuần:
| Ngày | Công việc                                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Triển khai AutoScaling cho WordPress Instance                                                                                                | 11/17/2025 | 11/17/2025      |<https://000101.awsstudygroup.com/4-asgforec2/>|
| 3   | - Khởi tạo AMI từ WebServer Instance <br>&emsp; + Launch Template <br>&emsp; + Tạo Load Balancer <br>&emsp; + Tạo Auto Scaling Group <br>&emsp; + Cơ sở dữ liệu <br>&emsp; + ... <br>                                              | 11/18/2025 | 11/18/2025     | <https://000101.awsstudygroup.com/4-asgforec2/4.5-createasg/> |
| 4   | - Sao lưu và khôi phục Cơ sở dữ liệu <br> - **Thực hành:** <br>&emsp; + Tạo DB Snapshot <br>&emsp; + Khôi phục với DB Snapshot | 11/19/2025| 11/19/2025     | <https://000101.awsstudygroup.com/5-backupandrestore/5.2-restorewithsnapshot/> |
| 5   | - Tạo CloudFront cho Web Server                  | 11/20/2025 | 11/20/2025      | <https://000101.awsstudygroup.com/6-createcloudfront/> |
| 6   | - Giới thiệu về Infrastructure as Code                                                                          | 11/21/2025 | 11/21/2025     | <https://000102.awsstudygroup.com/2-pre/> |


### Kết quả đạt được Tuần 11:

* Triển khai Auto Scaling cho các WordPress instance để xử lý dao động lưu lượng truy cập:
  * Cấu hình các chính sách Auto Scaling dựa trên sử dụng CPU và các metrics khác.
  * Thiết lập các trigger và ngưỡng scaling cho việc cung cấp instance tự động.
  * Kiểm tra hành vi scaling trong các điều kiện tải khác nhau.

* Tạo hạ tầng web có thể mở rộng với AMI và Load Balancer:
  * Tạo AMI tùy chỉnh từ web server instance đã được cấu hình.
  * Thiết kế và triển khai Launch Template với cấu hình instance phù hợp.
  * Triển khai Application Load Balancer để phân phối lưu lượng giữa các instance.
  * Tạo và cấu hình thành công Auto Scaling Group để đảm bảo tính khả dụng cao.

* Làm chủ các quy trình sao lưu và khôi phục thảm họa cơ sở dữ liệu:
  * Tạo snapshots cơ sở dữ liệu tự động và thủ công cho khôi phục theo thời gian.
  * Thực hành khôi phục cơ sở dữ liệu từ snapshots với thời gian ngừng hoạt động tối thiểu.
  * Triển khai các chính sách lưu giữ sao lưu và chiến lược sao lưu cross-region.
  * Xác thực tính toàn vẹn và nhất quán của dữ liệu sau quá trình khôi phục.

* Tổng kết: Hoàn thành workshop khôi phục thảm họa và khả năng mở rộng toàn diện bao gồm auto scaling, sao lưu/khôi phục cơ sở dữ liệu, tăng tốc phân phối nội dung và nền tảng tự động hóa hạ tầng.


