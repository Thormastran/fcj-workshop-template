---
title: "Worklog Tuần 6"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 6:

* Serverless — Cấu hình SSL cho ứng dụng serverless của bạn
* AWS Certificate Manager, Amazon Route 53, Amazon CloudFront


### Các công việc cần triển khai trong tuần này:
| Ngày | Công việc                                                                                                                                                                                                 | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                 |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------------- |
| 2   | - Tạo domain, Hosted Zone và yêu cầu chứng chỉ SSL/TLS                                                                                                                                                | 10/06/2025   | 10/06/2025      | <https://000082.awsstudygroup.com/2-create-domain-hosted-zone/>                   |
| 3   | - Tạo phân phối CloudFront và dọn dẹp tài nguyên                                                                                                                                                      | 10/07/2025   | 10/07/2025      | <https://000082.awsstudygroup.com/4-create-cloud-front/>                          |
| 4   | - Serverless — Xử lý đơn hàng với Amazon SQS và Amazon SNS                                                                                                                                             | 10/08/2025   | 10/08/2025      | <https://000083.awsstudygroup.com/>                                               |
| 5   | - Tạo API và hàm Lambda<br>- **Thực hành:** <br>&emsp; + Tạo bảng `OrdersTable` (DynamoDB)<br>&emsp; + Tạo hàm Lambda `checkout_order`<br>&emsp; + Tạo hàm Lambda `order_management`<br>&emsp; + Tạo hàm Lambda `handle_order` | 10/09/2025   | 10/09/2025      | <https://000083.awsstudygroup.com/3-create-api-lambda-function/>                  |
| 6   | - Kiểm thử hoạt động web và dọn dẹp (Clean up)                                                                                                                                                        | 10/10/2025   | 10/10/2025      | <https://000083.awsstudygroup.com/4-test-operation/>                              |


### Kết quả đạt được tuần 6:

- Đã tạo domain và Hosted Zone; yêu cầu và xác thực chứng chỉ SSL/TLS bằng AWS Certificate Manager (ACM).
- Đã thiết lập phân phối Amazon CloudFront và thực hiện các bước dọn dẹp để tối ưu chi phí và tránh tài nguyên thừa.
- Triển khai mô hình xử lý đơn hàng serverless sử dụng Amazon SQS và Amazon SNS để tách rời thông điệp và gửi thông báo sự kiện.
- Xây dựng và triển khai các endpoint API REST được hỗ trợ bởi AWS Lambda:
  - Đã tạo bảng `OrdersTable` trong Amazon DynamoDB để lưu trữ đơn hàng.
  - Triển khai các hàm Lambda cho các chức năng: `checkout_order` (thanh toán), `order_management` (quản lý đơn hàng) và `handle_order` (xử lý đơn hàng).
- Đã kiểm thử hoạt động web đầu-cuối (end-to-end) và dọn dẹp các tài nguyên thử nghiệm/tạm thời.

Kỹ năng & bài học rút ra:

- Thực hành ACM, Route 53 và CloudFront để bảo mật (HTTPS) và phân phối nội dung cho ứng dụng serverless.
- Hiểu cách thiết kế luồng serverless với SQS/SNS để xử lý các tác vụ bất đồng bộ một cách tin cậy.
- Kinh nghiệm tạo bảng DynamoDB và tích hợp nó với các API do Lambda điều khiển.
- Nắm được quy trình kiểm thử, triển khai và dọn dẹp để kiểm soát chi phí và tránh để lại tài nguyên vô chủ.


