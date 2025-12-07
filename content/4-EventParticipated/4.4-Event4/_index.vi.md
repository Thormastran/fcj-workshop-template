---
title: "Event4"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: "<b>4.4.</b>"
---

{{% notice warning %}}
⚠️ **Lưu ý:** Nội dung dưới đây chỉ để tham khảo. Vui lòng **không copy nguyên văn** vào báo cáo của bạn.
{{% /notice %}}

# Báo cáo tóm tắt: “AWS Security Day – 5 Pillars Bảo mật”

### Mục tiêu buổi học
- Hiểu sâu 5 trụ cột bảo mật theo AWS Well-Architected Framework  
- Biết cách thiết kế và vận hành môi trường cloud an toàn ở mức production  
- Thực hành ngay các best practices đang được doanh nghiệp lớn tại Việt Nam áp dụng  

### Speakers
- AWS Senior Security Solutions Architect  
- AWS Security Specialist Team Việt Nam  
- Khách mời từ ngân hàng/techcom đã đạt Security Pillar 100%  

### Nội dung nổi bật

#### 5 trụ cột bảo mật (Security Pillar)
1. **Identity & Access Management (IAM)**  
   – Nguyên tắc cốt lõi: không bao giờ dùng long-term credentials trong code  
   – IAM Identity Center + Permission Sets thay thế việc tạo IAM User  
   – SCP + Permission Boundaries là bắt buộc trong môi trường multi-account  
   – Mini demo: dùng IAM Access Analyzer phát hiện policy quá rộng chỉ trong 30 giây  

2. **Detection & Continuous Monitoring**  
   – GuardDuty + Security Hub + CloudTrail org-level là “bộ ba bắt buộc”  
   – Bật VPC Flow Logs và S3 Server Access Logging cho mọi account  
   – Detection-as-Code: viết rule GuardDuty custom bằng Terraform  

3. **Infrastructure Protection**  
   – VPC design chuẩn: không để EC2/RDS public nếu không thực sự cần  
   – Security Group = allow, NACL = deny cuối cùng  
   – WAF + Shield Advanced đang là xu hướng tất yếu với website/e-commerce Việt Nam  

4. **Data Protection**  
   – KMS customer-managed key + automatic rotation là default  
   – S3: Block Public Access + default encryption SSE-KMS  
   – Secrets Manager + tự động rotate database password (90 ngày/lần)  
   – Data classification: phân loại Sensitive → Restricted → Public  

5. **Incident Response (IR)**  
   – Có sẵn 3 playbook phổ biến nhất tại Việt Nam:  
     • IAM key bị leak  
     • S3 bucket bị public  
     • EC2 bị crypto miner  
   – Quy trình chuẩn: Isolate → Snapshot → Investigate → Remediate  
   – Demo auto-remediation bằng Lambda + EventBridge (tự động detach policy khi GuardDuty báo đỏ)  

### Key Takeaways (cực kỳ thực tế)
- 90% sự cố tại Việt Nam bắt nguồn từ IAM policy quá rộng hoặc thiếu MFA  
- GuardDuty bật 1 lần → phát hiện ngay hàng loạt vấn đề đang tồn tại  
- Multi-account + Landing Zone + Control Tower là cách duy nhất để scale bảo mật  
- Doanh nghiệp Việt không thiếu tool, thiếu discipline và automation  
- Security không phải “làm 1 lần” mà phải là continuous process  



### Trải nghiệm buổi học
Buổi học cực kỳ “đắt giá” với người làm cloud tại Việt Nam. Không lý thuyết suông, speaker đi thẳng vào những vấn đề thực tế đang xảy ra hàng ngày: S3 public, IAM key leak, crypto miner… Đặc biệt phần demo auto-remediation và chia sẻ case đạt 100% Security Pillar khiến mọi người “ngã ngửa” vì thấy rõ lộ trình mình cần làm gì tiếp theo.

