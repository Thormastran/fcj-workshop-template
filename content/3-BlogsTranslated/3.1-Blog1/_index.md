---
title: "Blog 1"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# AWS gears up for action-packed Gamescom and Devcom 2024

More than 320,000 gamers and games industry professionals are expected to gather in Cologne, Germany, next week as Gamescom gets underway. One of the world’s most anticipated games industry events, the conference brings together developers, publishers, hardware manufacturers, and cloud providers from around the world alongside the gaming community to celebrate industry achievements and discuss the trends shaping the industry’s future.

Historically, it’s also been a forum for launching new game development offerings, making partnership announcements, and debuting compelling technology demos. Devcom, the official game developer event of Gamescom, is an annual highlight, and Amazon Web Services (AWS) is preparing to make a splash at both events. Here’s what to expect:

---

## AWS on-site activities

Aligning purpose-built game development services and solutions from AWS and AWS Partners against six solution areas, including Cloud Game Development, Game Servers, Live Operations, Game Security, Game Analytics, and Game Artificial Intelligence (AI) and Machine Learning (ML), AWS helps developers build, run, and grow their games. AWS is participating in a range of Devcom-related activities inside and outside the Confex Exhibitor Hall, such as:

Live product demonstrations

Visitors to the AWS booth (E2) can check out real-time demos of game industry–specific AWS technology led by industry experts and partners. Among the lineup is a generative AI kiosk featuring Ada, an in-game non-player character (NPC) created with MetaHuman in Unreal Engine 5 and [Amazon Bedrock](https://aws.amazon.com/vi/bedrock/), a toolset for building generative AI applications. The demo shows how this technology can make NPCs more dynamic and intelligent to enhance the player experience.

A second demo dives deep into using Stable Diffusion within Amazon Bedrock to generate images for your mood boards, and to seamlessly integrate these generated images into Miro a popular online collaborative whiteboard platform. At the booth, AWS Partners EPAM Systems and Revolgy will also demo solutions for generative AI and cloud game development. Click the link to find out more about AWS booth demos or to schedule a meeting with an AWS industry expert at the show.

Panels and talks

AWS for Games leaders and partners will participate in a range of Devcom sessions, including:

Monday, August 19

A Fireside Chat on Generative AI in Gaming with Electronic Arts (EA) Head of Technology Partnerships Jeff Skelton and EPAM Systems Head of Gaming Solutions Vitalii Vashchuk from 11:30-12:00 CET on Stage 12 – (Confex Lvl-2).
Game Servers on AWS: How to Select for Success led by AWS for Games Senior Solutions Architect Benjamin Meyer and AWS for Games Solutions Architect Rieha Burrell from 15:00-15:30 CET on Stage 4 – (Confex Lvl-1).
Tuesday, August 20

Accelerating Game Development with AWS and Revolgy featuring AWS for Games Sr. Solutions Architect Chris Blackwell and Revolgy Senior Account Manager James Turner from 11:30-12:00 CET on Stage 4 – (Confex Lvl-1).
Networking events

The AWS team invites attendees to unwind after a busy day on the conference floor at one of many customer and partner events, including:

Monday, August 19

The fourth annual Celebrating Women in Games event, hosted by AWS, Amazon Games, and Women in Games International (WIGI) takes place on the 27th floor of Kölnsky from 18:00-21:00 CET and kicks off with a panel discussion led by WIGI CEO Joanie Kraut. After the panel, participants are invited to join a reception and networking event, followed by an after-party sponsored by DoIT. Space is limited, so be sure to reserve a spot.
Pragma’s Backend Services House networking happy hour runs from 17:00-20:00 CET. Register on the website to see the location.
Tuesday, August 20

A Community Clubhouse learning and networking event will offer sessions from industry leaders and AWS speakers from 8:00-18:30 CET. Register here.
Join CleverTap Gaming and AWS for an immersive, inspiring experience with some of the brightest minds in mobile and console gaming at the Product x LiveOps Symposium, which takes place from 16:00-17:30 CET.
From 19:00-22:00 CET, AccelByte, Unity, Edgegap, mod.io, Paddle, and AWS are hosting an exciting evening where developers can connect, enjoy food and drinks, and explore solutions. Register here.
Wednesday, August 21

Visit with Deconstructor of Fun, AppsFlyer, Heroic Labs, and other game development professionals for an intimate whiskey, wine, or water tasting at The Grid Bar starting at 16:00 CET. Register here.


**The solution architecture is now as follows:**

> *Figure 1. Overall architecture; colored boxes represent distinct services.*

---

While the term *microservices* has some inherent ambiguity, certain traits are common:  
- Small, autonomous, loosely coupled  
- Reusable, communicating through well-defined interfaces  
- Specialized to do one thing well  
- Often implemented in an **event-driven architecture**

When determining where to draw boundaries between microservices, consider:  
- **Intrinsic**: technology used, performance, reliability, scalability  
- **Extrinsic**: dependent functionality, rate of change, reusability  
- **Human**: team ownership, managing *cognitive load*

---

## Technology Choices and Communication Scope

| Communication scope                       | Technologies / patterns to consider                                                        |
| ----------------------------------------- | ------------------------------------------------------------------------------------------ |
| Within a single microservice              | Amazon Simple Queue Service (Amazon SQS), AWS Step Functions                               |
| Between microservices in a single service | AWS CloudFormation cross-stack references, Amazon Simple Notification Service (Amazon SNS) |
| Between services                          | Amazon EventBridge, AWS Cloud Map, Amazon API Gateway                                      |

---

## The Pub/Sub Hub

Using a **hub-and-spoke** architecture (or message broker) works well with a small number of tightly related microservices.  
- Each microservice depends only on the *hub*  
- Inter-microservice connections are limited to the contents of the published message  
- Reduces the number of synchronous calls since pub/sub is a one-way asynchronous *push*

Drawback: **coordination and monitoring** are needed to avoid microservices processing the wrong message.

---

## Core Microservice

Provides foundational data and communication layer, including:  
- **Amazon S3** bucket for data  
- **Amazon DynamoDB** for data catalog  
- **AWS Lambda** to write messages into the data lake and catalog  
- **Amazon SNS** topic as the *hub*  
- **Amazon S3** bucket for artifacts such as Lambda code

> Only allow indirect write access to the data lake through a Lambda function → ensures consistency.

---

## Front Door Microservice

- Provides an API Gateway for external REST interaction  
- Authentication & authorization based on **OIDC** via **Amazon Cognito**  
- Self-managed *deduplication* mechanism using DynamoDB instead of SNS FIFO because:  
  1. SNS deduplication TTL is only 5 minutes  
  2. SNS FIFO requires SQS FIFO  
  3. Ability to proactively notify the sender that the message is a duplicate  

---

## Staging ER7 Microservice

- Lambda “trigger” subscribed to the pub/sub hub, filtering messages by attribute  
- Step Functions Express Workflow to convert ER7 → JSON  
- Two Lambdas:  
  1. Fix ER7 formatting (newline, carriage return)  
  2. Parsing logic  
- Result or error is pushed back into the pub/sub hub  

---

## New Features in the Solution

### 1. AWS CloudFormation Cross-Stack References
Example *outputs* in the core microservice:
```yaml
Outputs:
  Bucket:
    Value: !Ref Bucket
    Export:
      Name: !Sub ${AWS::StackName}-Bucket
  ArtifactBucket:
    Value: !Ref ArtifactBucket
    Export:
      Name: !Sub ${AWS::StackName}-ArtifactBucket
  Topic:
    Value: !Ref Topic
    Export:
      Name: !Sub ${AWS::StackName}-Topic
  Catalog:
    Value: !Ref Catalog
    Export:
      Name: !Sub ${AWS::StackName}-Catalog
  CatalogArn:
    Value: !GetAtt Catalog.Arn
    Export:
      Name: !Sub ${AWS::StackName}-CatalogArn
