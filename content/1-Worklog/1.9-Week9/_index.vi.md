---
title: "Nhật ký Tuần 9"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ mang tính chất tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn, bao gồm cả cảnh báo này.
{{% /notice %}}


### Mục tiêu Tuần 9:

* Tìm hiểu sâu các thành phần VPC
* AWS Networking và Content Delivery

### Các công việc cần thực hiện trong tuần:
| Ngày | Công việc                                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Transit Gateway và Site-to-Site <br> - Datacenter Router và Transit Gateway Deployment VPNs                                                                                                 | 11/03/2025 | 11/03/2025      |<https://000092.awsstudygroup.com/4-transitgatewayandvpn/>|
| 3   | - Route53 DNS Endpoints và Internal Hosted Zones <br>&emsp; + Triển khai các thành phần DNS <br>&emsp; + Kiểm tra DNS                                            | 11/04/2025 | 11/04/2025     | <https://000092.awsstudygroup.com/5-route53/5.2-dnstesting/> |
| 4   | - VPC Endpoints cho các dịch vụ AWS <br> - **Thực hành:** <br>&emsp; + VPC Endpoints - Triển khai <br>&emsp; + Kiểm tra NP2 Endpoint <br> &emsp; + VPC Endpoint Services| 11/05/2025  | 11/05/2025      | <https://000092.awsstudygroup.com/7-vpcendpointonpremise/> |
| 5   | - VPC Endpoint Services - Triển khai                         | 11/06/2025 | 11/06/2025      | <https://000092.awsstudygroup.com/7-vpcendpointonpremise/7.1-vpcendpointservicesdeployment/> |
| 6   | - VPC Peering <br> - **Thực hành:** <br>&emsp; + Yêu cầu VPC Peering<br>&emsp; + Cấu hình Routing <br> &emsp; + Kiểm tra cuối cùng               | 11/07/2025  | 11/07/2025     | <https://000092.awsstudygroup.com/8-vpcpeering/8.2-routeconf/> |


### Kết quả đạt được Tuần 9:

* Làm chủ AWS Transit Gateway và kết nối Site-to-Site VPN:
  * Triển khai và cấu hình Transit Gateway cho kết nối đa VPC.
  * Thiết lập kết nối Site-to-Site VPN giữa datacenter router và AWS.
  * Cấu hình bảng định tuyến và kiểm tra kết nối qua kiến trúc mạng hybrid.

* Triển khai giải pháp Route53 DNS cho mạng nội bộ:
  * Triển khai các thành phần DNS bao gồm Route53 resolver endpoints.
  * Tạo và cấu hình internal hosted zones cho phân giải domain riêng tư.
  * Thực hiện kiểm tra DNS toàn diện để xác thực phân giải qua VPCs và on-premises.

* Cấu hình VPC Endpoints cho truy cập dịch vụ AWS an toàn:
  * Triển khai VPC endpoints để truy cập dịch vụ AWS mà không cần internet gateway.
  * Kiểm tra kết nối endpoint và xác thực đường truyền thông tin an toàn.
  * Triển khai VPC endpoint services cho truy cập ứng dụng tùy chỉnh.


