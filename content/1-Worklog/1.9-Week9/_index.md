---
title: "Week 9 Worklog"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 9 Objectives:

* VPC Components Deep Dive
* AWS Networking and Content Delivery

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Transit Gateway and Site-to-Site <br> - Datacenter Router and Transit Gateway Deployment VPNs                                                                                                 | 11/03/2025 | 11/03/2025      |<https://000092.awsstudygroup.com/4-transitgatewayandvpn/>|
| 3   | - Route53 DNS Endpoints and Internal Hosted Zones <br>&emsp; + DNS Components Deployment <br>&emsp; + DNS Testing                                            | 11/04/2025 | 11/04/2025     | <https://000092.awsstudygroup.com/5-route53/5.2-dnstesting/> |
| 4   | - VPC Endpoints for AWS Services <br> - **Practice:** <br>&emsp; + VPC Endpoints - Deployment <br>&emsp; + NP2 Endpoint Testing <br> &emsp; + VPC Endpoint Services| 11/05/2025  | 11/05/2025      | <https://000092.awsstudygroup.com/7-vpcendpointonpremise/> |
| 5   | - VPC Endpoint Services - Deployment                         | 11/06/2025 | 11/06/2025      | <https://000092.awsstudygroup.com/7-vpcendpointonpremise/7.1-vpcendpointservicesdeployment/> |
| 6   | - VPC Peering <br> - **Practice:** <br>&emsp; + VPC Peering Request<br>&emsp; + Routing Configuration <br> &emsp; + Final Testing               | 11/07/2025  | 11/07/2025     | <https://000092.awsstudygroup.com/8-vpcpeering/8.2-routeconf/> |


### Week 9 Achievements:

* Mastered AWS Transit Gateway and Site-to-Site VPN connectivity:
  * Deployed and configured Transit Gateway for multi-VPC connectivity.
  * Set up Site-to-Site VPN connections between datacenter router and AWS.
  * Configured routing tables and tested connectivity across hybrid network architecture.

* Implemented Route53 DNS solutions for internal networking:
  * Deployed DNS components including Route53 resolver endpoints.
  * Created and configured internal hosted zones for private domain resolution.
  * Conducted comprehensive DNS testing to validate resolution across VPCs and on-premises.

* Configured VPC Endpoints for secure AWS service access:
  * Deployed VPC endpoints to access AWS services without internet gateway.
  * Tested endpoint connectivity and validated secure communication paths.
  * Implemented VPC endpoint services for custom application access.
