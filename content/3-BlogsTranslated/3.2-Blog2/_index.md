---
title: "Blog 2"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# Best practices for utilizing AWS Systems Manager with AWS Fault Injection Service

by David Christian, Hans Nesbitt, and Neill Reidy on 26 JUN 2025 in [Advanced (300)](https://aws.amazon.com/blogs/mt/category/learning-levels/advanced-300/), [AWS Fault Injection Service (FIS)](https://aws.amazon.com/blogs/mt/category/developer-tools/aws-fault-injection-service-fis/), [AWS Systems Manager](https://aws.amazon.com/blogs/mt/category/management-tools/aws-systems-manager/), [Best Practices](https://aws.amazon.com/blogs/mt/category/post-types/best-practices/), [Management Tools](https://aws.amazon.com/blogs/mt/category/management-tools/), [Technical How-to](https://aws.amazon.com/blogs/mt/category/post-types/technical-how-to/s)[Permalink](https://aws.amazon.com/blogs/mt/best-practices-for-utilizing-aws-systems-manager-with-aws-fault-injection-service/)  Share


---

## Introduction

In today’s cloud-centric world, ensuring the resilience of mission-critical applications is paramount. The ability to withstand and recover from unexpected failures, including degradation of cloud provider services, can mean the difference between seamless operation and costly downtime. This is where the powerful combination of [AWS Systems Manager (SSM)](https://aws.amazon.com/systems-manager/) and [AWS Fault Injection Service (AWS FIS)](https://aws.amazon.com/fis/) comes into play.

FIS, launched in 2021, is a fully managed service designed to perform fault injection experiments on AWS workloads, improving reliability and resilience. While FIS provides built-in capabilities for simulating disruptive events, its integration with SSM opens a world of possibilities for creating custom, fine-grained fault injection experiments.

In this blog post, we’ll explore best practices for using SSM with FIS. We’ll delve into how this powerful duo can be leveraged to create more comprehensive and realistic chaos engineering experiments, going beyond the standard FIS actions to simulate a wider range of failure scenarios.

These testing techniques help ensure your critical applications—whether they’re custom-built solutions, enterprise systems like SAP, or web applications running on IIS—remain reliable and highly available. Using SSM, you can implement comprehensive application testing that goes beyond basic infrastructure checks to verify your entire application stack is performing as expected. This approach has helped organizations prevent service disruptions and deliver consistent experiences to their end users.
---
## AWS Fault Injection Service (FIS) overview
FIS is a managed service that enables you to perform fault injection experiments on your AWS workloads. Fault injection is based on the principles of chaos engineering. These experiments stress an application by creating disruptive events so that you can observe how your application responds. You can then use this information to improve the performance and resiliency of your applications. With FIS, you set up and run experiments that help you create the real-world conditions needed to uncover application issues.
---
## Amazon Systems Manager – Run Command overview
SSM helps you centrally view, manage, and operate instances at scale in AWS, on-premises, and multi-cloud environments. With the launch of a unified console experience, SSM consolidates various tools to help you complete common tasks across managed nodes (such as [Amazon Elastic Compute Cloud (EC2)](https://aws.amazon.com/ec2/) instances, edge devices, or on-premises servers) across AWS accounts and Regions.

SSM’s Run Command lets you execute bash and PowerShell scripts across your entire fleet of systems—enabling everything from routine administrative tasks to complex configuration changes and even fault injection testing. This powerful capability works on any managed node, whether it’s an Amazon EC2 instance or a non-EC2 machine in your hybrid and multi-cloud environment. You can trigger these scripts through multiple interfaces, including the AWS Management Console, [AWS Command Line Interface (AWS CLI)](https://aws.amazon.com/cli/), [AWS Tools for PowerShell](https://aws.amazon.com/powershell/), or AWS SDKs.
---
## Integration overview
FIS can execute fault injection experiments through Systems Manager using two actions: aws:ssm:send-command for SSM documents directly on EC2 instances, and aws:ssm:start-automation-execution for executing automation workflows. The send-command action is ideal for direct fault injection scenarios, while start-automation-execution is better suited for complex, multi-step fault injection experiments requiring orchestration. In this blog post, we’ll focus on using aws:ssm:send-command.

For direct command execution, AWS provides predefined fault injection documents (prefixed with AWSFIS) that handles common fault scenarios, like CPU spikes, disk consumption, memory leaks, and network packet loss. You can also create your own SSM Command documents containing custom fault injection logic that will be executed using the aws:ssm:send-command action.

SSM Agent is Amazon software that can be installed and configured on EC2 instances, on-premises servers, or virtual machines (VMs). This makes it possible for Systems Manager to manage these resources. The agent processes requests from SSM and then runs them as specified in the request. The AWS Systems Manager Console example below shows the FIS documents.

Note: The pre-configured SSM documents provided by FIS are supported only on EC2 instances. They are not supported on other types of managed nodes, such as on-premises servers.

More details on the AWSFIS documents can be found in the [user guide](https://docs.aws.amazon.com/fis/latest/userguide/actions-ssm-agent.html).
![ picture](/images/3-BlogsTranslated/image6.png)
Figure 1: Fault Injection documents available within AWS Systems Manager

---
## Best practices – Systems Manager documents

This section will cover best practices for implementing SSM documents for use with FIS. We will cover structure, parameter definition, OS detection, and preconditions.

*  Modular structure : Break down your documents into distinct, logical steps—similar to how AWS-managed documents typically separate dependency installation from script execution. This modular approach allows for independent failure handling, meaning if one step fails, you can isolate and address that specific issue without impacting other components. Each step can include its own cleanup actions, making it easier to roll back changes and return systems to a known good state when problems occur. The clear separation between steps also simplifies troubleshooting by helping you quickly pinpoint where issues arise, while well-defined modules can be reused across different automation documents.



```json
"mainSteps": [
  {
    "action": "aws:runPowerShellScript",
    "name": "ValidatePrerequisites",
    "description": "Validate system prerequisites before fault injection",
    "inputs": {
      "runCommand": [
        "# Check if required services are running",
        "$services = @('Spooler', 'Themes', 'AudioSrv')",
        "foreach ($service in $services) {",
        "  if ((Get-Service $service).Status -ne 'Running') {",
        "    throw \"Service $service is not running\"",
        "  }",
        "}"
      ]
    }
  },
  {
    "action": "aws:runPowerShellScript",
    "name": "StopIISAppPool",
    "description": "Stop specified IIS Application Pool",
    "inputs": {
      "runCommand": [
        "Import-Module WebAdministration",
        "Stop-WebAppPool -Name '{{AppPoolName}}'",
        "Write-Output 'Application pool {{AppPoolName}} stopped successfully'"
      ]
    }
  },
  {
    "action": "aws:runPowerShellScript",
    "name": "RestoreIISAppPool", 
    "description": "Restore IIS Application Pool to running state",
    "inputs": {
      "runCommand": [
        "Import-Module WebAdministration",
        "Start-WebAppPool -Name '{{AppPoolName}}'",
        "Write-Output 'Application pool {{AppPoolName}} restored successfully'"
      ]
    }
  }
]
```

### **Clear parameter definitions**

Parameters should be defined with type, description, and often include default values and allowed patterns. Parameters are validated using `allowedPattern` or `allowedValues` to ensure correct input:

```json
"parameters": {
  "AppPoolName": {
    "type": "String",
    "description": "Name of the IIS Application Pool to target",
    "allowedPattern": "^[a-zA-Z0-9._-]+$"
  },
  "DurationMinutes": {
    "type": "String",
    "description": "Duration in minutes for the fault injection",
    "default": "5",
    "allowedPattern": "^([1-9]|[1-5][0-9]|60)$"
  }
}
```
