---
title: "Sự kiện 3"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b>4.3.</b>"
---

{{% notice warning %}}
⚠️ **Lưu ý:** Nội dung dưới đây chỉ để tham khảo. Vui lòng **không copy nguyên văn** vào báo cáo của bạn (kể cả dòng cảnh báo này).
{{% /notice %}}

# Báo cáo tóm tắt: “AWS DevOps Day – Từ CI/CD đến Observability”

### Mục tiêu buổi học
- Nắm trọn stack DevOps hiện đại trên AWS chỉ trong 1 ngày  
- Thực hành thực tế CI/CD, IaC, Containers và Observability qua nhiều live demo  
- Học best practices và case study chuyển đổi DevOps thực tế tại Việt Nam  

### Speakers
- Các anh/chị Senior Solutions Architect chuyên DevOps & Containers của AWS  
- AWS Developer Advocate  


### Nội dung nổi bật

#### Văn hóa DevOps & các chỉ số quan trọng
- Tập trung mạnh vào 4 chỉ số DORA (Deployment Frequency, Lead Time, MTTR, Change Failure Rate)  
- DevOps không phải là “có một team DevOps” mà là văn hoá toàn tổ chức  

#### CI/CD hoàn chỉnh với AWS CodeSuite
- Source: CodeCommit + chiến lược Trunk-based (bỏ dần GitFlow)  
- Build & Test: CodeBuild với parallel test + caching  
- Deploy: CodeDeploy hỗ trợ Blue/Green, Canary, Linear  
- Orchestrate: CodePipeline + approval gate  
- Live demo: dựng full pipeline zero-downtime trong chưa tới 20 phút  

#### Infrastructure as Code – CloudFormation vs CDK
- CloudFormation: ổn định, có drift detection  
- AWS CDK: thân thiện với developer (TypeScript/Python/Java…), viết nhanh hơn 3–5 lần  
- Kết luận rõ ràng: dự án mới → ưu tiên CDK; môi trường cần tuân thủ nghiêm ngặt → CloudFormation  
- Live demo: cùng một VPC + ECS service được deploy bằng cả 2 công cụ để so sánh  

#### Hệ sinh thái Container trên AWS
- ECR: scan lỗ hổng + ký image  
- ECS Fargate vs EKS: khi nào dùng serverless, khi nào cần Kubernetes đầy đủ  
- AWS App Runner: cách nhanh nhất đưa web app lên production  
- Case study thực tế: startup dùng App Runner, ngân hàng dùng EKS  

#### Observability toàn diện
- CloudWatch (Metrics, Logs, Alarms, Container Insights, Lambda Insights)  
- X-Ray: distributed tracing xuyên service  
- Live demo: bật full observability + dashboard tự động chỉ trong 10 phút  

#### Best Practices & Case Studies
- Feature flags, A/B testing, dark launch  
- Chaos engineering cơ bản  
- Blameless postmortem  
- Case Việt Nam: 1 startup từ 1 deploy/tháng → 50 deploy/ngày; 1 ngân hàng giảm MTTR từ 4 tiếng xuống dưới 15 phút  

### Key Takeaways
- Trunk-based + short-lived branch là cách nhanh nhất để đạt Elite DORA  
- AWS CDK giờ là lựa chọn mặc định cho 90% dự án mới  
- Không còn phải chọn giữa “đơn giản” và “mạnh” → App Runner → ECS → EKS là lộ trình mượt mà  
- Observability phải thiết kế từ ngày đầu, không phải “thêm sau”  
- Zero-downtime deployment giờ là bắt buộc, không còn là “nice-to-have”  

### Áp dụng vào công việc (kế hoạch 30-60-90 ngày)
- 30 ngày: chuyển ít nhất 1 repo sang Trunk-based + bật cache CodeBuild  
- 60 ngày: chuyển 1 template CloudFormation cũ sang CDK (hoặc bắt đầu dự án mới bằng CDK)  
- 90 ngày: triển khai Blue/Green hoặc Canary cho 1 service quan trọng + bật full CloudWatch + X-Ray  
- Mục tiêu dài hạn: đạt mức Elite DORA trong năm 2026  

### Trải nghiệm buổi học
Buổi học siêu “nặng đô” từ 8g30 sáng đến 5g chiều nhưng không hề thấy dài vì hầu như toàn live demo và thảo luận thực tế. Tất cả demo đều có thể về nhà chạy lại ngay (speaker share luôn GitHub repo). Phần chia sẻ case study từ các công ty Việt Nam cực kỳ truyền cảm hứng – chứng minh team Việt hoàn toàn làm được DevOps đẳng cấp thế giới.

> “Đây không phải workshop slide, mà là chúng mình cùng build và deploy pipeline, IaC, container, observability production-grade ngay tại chỗ.”

Chắc chắn là buổi DevOps đáng giá nhất mình tham gia trong năm 2025 tính đến giờ!

*Ảnh sự kiện*  
*(Thêm ảnh chụp nhóm, ảnh màn hình demo, ảnh whiteboard, ảnh networking… tại đây)*

Rất khuyến khích team mình tham gia các buổi tương tự trong tương lai!