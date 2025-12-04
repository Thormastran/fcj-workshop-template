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


## AWS Fault Injection Service (FIS) overview

FIS is a managed service that enables you to perform fault injection experiments on your AWS workloads. Fault injection is based on the principles of chaos engineering. These experiments stress an application by creating disruptive events so that you can observe how your application responds. You can then use this information to improve the performance and resiliency of your applications. With FIS, you set up and run experiments that help you create the real-world conditions needed to uncover application issues.


## Amazon Systems Manager – Run Command overview

SSM helps you centrally view, manage, and operate instances at scale in AWS, on-premises, and multi-cloud environments. With the launch of a unified console experience, SSM consolidates various tools to help you complete common tasks across managed nodes (such as [Amazon Elastic Compute Cloud (EC2)](https://aws.amazon.com/ec2/) instances, edge devices, or on-premises servers) across AWS accounts and Regions.

SSM’s Run Command lets you execute bash and PowerShell scripts across your entire fleet of systems—enabling everything from routine administrative tasks to complex configuration changes and even fault injection testing. This powerful capability works on any managed node, whether it’s an Amazon EC2 instance or a non-EC2 machine in your hybrid and multi-cloud environment. You can trigger these scripts through multiple interfaces, including the AWS Management Console, [AWS Command Line Interface (AWS CLI)](https://aws.amazon.com/cli/), [AWS Tools for PowerShell](https://aws.amazon.com/powershell/), or AWS SDKs.

## Integration overview

FIS can execute fault injection experiments through Systems Manager using two actions: aws:ssm:send-command for SSM documents directly on EC2 instances, and aws:ssm:start-automation-execution for executing automation workflows. The send-command action is ideal for direct fault injection scenarios, while start-automation-execution is better suited for complex, multi-step fault injection experiments requiring orchestration. In this blog post, we’ll focus on using aws:ssm:send-command.

For direct command execution, AWS provides predefined fault injection documents (prefixed with AWSFIS) that handles common fault scenarios, like CPU spikes, disk consumption, memory leaks, and network packet loss. You can also create your own SSM Command documents containing custom fault injection logic that will be executed using the aws:ssm:send-command action.

SSM Agent is Amazon software that can be installed and configured on EC2 instances, on-premises servers, or virtual machines (VMs). This makes it possible for Systems Manager to manage these resources. The agent processes requests from SSM and then runs them as specified in the request. The AWS Systems Manager Console example below shows the FIS documents.

Note: The pre-configured SSM documents provided by FIS are supported only on EC2 instances. They are not supported on other types of managed nodes, such as on-premises servers.

More details on the AWSFIS documents can be found in the [user guide](https://docs.aws.amazon.com/fis/latest/userguide/actions-ssm-agent.html).
![ picture](/images/3-BlogsTranslated/image6.png)
Figure 1: Fault Injection documents available within AWS Systems Manager


## Best practices – Systems Manager documents

This section will cover best practices for implementing SSM documents for use with FIS. We will cover structure, parameter definition, OS detection, and preconditions.

*  Modular structure : Break down your documents into distinct, logical steps—similar to how AWS-managed documents typically separate dependency installation from script execution. This modular approach allows for independent failure handling, meaning if one step fails, you can isolate and address that specific issue without impacting other components. Each step can include its own cleanup actions, making it easier to roll back changes and return systems to a known good state when problems occur. The clear separation between steps also simplifies troubleshooting by helping you quickly pinpoint where issues arise, while well-defined modules can be reused across different automation documents.



```json
"mainSteps": [
  {
    "action": "aws:runPowerShellScript",
    "name": "ValidatePrerequisites",
    ...

  },
  {
    "action": "aws:runPowerShellScript",
    "name": "StopIISAppPool",
    "description": "Stop specified IIS Application Pool",
    ...
  },
  {
    "action": "aws:runPowerShellScript",
    "name": "RestoreIISAppPool", 
    "description": "Restore IIS Application Pool to running state",
    ...
  }
]
```

### **Clear parameter definitions**

Clear parameter definitions: Parameters are defined with type, description, and often include default values and allowed patterns. Parameters are validated using allowedPattern or allowedValues to ensure correct input.

```json

"IISAppPoolName": {
    "type": "String",
    "default": "DefaultAppPool",
    "description": "Name of the Windows IIS Application Pool to Stop",
    "allowedPattern": "^[a-zA-Z0-9]{1,50}$"
}
```

* Leverage environment variables for dynamic configuration: Always use SSM’s built-in environment variables (like AWS_SSM_REGION_NAME) instead of hardcoding values in your scripts. This practice ensures your automation remains portable across different regions and accounts, reduces the risk of errors from manual updates, and makes your documents more maintainable. For example, rather than embedding region-specific endpoints or paths, reference AWS_SSM_REGION_NAME to automatically adapt your scripts to whatever region they’re running in. You can view variables in your session by running an AWS Run Command for the AWS-RunShellScript document and pass the appropriate command for your OS. For example, pass printenv as a command to view environment variables on Linux.

* OS detection: Implement operating system detection to enable cross-platform automation. By identifying the OS type and version before executing any commands, you can properly handle the variations across different systems. This detect-then-execute pattern allows your automation to dynamically select the appropriate package manager (such as yum for Amazon Linux/CentOS or apt for Ubuntu), locate system files correctly, and use the right command line tools for each platform. This approach ensures your automation remains reliable and consistent whether you're managing a homogeneous environment or a diverse fleet of Linux distributions.

### **Detecting Amazon Linux**

```bash
if [ -f "/etc/system-release" ] && grep -i 'Amazon Linux' /etc/system-release ; then
    if ! grep -Fiq 'VERSION_ID="2023"' /etc/os-release ; then
        # Amazon Linux 2 or earlier
        yum -y install <package>
    elif grep -Fiq 'ID="amzn"' /etc/os-release && grep -Fiq 'VERSION_ID="2023"' /etc/os-release ; then
        # Amazon Linux 2023
        yum -y install <package>
    else
        echo "Exiting - This SSM document supports: Amazon Linux, Ubuntu, CentOS (7, Stream 8, Stream 9)"
        exit 1
    fi
fi
```

### **Detecting CentOS/RHEL**

```bash
elif grep -Fiq 'ID="centos"' /etc/os-release || grep -Fiq 'ID="rhel"' /etc/os-release ; then
    # Fetch OS Version
    os_version_number=$(grep -oP '(?<=^VERSION_ID=).+' /etc/os-release | tr -d '"')
    # If the version has a decimal, this line will remove it
    os_major_version_number=${os_version_number%.*}
    # ... (EPEL repository setup)
    yum -y install <package>
fi
```

* Preconditions: Create unified, intelligent automation by using preconditions in your SSM documents. Rather than maintaining separate Windows and Linux documents, write a single document that automatically executes the right commands for each platform. For example, your document can use ‘platformType’ preconditions to run PowerShell commands on Windows servers while executing bash scripts on Linux instances. You can also control workflow logic by using preconditions to check step completion—like verifying dependencies are installed before proceeding to configuration steps. This is done by setting a precondition that checks if a previous step completed successfully, for example, ‘StringEquals: InstallDependencies: True’. This approach ensures that steps execute in the correct order and only when previous requirements are met, while still maintaining platform-specific intelligence. The result is more reliable automation that works across your entire infrastructure, with built-in validation between steps.

```yaml
- action: aws:runShellScript
  name: InstallDependencies
  precondition:
    StringEquals:
      - platformType
      - Linux
   inputs:
     # step inputs

- action: aws:runShellScript
  name: ConfigureApplication
   precondition:
   StringEquals:
    - '{{ InstallDependencies }}'
    - 'True'
 inputs:
    # step inputs
```

* OnFailure: Handle the flow of execution in the document by leveraging the OnFailure field. OnFailure supports two values ‘exit’ or ‘successAndExit’. Both immediately stop processing any steps that haven’t been defined as a ‘finallyStep’. The difference between the two is that ‘exit’ will return a failure status, but successAndExit will return success. Since we are running these in the context of FIS, we want the failure to also fail the experiment. OnFailure: exit is also a good pattern to add to your prerequisites to ensure the target is ready to run the experiment. Here’s what the previous example looks like with the onFailure added.

```yaml
- action: aws:runShellScript
  name: InstallDependencies
  precondition:
    StringEquals:
    - platformType
    - Linux
  inputs:
-	onFailure: exit
    # step inputs
```

## Code best practices
We will now look at what can we do within the SSM FIS code to ensure best practices at all layers of the automation. For additional SSM Run Command best practices, refer [use cases and best practices.](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-best-practices.html)

* Idempotency: Implement idempotency in your scripts to ensure reliability and prevent unintended side effects during retries or repeated executions. This means your script must thoroughly check the current state of the system before making any changes and only perform actions when necessary. For example, before installing software, verify if it’s already installed and at the correct version; before modifying configurations, confirm they aren’t already at the desired state. Design your scripts to handle interrupted executions gracefully by implementing state tracking mechanisms (such as status files or SSM Parameter Store entries) that record progress and allow subsequent runs to pick up where they left off.

```powershell
# Check if experiment is already running
if (Test-Path -Path 'C:\temp\fis_windows_iis_experiment.json') {
    Write-Host "ERROR: fis_windows_iis_experiment.json already exists. Exiting."
    Exit 1
}

# Verify service exists before attempting operations
if (-not (Get-IISAppPool -Name {{IISAppPoolName}} -ErrorAction SilentlyContinue)) {
    Write-Host "ERROR: Application Pool {{IISAppPoolName}} not found"
    Exit 1
}
```

* Timeouts: Set appropriate timeouts in your automation steps to enforce operational safety and enable proper error handling. Each step should include a ‘timeoutSeconds’ parameter that reflects both the expected execution time and your tolerance for delayed completion. This timeout acts as a critical safety mechanism—when a step exceeds its timeout, Systems Manager will terminate the execution and trigger cleanup procedures. This approach ensures your automation doesn’t leave systems in an unknown state during failures. For example, if a software installation step times out, your script should include cleanup logic to roll back any partial changes, such as removing incomplete installations or restoring original configurations. By combining timeouts with proper cleanup procedures, you maintain system integrity even when the control structure around your scripts fails or become unresponsive. Example from the AWSFIS-Run-CPU-Stress.

```bash
- action: aws:runShellScript
  name: StopService
  inputs:
    timeoutSeconds: 60
    runCommand:
      - |
        #!/bin/bash
        # script content here


#################################
# General post fault-execution logic #
#################################
DURATION={{ DurationSeconds }}

if [[ -z $start_time ]];then
    >&2 echo "start_time is not defined"
    exit 1;
fi

elapsed_time=$(( $(date +%s) - start_time ))

# Fail if the fault command exits successfully but the execution duration is less than the expected duration.
# This happens when Stress-ng is killed prematurely using SIGTERM or SIGINT.
if [[ "$elapsed_time" -lt "$DURATION" ]]; then
    >&2 echo "Fault took $elapsed_time seconds to execute, which is less than expected duration $DURATION"
    exit 1;
fi
```
* Logging: Scripts should include echo statements to log progress and the result of the Run command SSM document. Command output can be viewed in a number of ways and is useful for debugging the document as well as recording success messages throughout the duration of the execution. Output options can be configured as part of the Run Command, logging can be viewed by navigating to the command history in SSM, or via [Amazon Simple Storage Service (Amazon S3)](https://aws.amazon.com/s3/) bucket or [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) logs. Note that [additional configuration steps](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-rc-setting-up-cwlogs.html) are required for setting up the S3 bucket logging and/or CloudWatch logs. Also, logging can be redirected to the local OS /tmp location which may be valuable in more complex scenarios.

```powershell
function Write-Log {
    param($Message)
    $timestamp = Get-Date -Format 'yyyy-MM-dd HH:mm:ss'
    Write-Host "[$timestamp] $Message"
}

# Logging example
Write-Log "Stopping IIS Application Pool: {{IISAppPoolName}}"

# Rollback implementation
try {
    Write-Log "Restoring IIS Application Pool: {{IISAppPoolName}}"
    Start-WebAppPool -Name {{IISAppPoolName}}
    $startedPool = Get-IISAppPool -Name {{IISAppPoolName}}
    if ($startedPool.State -ne 'Started') {
        throw "Failed to start application pool"
    }
} catch {
    Write-Log "ERROR during restoration: $($_.Exception.Message)"
    throw
}
```
Output options can be configured in AWS Systems Manager > Node Tools > Run Command

![ picture](/images/3-BlogsTranslated/image7.png)
Figure 2: Output Options

Run command output can be viewed after a job is run by navigating to “Command history” and choosing “View output”

![ picture](/images/3-BlogsTranslated/image8.png)
Figure 3: – Output from run command

* Cleanup Procedures: As idempotency is a desired characteristic, we must clean up once we have completed our impairment to allow for the experiment to be re-ran. Implement cleanup procedures to remove temporary files, restore service configurations, and reset system states to their original condition. Your cleanup routines should be as thorough as your setup procedures, ensuring that each experiment truly starts from a clean slate. This includes removing any temporary files, restoring original configuration files, clearing cached data, and verifying service states. Always implement proper error handling and logging during cleanup, as failed cleanup procedures can compromise the idempotency of future experiment runs.

```powershell
# Windows (IIS) Cleanup Example
try {
    Write-Log "Cleaning up: Deleting JSON file C:\\temp\\fis_windows_iis_experiment.json"
    Remove-Item -Path C:\\temp\\fis_windows_iis_experiment.json -Force
    Write-Log "JSON file deleted successfully"
} catch {
    Write-Log "ERROR during cleanup: $($_.Exception.Message)"
throw
}
```

```bash
# Linux (HTTPD) Cleanup Example
echo 'Cleaning up: Deleting /tmp/fis_linux_service_experiment.json'
rm -f /tmp/fis_linux_service_experiment.json echo 'JSON file deleted successfully.'
```

* Service State Restoration: Your FIS experiment will stress an application in testing or production environments by creating disruptive events. As chaos testing matures into production environments, it’s critical to implement checks to ensure services are properly restored.

```powershell
# Windows (IIS) Service Restoration
try {
    Write-Log "Restoring IIS Application Pool: {{IISAppPoolName}}"
    Start-WebAppPool -Name {{IISAppPoolName}}
    $startedPool = Get-IISAppPool -Name {{IISAppPoolName}}
    if ($startedPool.State -ne 'Started') {
        throw "Failed to start application pool"
    }
    Write-Log "Application Pool restored successfully"
}
catch {
    Write-Log "ERROR during restoration: $($_.Exception.Message)"
    throw
}
```

```bash
# Linux (HTTPD) Service Restoration
echo 'Restoring service {{ServiceName}}'
sudo systemctl start {{ServiceName}}
if ! systemctl is-active --quiet {{ServiceName}}; then
    echo 'ERROR: Failed to start service {{ServiceName}}'
    exit 1
fi
echo 'Service restored successfully.'
```
* Duration Management: Proper rollback includes managing experiment duration and ensuring timely restoration. As chaos engineering practices evolve, timing of the experiment will become more precise. Other factors may come into play as part of the experiment depending on architecture being tested; for example, dynamic scaling methods may be in place for Auto Scaling Groups. Refer [planning your AWS FIS experiments](https://docs.aws.amazon.com/fis/latest/userguide/getting-started-planning.html) for more details.

```powershell
# Windows Duration Management
$elapsed_time = ((Get-Date) - $start_time).TotalSecondsWrite-Log "Elapsed time: $elapsed_time seconds"
$remaining_time = {{DurationSeconds}} - $elapsed_time
if ($remaining_time -gt 0) {
    Write-Log "Waiting for remaining time: $remaining_time seconds before restart"
Start-Sleep -Seconds $remaining_time
}
```
```bash
# Linux Duration Management
elapsed_time=$((current_time_epoch - start_time_epoch))
remaining_time=$(( {{DurationSeconds}} - elapsed_time ))if [ $remaining_time -gt 0 ]; then
echo "Waiting for remaining time: $remaining_time seconds before restart"
sleep $remaining_time
fi
```
* Error Handling During Rollback: Implement comprehensive error handling during rollback operations. As part of the fault experiment, tests should include verbose error handling to aid in debugging and simplifying the ongoing journey of Chaos Engineering. Teams should take a continuous approach to resilience in the cloud, responding and learning based on success and failure of previous experiments. Refer to the [Resilience lifecycle framework](https://docs.aws.amazon.com/prescriptive-guidance/latest/resilience-lifecycle-framework/introduction.html) for more guidance on resilience best practices and continuous improvement.

```powershell
# Windows Error Handling
try {
    # Restoration logic
} catch {
    Write-Log "ERROR: Rollback failed - $($_.Exception.Message)"
    Write-Log "Manual intervention may be required"
    throw
} finally {
    # Cleanup that must happen regardless of success/failure
    if (Test-Path -Path 'C:\temp\fis_windows_iis_experiment.json') {
        Remove-Item -Path C:\temp\fis_windows_iis_experiment.json -Force
    }
}
```

## Conclusion
Systems Manager and Fault Injection Service provide a powerful platform for implementing chaos engineering principles and improving the resilience of your AWS workloads. By following the best practices outlined in this blog, you can create more robust, efficient, and maintainable SSM documents for use with FIS. Key takeaways include implementing a modular structure, proper parameter management, OS detection, comprehensive error handling and logging, robust rollback mechanisms, effective timeout management, and proper signal handling. Additionally, utilizing preconditions, adhering to [security best practices](https://docs.aws.amazon.com/fis/latest/userguide/security.html), and thorough testing are crucial for creating effective chaos engineering experiments.

As you continue to leverage SSM and FIS in your AWS environments, remember that chaos engineering is an ongoing process. Continuously refine your approach based on the insights gained from each experiment and stay updated with the latest features and best practices as noted in the [AWS Resilience Lifecycle Framework](https://docs.aws.amazon.com/prescriptive-guidance/latest/resilience-lifecycle-framework/introduction.html). Also, for those getting started with FIS, the [Chaos Engineering](https://catalog.us-east-1.prod.workshops.aws/workshops/eb89c4d5-7c9a-40e0-b0bc-1cde2df1cb97/en-US) Workshop provides a safe environment to learn and practice experiments. By mastering the integration of Systems Manager with FIS and implementing these best practices, you’re taking a significant step towards building more resilient, fault-tolerant systems that can withstand the unpredictable nature of production environments. This not only improves your current fault injection experiments but also sets a solid foundation for future chaos engineering initiatives, ultimately leading to more reliable and robust AWS deployments.


