---
title: "Event 3"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: "<b>4.3.</b>"
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy it verbatim** into your report, including this warning.
{{% /notice %}}

# Summary Report: “AWS DevOps Day – From CI/CD to Observability”

### Event Objectives
- Master the complete modern AWS DevOps stack in one intensive day
- Understand end-to-end CI/CD, Infrastructure as Code, Containers, and Observability
- Hands-on experience through multiple live demos
- Learn real-world best practices and transformation case studies

### Speakers
- AWS Senior Solutions Architects (DevOps & Containers)
- AWS Developer Advocates
- DevOps engineering Tymex

### Key Highlights

#### DevOps Culture & Core Metrics
- Strong focus on DORA metrics: Deployment Frequency, Lead Time for Changes, MTTR, Change Failure Rate
- Shift from “DevOps team” → DevOps culture across the entire organization

#### Full AWS CI/CD Pipeline (CodeSuite)
- Source: CodeCommit + Trunk-based development (moving away from GitFlow)
- Build & Test: CodeBuild with parallel testing and caching
- Deploy: CodeDeploy supporting Blue/Green, Canary, and Linear rollouts
- Orchestration: CodePipeline with manual approval gates
- Live demo: Zero-downtime deployment pipeline built from scratch in <20 minutes

#### Infrastructure as Code – CloudFormation vs CDK
- CloudFormation: reliable, mature, drift detection
- AWS CDK: developer-friendly (TypeScript/Python/Java/C#), reusable constructs, faster iteration
- Clear decision framework: use CDK for new projects, CloudFormation for regulated environments
- Live demo: same VPC + ECS service deployed with both tools side-by-side

#### Container Ecosystem on AWS
- ECR: vulnerability scanning + image signing
- ECS Fargate vs EKS: when to choose serverless containers vs full Kubernetes
- AWS App Runner: the fastest way to production for simple web apps
- Case study comparison: startup (App Runner) vs enterprise finance (EKS)

#### Observability Triad
- CloudWatch: metrics, logs, alarms, Container Insights, Lambda Insights
- X-Ray: distributed tracing across Lambda, ECS, API Gateway
- Live demo: full observability stack set up in 10 minutes with automated dashboards

#### Advanced DevOps Practices
- Feature flags + dark launches
- Chaos engineering introduction
- Blameless postmortems culture
- Real customer stories: 1 startup went from 1 deploy/month → 50 deploys/day; 1 bank reduced MTTR from 4 hours → <15 minutes

### Key Takeaways
- Trunk-based development + short-lived branches is the fastest path to high DORA performance
- AWS CDK has become the default choice for 90% of new projects at AWS customers
- You no longer need to choose between “simple” and “powerful” — App Runner → ECS → EKS is a smooth progression path
- Observability must be designed Day 1, not added later
- Zero-downtime deployments are now table stakes, not nice-to-have

### Applying to Work (30-60-90 Day Plan)
- 30 days: migrate at least 1 repo to Trunk-based + enable CodeBuild caching
- 60 days: convert 1 existing CloudFormation template to CDK (or start new project with CDK)
- 90 days: implement Blue/Green or Canary deployment for 1 critical service + full CloudWatch/X-Ray observability
- Long-term: target Elite DORA performer status in 2026

### Event Experience
One of the most intense and value-packed workshops I’ve ever attended — full day from 8:30 AM to 5:00 PM, yet time flew by because every minute was either live demo or real-world discussion. The demos were 100% reproducible (speaker shared GitHub repo immediately). The case-study session with Vietnamese companies was particularly inspiring — proved that local teams can achieve world-class DevOps maturity.

> “This wasn’t a slideware workshop — we built and deployed production-grade pipelines, IaC, containers, and observability in real time.”

Hands down the best AWS DevOps learning experience in 2025 so far.

*Event photos*  
*(Add photos: group photo, live demo screenshots, white-boarding sessions, networking, etc.)*

Highly recommend this format to any team serious about modern cloud development!