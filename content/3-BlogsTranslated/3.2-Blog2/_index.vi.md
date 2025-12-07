---
title: "Blog 2"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Thực tiễn tốt nhất để sử dụng AWS Systems Manager với AWS Fault Injection Service

bởi David Christian, Hans Nesbitt, và Neill Reidy vào ngày 26 tháng 6 năm 2025 trong [Nâng cao (300)](https://aws.amazon.com/blogs/mt/category/learning-levels/advanced-300/), [AWS Fault Injection Service (FIS)](https://aws.amazon.com/blogs/mt/category/developer-tools/aws-fault-injection-service-fis/), [AWS Systems Manager](https://aws.amazon.com/blogs/mt/category/management-tools/aws-systems-manager/), [Thực tiễn tốt nhất](https://aws.amazon.com/blogs/mt/category/post-types/best-practices/), [Công cụ quản lý](https://aws.amazon.com/blogs/mt/category/management-tools/), [Hướng dẫn kỹ thuật](https://aws.amazon.com/blogs/mt/category/post-types/technical-how-to/s)[Permalink](https://aws.amazon.com/blogs/mt/best-practices-for-utilizing-aws-systems-manager-with-aws-fault-injection-service/)  Chia sẻ

---

## Giới thiệu

Trong thế giới tập trung vào đám mây ngày nay, đảm bảo khả năng phục hồi của các ứng dụng quan trọng là tối quan trọng. Khả năng chịu đựng và phục hồi từ các lỗi bất ngờ, bao gồm sự suy giảm của các dịch vụ nhà cung cấp đám mây, có thể tạo ra sự khác biệt giữa hoạt động liền mạch và thời gian ngừng hoạt động tốn kém. Đây là nơi sự kết hợp mạnh mẽ của [AWS Systems Manager (SSM)](https://aws.amazon.com/systems-manager/) và [AWS Fault Injection Service (AWS FIS)](https://aws.amazon.com/fis/) phát huy tác dụng.

FIS, được ra mắt vào năm 2021, là một dịch vụ được quản lý đầy đủ được thiết kế để thực hiện các thí nghiệm tiêm lỗi trên các workload AWS, cải thiện độ tin cậy và khả năng phục hồi. Trong khi FIS cung cấp khả năng tích hợp sẵn để mô phỏng các sự kiện gián đoạn, việc tích hợp với SSM mở ra một thế giới khả năng để tạo các thí nghiệm tiêm lỗi tùy chỉnh, chi tiết.

Trong bài blog này, chúng tôi sẽ khám phá các thực tiễn tốt nhất để sử dụng SSM với FIS. Chúng tôi sẽ đi sâu vào cách bộ đôi mạnh mẽ này có thể được tận dụng để tạo ra các thí nghiệm kỹ thuật hỗn loạn toàn diện và thực tế hơn, vượt ra ngoài các hành động FIS tiêu chuẩn để mô phỏng một loạt rộng hơn các kịch bản lỗi.

Những kỹ thuật kiểm thử này giúp đảm bảo các ứng dụng quan trọng của bạn—cho dù chúng là các giải pháp tùy chỉnh, hệ thống doanh nghiệp như SAP, hoặc ứng dụng web chạy trên IIS—vẫn đáng tin cậy và có tính khả dụng cao. Sử dụng SSM, bạn có thể triển khai kiểm thử ứng dụng toàn diện vượt ra ngoài các kiểm tra cơ sở hạ tầng cơ bản để xác minh toàn bộ stack ứng dụng của bạn đang hoạt động như mong đợi. Cách tiếp cận này đã giúp các tổ chức ngăn ngừa gián đoạn dịch vụ và cung cấp trải nghiệm nhất quán cho người dùng cuối của họ.

## Tổng quan AWS Fault Injection Service (FIS)

FIS là một dịch vụ được quản lý cho phép bạn thực hiện các thí nghiệm tiêm lỗi trên các workload AWS của bạn. Tiêm lỗi dựa trên các nguyên tắc của kỹ thuật hỗn loạn. Những thí nghiệm này gây căng thẳng cho ứng dụng bằng cách tạo ra các sự kiện gián đoạn để bạn có thể quan sát cách ứng dụng của bạn phản ứng. Sau đó bạn có thể sử dụng thông tin này để cải thiện hiệu suất và khả năng phục hồi của ứng dụng. Với FIS, bạn thiết lập và chạy các thí nghiệm giúp bạn tạo ra các điều kiện thế giới thực cần thiết để khám phá các vấn đề ứng dụng.

## Tổng quan Amazon Systems Manager – Run Command

SSM giúp bạn xem, quản lý và vận hành các instance ở quy mô lớn trong AWS, môi trường tại chỗ và đa đám mây một cách tập trung. Với việc ra mắt trải nghiệm console thống nhất, SSM tích hợp các công cụ khác nhau để giúp bạn hoàn thành các tác vụ thông thường trên các node được quản lý (chẳng hạn như [Amazon Elastic Compute Cloud (EC2)](https://aws.amazon.com/ec2/) instances, thiết bị biên, hoặc server tại chỗ) trên các tài khoản và Region AWS.

Run Command của SSM cho phép bạn thực thi các script bash và PowerShell trên toàn bộ hệ thống của bạn—cho phép mọi thứ từ các tác vụ quản trị thường xuyên đến thay đổi cấu hình phức tạp và thậm chí kiểm thử tiêm lỗi. Khả năng mạnh mẽ này hoạt động trên bất kỳ node được quản lý nào, cho dù đó là Amazon EC2 instance hoặc máy không phải EC2 trong môi trường hybrid và đa đám mây của bạn. Bạn có thể kích hoạt các script này thông qua nhiều giao diện, bao gồm AWS Management Console, [AWS Command Line Interface (AWS CLI)](https://aws.amazon.com/cli/), [AWS Tools for PowerShell](https://aws.amazon.com/powershell/), hoặc AWS SDK.

## Tổng quan tích hợp

FIS có thể thực hiện các thí nghiệm tiêm lỗi thông qua Systems Manager bằng cách sử dụng hai hành động: aws:ssm:send-command cho các tài liệu SSM trực tiếp trên EC2 instances, và aws:ssm:start-automation-execution để thực thi quy trình tự động hóa. Hành động send-command lý tưởng cho các kịch bản tiêm lỗi trực tiếp, trong khi start-automation-execution phù hợp hơn cho các thí nghiệm tiêm lỗi phức tạp, nhiều bước yêu cầu điều phối. Trong bài blog này, chúng tôi sẽ tập trung vào việc sử dụng aws:ssm:send-command.

Để thực thi lệnh trực tiếp, AWS cung cấp các tài liệu tiêm lỗi được định nghĩa trước (có tiền tố AWSFIS) xử lý các kịch bản lỗi thường gặp, như tăng đột biến CPU, tiêu thụ ổ đĩa, rò rỉ bộ nhớ và mất gói mạng. Bạn cũng có thể tạo tài liệu SSM Command của riêng mình chứa logic tiêm lỗi tùy chỉnh sẽ được thực thi bằng hành động aws:ssm:send-command.

SSM Agent là phần mềm Amazon có thể được cài đặt và cấu hình trên EC2 instances, server tại chỗ, hoặc máy ảo (VM). Điều này làm cho Systems Manager có thể quản lý những tài nguyên này. Agent xử lý các yêu cầu từ SSM và sau đó chạy chúng như được chỉ định trong yêu cầu. Ví dụ AWS Systems Manager Console dưới đây hiển thị các tài liệu FIS.

Lưu ý: Các tài liệu SSM được cấu hình trước do FIS cung cấp chỉ được hỗ trợ trên EC2 instances. Chúng không được hỗ trợ trên các loại node được quản lý khác, chẳng hạn như server tại chỗ.

Thông tin chi tiết hơn về các tài liệu AWSFIS có thể được tìm thấy trong [hướng dẫn người dùng](https://docs.aws.amazon.com/fis/latest/userguide/actions-ssm-agent.html).

![ picture](/images/3-BlogsTranslated/image6.png)
Hình 1: Tài liệu Fault Injection có sẵn trong AWS Systems Manager

## Thực tiễn tốt nhất – Tài liệu Systems Manager

Phần này sẽ đề cập đến các thực tiễn tốt nhất để triển khai tài liệu SSM để sử dụng với FIS. Chúng tôi sẽ đề cập đến cấu trúc, định nghĩa tham số, phát hiện hệ điều hành và điều kiện tiên quyết.

* Cấu trúc mô-đun: Chia nhỏ tài liệu của bạn thành các bước riêng biệt, logic—tương tự như cách các tài liệu được quản lý bởi AWS thường tách biệt việc cài đặt phụ thuộc khỏi việc thực thi script. Cách tiếp cận mô-đun này cho phép xử lý lỗi độc lập, có nghĩa là nếu một bước thất bại, bạn có thể cô lập và giải quyết vấn đề cụ thể đó mà không ảnh hưởng đến các thành phần khác. Mỗi bước có thể bao gồm các hành động dọn dẹp riêng của nó, giúp việc rollback thay đổi và trả hệ thống về trạng thái tốt đã biết dễ dàng hơn khi xảy ra vấn đề. Việc tách biệt rõ ràng giữa các bước cũng đơn giản hóa việc khắc phục sự cố bằng cách giúp bạn nhanh chóng xác định nơi xảy ra vấn đề, trong khi các mô-đun được định nghĩa rõ ràng có thể được tái sử dụng trên các tài liệu tự động hóa khác nhau.

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

### **Định nghĩa tham số rõ ràng**

Định nghĩa tham số rõ ràng: Các tham số được định nghĩa với kiểu, mô tả và thường bao gồm các giá trị mặc định và pattern được phép. Các tham số được xác thực bằng allowedPattern hoặc allowedValues để đảm bảo đầu vào đúng.

```json
"IISAppPoolName": {
    "type": "String",
    "default": "DefaultAppPool",
    "description": "Name of the Windows IIS Application Pool to Stop",
    "allowedPattern": "^[a-zA-Z0-9]{1,50}$"
}
```

* Tận dụng biến môi trường cho cấu hình động: Luôn sử dụng các biến môi trường tích hợp sẵn của SSM (như AWS_SSM_REGION_NAME) thay vì hardcode các giá trị trong script của bạn. Thực tiễn này đảm bảo tự động hóa của bạn vẫn có thể di chuyển được trên các region và tài khoản khác nhau, giảm nguy cơ lỗi từ cập nhật thủ công, và làm cho tài liệu của bạn dễ bảo trì hơn. Ví dụ, thay vì nhúng các endpoint hoặc đường dẫn cụ thể theo region, hãy tham chiếu AWS_SSM_REGION_NAME để tự động điều chỉnh script của bạn theo bất kỳ region nào chúng đang chạy. Bạn có thể xem các biến trong phiên của mình bằng cách chạy AWS Run Command cho tài liệu AWS-RunShellScript và truyền lệnh thích hợp cho hệ điều hành của bạn. Ví dụ, truyền printenv như một lệnh để xem biến môi trường trên Linux.

* Phát hiện hệ điều hành: Triển khai phát hiện hệ điều hành để cho phép tự động hóa đa nền tảng. Bằng cách xác định loại và phiên bản hệ điều hành trước khi thực thi bất kỳ lệnh nào, bạn có thể xử lý đúng các biến thể trên các hệ thống khác nhau. Pattern phát hiện-rồi-thực thi này cho phép tự động hóa của bạn tự động chọn trình quản lý gói thích hợp (chẳng hạn như yum cho Amazon Linux/CentOS hoặc apt cho Ubuntu), định vị tệp hệ thống một cách chính xác, và sử dụng các công cụ dòng lệnh phù hợp cho từng nền tảng. Cách tiếp cận này đảm bảo tự động hóa của bạn vẫn đáng tin cậy và nhất quán cho dù bạn đang quản lý môi trường đồng nhất hay một đội tàu đa dạng các bản phân phối Linux.

### **Phát hiện Amazon Linux**

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

### **Phát hiện CentOS/RHEL**

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

* Điều kiện tiên quyết: Tạo tự động hóa thống nhất, thông minh bằng cách sử dụng điều kiện tiên quyết trong tài liệu SSM của bạn. Thay vì duy trì các tài liệu Windows và Linux riêng biệt, hãy viết một tài liệu duy nhất tự động thực thi các lệnh phù hợp cho từng nền tảng. Ví dụ, tài liệu của bạn có thể sử dụng điều kiện tiên quyết 'platformType' để chạy các lệnh PowerShell trên các server Windows trong khi thực thi script bash trên các instance Linux. Bạn cũng có thể kiểm soát logic workflow bằng cách sử dụng điều kiện tiên quyết để kiểm tra hoàn thành bước—như xác minh các phụ thuộc được cài đặt trước khi tiến hành các bước cấu hình. Điều này được thực hiện bằng cách đặt điều kiện tiên quyết kiểm tra xem bước trước có hoàn thành thành công hay không, ví dụ, 'StringEquals: InstallDependencies: True'. Cách tiếp cận này đảm bảo rằng các bước thực thi theo thứ tự chính xác và chỉ khi các yêu cầu trước đó được đáp ứng, trong khi vẫn duy trì trí thông minh cụ thể theo nền tảng. Kết quả là tự động hóa đáng tin cậy hơn hoạt động trên toàn bộ cơ sở hạ tầng của bạn, với xác thực tích hợp sẵn giữa các bước.

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

* OnFailure: Xử lý luồng thực thi trong tài liệu bằng cách tận dụng trường OnFailure. OnFailure hỗ trợ hai giá trị 'exit' hoặc 'successAndExit'. Cả hai đều ngay lập tức dừng xử lý bất kỳ bước nào chưa được định nghĩa là 'finallyStep'. Sự khác biệt giữa hai cái là 'exit' sẽ trả về trạng thái thất bại, nhưng successAndExit sẽ trả về thành công. Vì chúng ta đang chạy những thứ này trong ngữ cảnh của FIS, chúng ta muốn lỗi cũng làm thất bại thí nghiệm. OnFailure: exit cũng là một pattern tốt để thêm vào các điều kiện tiên quyết của bạn để đảm bảo mục tiêu sẵn sàng chạy thí nghiệm. Đây là cách ví dụ trước đó trông như thế nào với onFailure được thêm vào.

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

## Thực tiễn tốt nhất về mã

Bây giờ chúng ta sẽ xem xét những gì chúng ta có thể làm trong mã SSM FIS để đảm bảo các thực tiễn tốt nhất ở tất cả các lớp của tự động hóa. Để biết thêm các thực tiễn tốt nhất về SSM Run Command, tham khảo [các trường hợp sử dụng và thực tiễn tốt nhất.](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-best-practices.html)

* Tính idempotency: Triển khai tính idempotency trong script của bạn để đảm bảo độ tin cậy và ngăn ngừa các tác dụng phụ không mong muốn trong quá trình thử lại hoặc thực thi lặp lại. Điều này có nghĩa là script của bạn phải kiểm tra kỹ lưỡng trạng thái hiện tại của hệ thống trước khi thực hiện bất kỳ thay đổi nào và chỉ thực hiện các hành động khi cần thiết. Ví dụ, trước khi cài đặt phần mềm, hãy xác minh xem nó đã được cài đặt chưa và ở phiên bản chính xác; trước khi sửa đổi cấu hình, hãy xác nhận chúng chưa ở trạng thái mong muốn. Thiết kế script của bạn để xử lý các lần thực thi bị gián đoạn một cách graceful bằng cách triển khai các cơ chế theo dõi trạng thái (chẳng hạn như tệp trạng thái hoặc mục SSM Parameter Store) ghi lại tiến độ và cho phép các lần chạy tiếp theo tiếp tục từ nơi đã dừng lại.

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

* Timeouts: Đặt timeout thích hợp trong các bước tự động hóa của bạn để thực thi an toàn hoạt động và cho phép xử lý lỗi phù hợp. Mỗi bước nên bao gồm tham số 'timeoutSeconds' phản ánh cả thời gian thực thi dự kiến và khả năng chịu đựng của bạn đối với việc hoàn thành bị trễ. Timeout này hoạt động như một cơ chế an toàn quan trọng—khi một bước vượt quá timeout của nó, Systems Manager sẽ chấm dứt việc thực thi và kích hoạt các quy trình dọn dẹp. Cách tiếp cận này đảm bảo tự động hóa của bạn không để hệ thống ở trạng thái không xác định trong quá trình lỗi. Ví dụ, nếu một bước cài đặt phần mềm timeout, script của bạn nên bao gồm logic dọn dẹp để rollback bất kỳ thay đổi một phần nào, chẳng hạn như loại bỏ các cài đặt không hoàn chỉnh hoặc khôi phục cấu hình gốc. Bằng cách kết hợp timeout với các quy trình dọn dẹp phù hợp, bạn duy trì tính toàn vẹn của hệ thống ngay cả khi cấu trúc kiểm soát xung quanh script của bạn bị lỗi hoặc không phản hồi. Ví dụ từ AWSFIS-Run-CPU-Stress.

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

* Logging: Script nên bao gồm các câu lệnh echo để ghi lại tiến độ và kết quả của tài liệu Run command SSM. Đầu ra lệnh có thể được xem theo nhiều cách và hữu ích để debug tài liệu cũng như ghi lại các thông báo thành công trong suốt thời gian thực thi. Các tùy chọn đầu ra có thể được cấu hình như một phần của Run Command, logging có thể được xem bằng cách điều hướng đến lịch sử lệnh trong SSM, hoặc thông qua bucket [Amazon Simple Storage Service (Amazon S3)](https://aws.amazon.com/s3/) hoặc log [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/). Lưu ý rằng [các bước cấu hình bổ sung](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-rc-setting-up-cwlogs.html) được yêu cầu để thiết lập logging bucket S3 và/hoặc log CloudWatch. Ngoài ra, logging có thể được chuyển hướng đến vị trí /tmp của hệ điều hành cục bộ có thể có giá trị trong các kịch bản phức tạp hơn.

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

Các tùy chọn đầu ra có thể được cấu hình trong AWS Systems Manager > Node Tools > Run Command

![ picture](/images/3-BlogsTranslated/image7.png)
Hình 2: Tùy chọn đầu ra

Đầu ra run command có thể được xem sau khi một job được chạy bằng cách điều hướng đến "Command history" và chọn "View output"

![ picture](/images/3-BlogsTranslated/image8.png)
Hình 3: – Đầu ra từ run command

* Quy trình dọn dẹp: Vì tính idempotency là một đặc tính mong muốn, chúng ta phải dọn dẹp sau khi hoàn thành sự suy giảm để cho phép thí nghiệm được chạy lại. Triển khai các quy trình dọn dẹp để loại bỏ các tệp tạm thời, khôi phục cấu hình dịch vụ và đặt lại trạng thái hệ thống về điều kiện ban đầu của chúng. Các quy trình dọn dẹp của bạn nên được kỹ lưỡng như các quy trình thiết lập của bạn, đảm bảo rằng mỗi thí nghiệm thực sự bắt đầu từ một slate sạch. Điều này bao gồm loại bỏ bất kỳ tệp tạm thời nào, khôi phục các tệp cấu hình gốc, xóa dữ liệu được cache và xác minh trạng thái dịch vụ. Luôn triển khai xử lý lỗi và logging thích hợp trong quá trình dọn dẹp, vì các quy trình dọn dẹp thất bại có thể ảnh hưởng đến tính idempotency của các lần chạy thí nghiệm tương lai.

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

* Khôi phục trạng thái dịch vụ: Thí nghiệm FIS của bạn sẽ gây căng thẳng cho một ứng dụng trong môi trường thử nghiệm hoặc sản xuất bằng cách tạo ra các sự kiện gián đoạn. Khi kiểm thử hỗn loạn trưởng thành vào môi trường sản xuất, việc triển khai các kiểm tra để đảm bảo dịch vụ được khôi phục đúng cách là rất quan trọng.

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

* Quản lý thời lượng: Rollback đúng cách bao gồm quản lý thời lượng thí nghiệm và đảm bảo khôi phục kịp thời. Khi các thực tiễn kỹ thuật hỗn loạn phát triển, thời gian của thí nghiệm sẽ trở nên chính xác hơn. Các yếu tố khác có thể có tác dụng như một phần của thí nghiệm tùy thuộc vào kiến trúc được kiểm thử; ví dụ, các phương pháp mở rộng động có thể được áp dụng cho Auto Scaling Groups. Tham khảo [lập kế hoạch thí nghiệm AWS FIS của bạn](https://docs.aws.amazon.com/fis/latest/userguide/getting-started-planning.html) để biết thêm chi tiết.

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

* Xử lý lỗi trong quá trình rollback: Triển khai xử lý lỗi toàn diện trong các hoạt động rollback. Như một phần của thí nghiệm lỗi, các bài kiểm tra nên bao gồm xử lý lỗi chi tiết để hỗ trợ debug và đơn giản hóa hành trình Kỹ thuật Hỗn loạn đang tiếp diễn. Các nhóm nên có cách tiếp cận liên tục đối với khả năng phục hồi trong đám mây, phản ứng và học hỏi dựa trên thành công và thất bại của các thí nghiệm trước đó. Tham khảo [Khung vòng đời khả năng phục hồi](https://docs.aws.amazon.com/prescriptive-guidance/latest/resilience-lifecycle-framework/introduction.html) để biết thêm hướng dẫn về các thực tiễn tốt nhất về khả năng phục hồi và cải tiến liên tục.

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

## Kết luận

Systems Manager và Fault Injection Service cung cấp một nền tảng mạnh mẽ để triển khai các nguyên tắc kỹ thuật hỗn loạn và cải thiện khả năng phục hồi của các workload AWS của bạn. Bằng cách tuân theo các thực tiễn tốt nhất được nêu trong blog này, bạn có thể tạo ra các tài liệu SSM mạnh mẽ, hiệu quả và dễ bảo trì hơn để sử dụng với FIS. Các điểm chính bao gồm triển khai cấu trúc mô-đun, quản lý tham số phù hợp, phát hiện hệ điều hành, xử lý lỗi và logging toàn diện, cơ chế rollback mạnh mẽ, quản lý timeout hiệu quả và xử lý tín hiệu phù hợp. Ngoài ra, việc sử dụng điều kiện tiên quyết, tuân thủ [các thực tiễn tốt nhất về bảo mật](https://docs.aws.amazon.com/fis/latest/userguide/security.html), và kiểm thử kỹ lưỡng là rất quan trọng để tạo ra các thí nghiệm kỹ thuật hỗn loạn hiệu quả.

Khi bạn tiếp tục tận dụng SSM và FIS trong môi trường AWS của mình, hãy nhớ rằng kỹ thuật hỗn loạn là một quá trình đang tiếp diễn. Liên tục tinh chỉnh cách tiếp cận của bạn dựa trên những hiểu biết thu được từ mỗi thí nghiệm và cập nhật với các tính năng và thực tiễn tốt nhất mới nhất như được ghi chú trong [Khung Vòng đời Khả năng Phục hồi AWS](https://docs.aws.amazon.com/prescriptive-guidance/latest/resilience-lifecycle-framework/introduction.html). Ngoài ra, đối với những người mới bắt đầu với FIS, [Workshop Kỹ thuật Hỗn loạn](https://catalog.us-east-1.prod.workshops.aws/workshops/eb89c4d5-7c9a-40e0-b0bc-1cde2df1cb97/en-US) cung cấp một môi trường an toàn để học và thực hành các thí nghiệm. Bằng cách thành thạo việc tích hợp Systems Manager với FIS và triển khai những thực tiễn tốt nhất này, bạn đang thực hiện một bước quan trọng hướng tới việc xây dựng các hệ thống có khả năng phục hồi hơn, chịu lỗi có thể chịu đựng được bản chất không thể đoán trước của môi trường sản xuất. Điều này không chỉ cải thiện các thí nghiệm tiêm lỗi hiện tại của bạn mà còn đặt nền tảng vững chắc cho các sáng kiến kỹ thuật hỗn loạn tương lai, cuối cùng dẫn đến các triển khai AWS đáng tin cậy và mạnh mẽ hơn.