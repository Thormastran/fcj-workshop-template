---
title: "Event 4"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: "<b>4.4.</b>"
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy it verbatim** into your report.
{{% /notice %}}

# Summary Report: “AWS Security Day – 5 Security Pillars in Practice”

### Event Objectives
- Deep understanding of the 5 AWS Security Pillar areas  
- Learn production-grade patterns used by top Vietnamese enterprises and financial institutions  
- Walk away with immediately actionable security improvements  

### Speakers
- AWS Senior Security Solutions Architect  
- AWS Vietnam Security Specialist Team  
- Guest speaker from a local bank that achieved 100% Security Pillar compliance  

### Key Highlights

#### 1. Identity & Access Management (IAM) – Modern Approach
- Zero long-term credentials in code — always use roles and temporary credentials  
- IAM Identity Center + Permission Sets as the new default (replacing IAM Users)  
- Service Control Policies (SCPs) + Permission Boundaries mandatory for multi-account environments  
- Mini demo: IAM Access Analyzer found overly permissive policies in <30 seconds  

#### 2. Detection & Continuous Monitoring
- Mandatory trio: Organization-level CloudTrail + GuardDuty + Security Hub  
- Enable VPC Flow Logs, S3 Server Access Logging, ALB logs everywhere  
- Detection-as-Code: custom GuardDuty rules via Terraform  
- Real Vietnam story: GuardDuty caught crypto-mining on EC2 on Day 1  

#### 3. Infrastructure Protection
- VPC design principle: nothing public unless absolutely required  
- Security Groups = allow rules, NACLs = final deny layer  
- WAF + Shield Advanced now table stakes for any customer-facing application in Vietnam  

#### 4. Data Protection
- Customer-managed KMS keys with automatic rotation as default  
- S3: Block Public Access + default SSE-KMS enforced at organization level  
- Secrets Manager + automatic 90-day credential rotation pattern  
- Data classification framework: Public → Internal → Sensitive → Restricted  

#### 5. Incident Response (IR) – Ready-to-Use Playbooks
- Top 3 incidents in Vietnam:  
  1. Compromised IAM keys  
  2. Public S3 buckets  
  3. Crypto-mining on EC2  
- Standard IR flow: Isolate → Snapshot → Investigate → Remediate  
- Live demo: automated remediation with Lambda + EventBridge (auto-detach malicious IAM policy when GuardDuty fires)  

### Key Takeaways
- 90% of breaches in Vietnam start with weak IAM or missing MFA  
- Turning on GuardDuty + Security Hub instantly reveals existing critical issues  
- Multi-account strategy + Landing Zone + Control Tower is the only scalable secure model  
- Security is not a one-time project — it’s a continuous discipline  
- Vietnamese companies can (and must) achieve world-class cloud security today  


