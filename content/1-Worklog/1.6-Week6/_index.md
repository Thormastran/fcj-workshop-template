---
title: "Week 6 Worklog"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 6 Objectives:

* Serverless - Setting up SSL for your serverless app
* AWS Certificate Manager,Amazon Route 53,Amazon CloudFront

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Create Domain , Hosted zone and Request SSL certificate                                                                                                 | 10/06/2025 | 10/06/2025     |<https://000082.awsstudygroup.com/2-create-domain-hosted-zone/>|
| 3   | - Create CloudFront distribution and clean up| 10/07/2025  | 10/07/2025     | <https://000082.awsstudygroup.com/4-create-cloud-front/> |
| 4   | - Serverless - Processing orders with SQS and SNS | 10/08/2025 | 10/08/2025     | <https://000083.awsstudygroup.com/> |
| 5   | - Create API and Lambda functionCreate API and Lambda function<br>- **Practice:** <br>+Create OrdersTable DynamoDB table<br>+Create checkout_order Lambda function<br>+Create order_management Lambda function<br>+Create handle_order Lambda function<br>| 10/09/2025 | 10/09/2025     | <https://000083.awsstudygroup.com/3-create-api-lambda-function/> |
| 6   | - Test web operation and Clean up                                                                                | 10/10/2025 | 10/10/2025      | <https://000083.awsstudygroup.com/4-test-operation/> |


### Week 6 Achievements:

- Created domain and hosted zone, requested and validated an SSL/TLS certificate using AWS Certificate Manager.
- Provisioned an Amazon CloudFront distribution and performed cleanup steps to reduce cost/resources.
- Implemented serverless order-processing patterns using Amazon SQS and Amazon SNS for decoupled messaging and event notification.
- Built and deployed REST API endpoints backed by AWS Lambda functions:
- Created an OrdersTable in Amazon DynamoDB for order persistence.
- Implemented Lambda functions for checkout (checkout_order), order management (order_management), and order handling (handle_order).
- Tested end-to-end web operations and performed cleanup of temporary/test resources.