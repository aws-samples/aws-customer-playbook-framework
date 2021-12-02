# 事件响应 Playbook：未经授权的网络更改
本文档仅供参考。 它代表了截至本文档发布之日 Amazon Web Services (AWS) 提供的当前产品和实践，这些产品和做法如有更改，恕不另行通知。 客户有责任对本文档中的信息以及对 AWS 产品或服务的任何使用情况进行自己的独立评估，每种产品或服务都是 “按原样” 提供的，无论是明示还是暗示的担保。 本文档不创建 AWS、其附属公司、供应商或许可方的任何担保、陈述、合同承诺、条件或保证。 AWS 对客户的责任和责任受 AWS 协议的控制，本文档既不是 AWS 与其客户之间的任何协议的一部分，也不修改。

© 2021 Amazon 网络服务公司或其附属公司。 保留所有权利。 本作品根据知识共享署名 4.0 国际许可协议进行许可。

提供此 AWS 内容须遵守 http://aws.amazon.com/agreement 提供的 AWS 客户协议的条款或客户与 Amazon Web Services, Inc. 或 Amazon 网络服务 EMEA SARL 或两者之间的其他书面协议。

## 联系点

作者：`作者姓名`
批准者：`批准者姓名`
最后批准日期：

## 执行摘要
本行动手册概述了应对 AWS 环境中对网络资产的未授权或可疑更改的过程。

## 潜在的妥协指标
-新的或无法识别的服务资源
-无法识别或未经授权的资源（例如 EC2、Lambda）
-不寻常的账单增加
-来自安全研究员的通知
-通知我的 AWS 资源或账户可能会被盗用

## 潜在的 AWS GuardDuty 调查结果
-Backdoor: EC2/C&CActivity.B
-Backdoor: EC2/C&CActivity.B！ DNS
-Backdoor: EC2/Spambot
-Behavior: EC2/NetworkPortUnusual
-Behavior: EC2/TrafficVolumeUnusual
-CryptoCurrency: EC2/BitcoinTool.B
-CryptoCurrency: EC2/BitcoinTool.B! DNS
-Impact: EC2/AbusedDomainRequest.Reputation
-Impact: EC2/BitcoinDomainRequest.Reputation
-Impact: EC2/MaliciousDomainRequest.Reputation
-Impact: EC2/SuspiciousDomainRequest.Reputation
-Impact: EC2/WinRMBruteForce
-RERecon: EC2/PortProbeEMRUnprotectedPort
-RERecon: EC2/PortProbeUnprotectedPort
-Trojan: EC2/BlackholeTraffic
-Trojan: EC2/BlackholeTraffic！ DNS
-Trojan: EC2/DGADomainRequest.B
-特洛伊木马:EC2/dgadomaMain请求.c! DNS
-Trojan: EC2/DNSDataExfiltration
-特洛伊木马：EC2/驱动BySourcRaffic！ DNS
-Trojan: EC2/DropPoint
-Trojan: EC2/DropPoint！ DNS
-特洛伊木马：EC2/网络网络网页请求！ DNS
-UnauthorizedAccess: EC2/MaliciousIPCaller.Custom
-UnauthorizedAccess: EC2/MetadataDNSRebind
-UnauthorizedAccess: EC2/RDPBruteForce
-UnauthorizedAccess: EC2/SSHBruteForce
-UnauthorizedAccess: EC2/TorClient
-UnauthorizedAccess: EC2/TorRelay
-Discovery: S3/MaliciousIPCaller
-CredentialAccess: IAMUser/AnomalousBehavior
-DefenseEvasion: IAMUser/AnomalousBehavior
-Discovery: IAMUser/AnomalousBehavior
-Exfiltration: IAMUser/AnomalousBehavior
-Impact: IAMUser/AnomalousBehavior
-InitialAccess: IAMUser/AnomalousBehavior
-Persistence: IAMUser/AnomalousBehavior
-Policy: IAMUser/RootCredentialUsage
-PrivilegeEscalation: IAMUser/AnomalousBehavior
-RERecon: IAMUser/MaliciousIPCaller
-RERecon: IAMUser/MaliciousIPCaller.Custom
-RECORecon: IAMUser/TorIPCaller
-Stealth: IAMUser/CloudTrailLoggingDisabled
-Stealth: IAMUser/PasswordPolicyChange
-UnauthorizedAccess: IAMUser/ConsoleLoginSuccess.B
-UnauthorizedAccess: IAMUser/InstanceCredentialExfiltration se
-UnauthorizedAccess: IAMUser/MaliciousIPCaller
-UnauthorizedAccess: IAMUser/MaliciousIPCaller.Custom
-UnauthorizedAccess: IAMUser/TorIPCaller

### 目标
在整个行动手册的执行过程中，重点关注 _*** 预期结果 ***_，记下增强事件响应能力的注意事件。

#### 确定：
* ** 漏洞被利用 **
* ** 观察到的漏洞和工具 **
* ** 演员的意图 **
* ** 演员的归因 **
* ** 对环境和业务造成的损害 **

#### 恢复：
* ** 返回原始和强化配置 **

#### 增强 CAF 安全视角组件：
[AWS Cloud Adoption Framework 安全角度] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* ** 指令 **
* ** 侦探 **
* ** 响应 **
* ** 预防性 **

! [图片] (/images/aws_caf.png)
* * *

### 响应步骤
1. [** 准备 **] 公开披露的资产库存
2. [** 准备 **] 安装日志记录
3. [** 准备 **] 实施培训计划来识别和评估云取证能力
4. [** 准备 **] 识别、记录和测试上报程序
5. [** 检测和分析 **] 查看 CloudTrail
6. [** 检测和分析 **] 查看 CloudWatch
7. [** 检测和分析 **] 使用 AWS Config 检查安全组的配置
8. [** 检测和分析 **] 在控制台中查看 VPC Flow Logs
9. [** 检测和分析 **] 使用 Amazon Athena 查询流日志
10. [** CONTANMENT**] 确定哪个用户或角色执行了未经授权的网络更改
11. [**ERADICATION**] 查看来自被泄露的访问密钥查看 CloudTrail 事件历史记录中的调查结果以了解活动
12. [** 根除 **] 查看避免意外费用
13. [**RECOVERY**] 还原对网络资源发生的任何更改
14. [** 准备 **] 其他预防措施：AWS Config 和 AWS 系统管理器
15. [** 准备 **] 其他预防措施：AWS Security Hub
16. [** 准备 **] 其他预防措施：总体安全状况

*** 响应步骤遵循 NIST 特别出版物 800-61r2 的事件响应生命周期
[NIST 计算机安全事件处理指南] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)
! [图片] (images/nist_life_cycle.png) ***

### 事件分类和处理
* ** 策略、技术和程序 **：工具：取证
* ** 类别 **：取证
* ** 资源 **：EC2、VPC
* ** 指标 **：网络威胁情报、第三方通知、Cloudwatch 指标
* ** 日志源 **：端点
* ** 团队 **：安全运营中心 (SOC)、法医调查员、云工程

## 事件处理流程
### 事件响应流程有以下几个阶段：
* 准备
* 检测和分析
* 遏制和根除
* 恢复
* 事件后活动

## 准备

本行动手册在可能的情况下引用并集成了 [Prowler] (https://github.com/toniblyx/prowler)，这是一种命令行工具，可以帮助您进行 AWS 安全评估、审计、强化和事件响应。

它遵循 CIS Amazon Web Services Foundations Benchmark 指南（49 个支票），并有 100 多项额外支票，包括与 GDPR、HIPAA、PCI-DSS、ISO-27001、FFIEC、SOC2 和其他相关的支票。

此工具提供了客户环境中当前安全状态的快速快照。 此外，[AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) 提供了自动合规性扫描并可以 [与 Prowler 集成] (https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### 公开资产库存
您可以使用 [VPC 控制面板] (https://console.aws.amazon.com/vpc/home) 和 [网络管理器控制面板] (https://console.aws.amazon.com/networkmanager/home) 使您能够可视化和监控网络资源。 网络管理器控制面板包括有关全球网络中的资源、其地理位置、网络拓扑、Amazon CloudWatch 指标和事件的信息，并使您能够执行路由分析。

您应该维护以下资源的清单并监控对以下资源的更改：
-虚拟私有云 (VPC)
-VPC 网络访问控制列表 (NACL)
-VPC 安全组
-VPC 路由表
-VPC 弹性网络接口 (ENI)
-VPC 互联网网关
-VPC 对等连接
-VPC NAT 网关
-VPC 终端节点
-VPN 客户网关
-VPC 弹性 IP 地址 (EIP)

Prowler 还可以帮助你找到许多暴露在互联网上的资源：`。 /prowler-g group17`

### 安装日志记录
** 流程日志：**
-流日志捕获有关进出 VPC 中网络接口的 IP 流量的信息。
-您可以为 VPC、子网或单个网络接口创建流日志。
-流日志数据发布到 CloudWatch Logs 或 Amazon S3，可以帮助您诊断过于限制或过于宽容的安全组和网络 ACL 规则。

** 使用控制台为网络接口创建流日志 **
1. 打开 Amazon [EC2 控制台]（https://console.aws.amazon.com/ec2/）
1. 在导航窗格中，选择网络接口。
1. 选中一个或多个网络接口的复选框，然后选择操作、Create flow log。
1. 对于 Filter，指定要记录的流量类型。
-选择全部记录接受和拒绝的流量，选择拒绝仅记录拒绝的流量，或者选择接受仅记录接受的流量。
1. 对于最长聚合间隔，请选择捕获流并聚合到一个流日志记录中的最长时间段。
1. 对于目标，请选择 Send to an Amazon S3 bucket。
1. 对于 S3 bucket ARN，请指定现有 Amazon S3 存储桶的 Amazon 资源名称 (ARN)。
-您可以在存储桶 ARN 中包含子文件夹。 存储桶不能使用 AWSLogs 作为子文件夹名称，因为这是一个预留期限。
-例如，要在名为 my-bucket 的存储桶中指定名为 my-log 的子文件夹，请使用以下 ARN：`arn: aw: s3።: my-bucket/my-logs/`
-如果您拥有存储桶，我们会自动创建资源策略并将其附加到存储桶。 有关更多信息，请参阅针对流日志的 Amazon S3 存储桶权限。
1. 对于日志记录格式，请选择流日志记录的格式。
* 要使用默认格式，请选择 AWS default format。
* 要使用自定义格式，请选择自定义格式，然后从日志格式中选择字段。
1. 选择 Create flow log。

** 使用控制台为 VPC 或子网创建流日志 **
1. 打开 Amazon [VPC 控制台]（https://console.aws.amazon.com/vpc/）
1. 在导航窗格中，选择您的 VPC 或选择 Subnets。
1. 选中一个或多个 VPC 或子网的复选框，然后选择操作、Create flow log。
1. 对于 Filter，指定要记录的流量类型。
-选择全部记录接受和拒绝的流量，选择拒绝仅记录拒绝的流量，或者选择接受仅记录接受的流量。
1. 对于最长聚合间隔，请选择捕获流并聚合到一个流日志记录中的最长时间段。
1. 对于目标，请选择 Send to an Amazon S3 bucket。
1. 对于 S3 bucket ARN，请指定现有 Amazon S3 存储桶的 Amazon 资源名称 (ARN)。
-您可以在存储桶 ARN 中包含子文件夹。 存储桶不能使用 AWSLogs 作为子文件夹名称，因为这是一个预留期限。
-例如，要在名为 my-bucket 的存储桶中指定名为 my-log 的子文件夹，请使用以下 ARN：`arn: aw: s3።: my-bucket/my-logs/`
-如果您拥有存储桶，我们会自动创建资源策略并将其附加到存储桶。 有关更多信息，请参阅针对流日志的 Amazon S3 存储桶权限。
1. 对于日志记录格式，请选择流日志记录的格式。
* 要使用默认格式，请选择 AWS default format。
* 要使用自定义格式，请选择自定义格式，然后从日志格式中选择字段。
1. 选择 Create flow log。

** 监控 NAT 网关：** 您可以使用 CloudWatch 监控 NAT 网关，CloudWatch 从 NAT 网关收集信息并创建可读、接近实时的指标。
1. NAT 网关指标每隔 1 分钟发送到 CloudWatch。
1. 您可以按如下方式查看 NAT 网关的指标。
* 在 https://console.aws.amazon.com/cloudwatch/ 打开 CloudWatch 控制台
* 在导航窗格中，选择量度。
* 在 “所有指标” 下，选择 NAT 网关指标命名空间。
* 要查看指标，请选择指标维度。
1. 要为通过 NAT 网关的出站流量创建警报：
* 在 https://console.aws.amazon.com/cloudwatch/ 打开 CloudWatch 控制台
* 在导航窗格中，选择警报，创建警报。
* 选择 NAT 网关。
* 选择 NAT 网关和 BytesOutToDestination 指标，然后选择下一步。
* 按如下方式配置警报，然后在完成后选择创建警报：
* 在警报阈值下，输入警报的名称和描述。 对于任何时候，选择 >= 然后输入 5000000。 连续时间输入 1。
* 在 “操作” 下，选择现有的通知列表或选择 “新建列表” 以创建新列表。
* 在警报预览下，选择 15 分钟的时间段并指定 Sum 的统计数据。
1. 创建警报以监控端口分配错误
* 在 https://console.aws.amazon.com/cloudwatch/ 打开 CloudWatch 控制台
* 在导航窗格中，选择警报，创建警报。
* 选择 NAT 网关。
* 选择 NAT 网关和 ErrorPortAllocation 指标，然后选择下一步。
* 按如下方式配置警报，然后在完成后选择创建警报：
* 在警报阈值下，输入警报的名称和描述。 对于任何时候，选择 > 然后输入 0。 连续时间输入 3。
* 在 “操作” 下，选择现有的通知列表或选择 “新建列表” 以创建新列表。
* 在 “警报预览” 下，选择 5 分钟的时间段并指定统计数据 “最大值”。

### 训练
-“为了熟悉 AWS API（命令行环境）、S3、RDS 和其他 AWS 服务，公司内的分析师提供了哪些培训？ `
>>>
威胁检测和事件响应的机会包括：\
[AWS RE: INFORCE] (https://reinforce.awsevents.com/faq/)\
[Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

-“哪些角色能够对账户中的服务进行更改？ `
-“哪些用户分配了这些角色？ 遵守的权限最少吗？还是存在超级管理员用户？ `
-“是否针对您的环境进行了安全评估，您是否有已知的基准来检测 “新” 或 “可疑” 事物？ `

### 通信技术
-“团队/公司内部使用什么技术来沟通问题？ 有什么自动化吗？ `
>>>
电话\
电子邮件\
AWS SES\
AWS SNS\
Slack\
Chime\
其他？
>>>

## 检测

### CloudTrail
1. 导航到你的 [CloudTrail 控制面板]（https://console.aws.amazon.com/cloudtrail）
1. 在左边栏中选择 “事件历史记录”
1. 在下拉菜单中，从 “只读” 更改为 “事件名称”
1. 查看 CloudTrail 日志中的事件名称 “授权安全组 inGress`、“吊销安全组 inGress`、“授权 SecurityGroupegress” 和 “吊销 SecurityGrouppegress”

### CloudWatch
使用 CloudWatch 控制台创建在事件上触发的 CloudWatch Event 规则

1. 打开 CloudWatch 控制台。
1. 在导航窗格中，选择 “事件” 下的 “规则”，然后选择 “创建规则”。
1. 选择事件模式。
1. 对于服务名称，选择 EC2。
1. 对于事件类型，请选择通过 CloudTrail 调用 AWS API。
1. 选择特定操作并提供以下 API 调用。 这些 API 调用用于添加或删除安全组规则。
-AuthorizeSecurityGroupIngress
-AuthorizeSecurityGroupEgress
-RevokeSecurityGroupIngress
-RevokeSecurityGroupEgress
1. 这些设置创建以下事件模式。
``json
{
“来源”：[
“aws.ec2"
]，
“详细类型”：[
“通过 CloudTrail 调用 AWS API”
]，
“详细信息”：{
“事件来源”：[
“ec2.amazonaws.com”
]，
“事件名称”: [
“AuthorizeSecurityGroupIngress”,
“AuthorizeSecurityGroupEgress”，
“RevokeSecurityGroupIngress”,
“RevokeSecurityGroupEgress”
]
}
}
``
1. 选择添加目标。
1. 在目标列表中，选择 SNS 主题。
1. 对于 Topic，请选择创建新主题的现有主题。
1. 选择配置详细信息。
1. 在配置规则详细信息页面上，输入名称和可选说明。
-对于 “状态”，请将启用框保持选中状态。
1. 选择创建规则。

### 使用 AWS Config 检查安全组的配置
计划使用 AWS Config 和 AWS Config Rules 时，首先确定是主动或主动使用该服务。
-要被动地使用 AWS Config，只需启用该服务，它将开始创建 AWS 资源清单和更改历史记录。
-要使用 AWS Config 进行主动更改通知，请执行以下任务：
1. 创建 Amazon SNS 主题以接收配置更改通知。
1. 开发 AWS Lambda 函数或基于 EC2 的 SQS 消息处理器，以识别特定于网络的更改并发送适当的通知。
1. 考虑至少有两个不同的收件人获得更改通知：
-网络安全电子邮件或寻呼通讯组列表，用于接收需要立即人为干预的通知（例如，修改安全组以 allow all 全局访问或将 Internet 网关附加到仅限内部的 VPC）。
-一个中央日志记录或变更管理系统，可用于协调批准的更改请求与实际配置更改。
1. 为您的 AWS Lambda 函数或 SQS 队列订阅更改通知 SNS 主题。
1. 配置 AWS Config 以将更改通知发布到更改通知 SNS 主题。

以下是将 AWS Config 与 Lambda 函数结合使用的示例：

1. 创建 [Lambda 执行角色] (http://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#lambda-intro-execution-role) 和 [AWS Config 角色] (http://docs.aws.amazon.com/config/latest/developerguide/iamrole-permissions.html)。
1. 启用 AWS Config 记录安全组的配置，以便 AWS Config 可以监控安全组的配置更改。 以下屏幕截图显示了设置的示例。
显示配置设置示例的屏幕截图
1. [创建安全组]（http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html），启用与您的业务需求相关的端口（例如 HTTP 为 80/443，SMTP 为 25 个）
1. 使用 [来自 GitHub 上 AWS Config 规则库的代码] 创建 Python Lambda 函数（https://github.com/awslabs/aws-config-rules/blob/master/python/ec2_security_group_ingress.py）。
-Lambda 函数代码定义了一个名为 REQUARD_PERMENTS 的列表，其中包含代表协议、端口范围和 IP 范围的元素，这些元素共同定义了安全权限。 在这种情况下，只授权端口 80 和 443。
-使用业务需求所需的端口和 IP 范围自定义此 Lambda 函数。
1. [创建 AWS Config 规则] (https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_develop-rules_nodejs.html#creating-a-custom-rule-with-the-AWS-Config-console) 使用您在步骤 4 中创建的 Lambda 函数。
-对于触发器类型，选择配置更改。
-对于更改范围，请选择 EC2：SecurityGroup，然后键入您在步骤 3 中创建的安全组的 ID。
1. 运行配置规则。 这将排队执行规则，规则应在大约 10 分钟后运行到完成。
1. 检查您在步骤 3 中创建的安全组。

## 上报程序
-“谁在监控日志/警报，接收日志/警报并对其采取行动？ `
-“发现警报后谁会收到通知？ `
-“什么时候公共关系和法律参与这一过程？ `
-“你什么时候联系 AWS Support 寻求帮助？ `

## 分析
### 在控制台中查看 VPC Flow Logs
** 查看网络接口的流日志信息 **
1. 打开 Amazon [EC2 控制台]（https://console.aws.amazon.com/ec2/）
1. 在导航窗格中，选择网络接口。
1. 选择网络接口，然后选择 Flow Logs。 有关流日志的信息显示在选项卡上。 “目标类型” 列指示将流日志发布到的目标。

** 查看 VPC 或子网的流日志信息 **
1. 在 https://console.aws.amazon.com/vpc/ 打开 Amazon VPC 控制台
1. 在导航窗格中，选择您的 VPC 或 Subnets。
1. 选择您的 VPC 或子网，然后选择流日志。
-有关流程日志的信息显示在选项卡上。
-“目标类型” 列指示流日志发布到的目标。

** 查看发布到亚马逊 S3 的流日志记录 **
1. 在 https://console.aws.amazon.com/s3/ 打开 Amazon S3 控制台
1. 对于存储桶名称，选择将流日志发布到的存储桶。
1. 对于名称，选中日志文件旁边的复选框。 在对象概述面板上，选择下载。

### 使用 Amazon Athena 查询流日志
Amazon Athena 是一项交互式查询服务，使您能够使用标准 SQL 分析 Amazon S3 中的数据，例如流日志。 您可以将 Athena 与 VPC Flow Logs 结合使用，以快速获得有关通过 VPC 的流量的可操作见解。 例如，您可以确定虚拟私有云 (VPC) 中的哪些资源是排名第一的人，或者识别 TCP 连接被拒绝最多的 IP 地址。

您可以通过生成 CloudFormation 模板来简化和自动化 VPC 流日志与 Athena 的集成，该模板创建所需的 AWS 资源和预定义查询，您可以运行这些查询来获取有关通过 VPC 的流量的见解。

1. CloudFormation 模板创建以下资源：
-Athena 数据库。 数据库名称是数据 vpcflowlogsathenadatabase <flow-logs-subscription-id><flow-logs-subscription-id>。
-Athena 工作组。 <flow-log-subscription-id><partition-load-frequency><start-date><end-date><flow-log-subscription-id><partition-load-frequency><start-date><end-date>workgroup 名称是工作组
-与流日志记录对应的分区 Athena 表。 表名是<flow-log-subscription-id><partition-load-frequency><start-date><end-date>。
-一组 Athena 命名查询。 有关详细信息，请参阅预定义查询。
-一个 Lambda 函数，它按照指定的时间表（每天、每周或每月）将新分区加载到表中。
-授予运行 Lambda 函数的权限的 IAM 角色。
1. 要求
-您必须选择支持 AWS Lambda 和 Amazon Athena 的区域。
-Amazon S3 存储桶必须位于选定的区域。
1. 定价
-运行查询时，您需要支付标准的 Amazon Athena 费用。
-对于按照定期计划加载新分区的 Lambda 函数（如果您指定分区加载频率但未指定开始和结束日期），您需要支付标准 AWS Lambda 费用。
1. 执行以下操作之一：
-打开 Amazon VPC 控制台。 在导航窗格中，选择您的 VPC，然后选择您的 VPC。
-打开 Amazon VPC 控制台。 在导航窗格中，选择 Subnets，然后选择您的子网。
-打开 Amazon EC2 控制台。 在导航窗格中，选择网络接口，然后选择您的网络接口。
1. 在 Flow log 选项卡上，选择发布到 Amazon S3 的流日志，然后选择操作、生成 Athena 集成。
1. 指定分区加载频率。
-如果选择 “无”，则必须使用过去的日期指定分区开始和结束日期。
-如果您选择每日、每周或每月，则分区开始和结束日期是可选的。
-如果您没有指定开始日期和结束日期，CloudFormation 模板将创建一个 Lambda 函数，该函数按重复计划加载新分区。
1. 为生成的模板选择或创建 S3 存储桶，为查询结果选择或创建 S3 存储桶。
1. 选择生成 Athena 集成。
1. （可选）在成功消息中，选择链接以导航到您为 CloudFormation 模板指定的存储桶，然后自定义模板。
1. 在成功消息中，选择创建 CloudFormation 堆栈以在 AWS CloudFormation 控制台中打开创建堆栈向导。
-生成的 CloudFormation 模板的 URL 在模板部分中指定。
-完成向导以创建模板中指定的资源。

** 运行预定义的查询 **

生成的 CloudFormation 模板提供了一组预定义的查询，您可以运行这些查询，以便快速获得有关 AWS 网络中流量的有意义的见解。 创建堆栈并验证所有资源创建正确后，可以运行其中一个预定义的查询。

1. 使用控制台运行预定义查询
-打开 Athena 控制台。 在 “工作组” 面板中，选择由 CloudFormation 模板创建的工作组。
-选择其中一个预定义的查询，根据需要修改参数，然后运行查询。
-打开 Amazon S3 控制台。 导航到您为查询结果指定的存储桶，然后查看查询结果。

以下是生成的 CloudFormation 模板提供的 Athena 命名查询：
* VpcFlowLogsAcceptedTraffic uctions — 基于您的安全组和网络 ACL 允许的 TCP 连接。
* VpcFlowLogsAdminPortTraffic — 管理 Web 应用程序端口上记录的流量。
* VpcFlowLogsIPv4Traffic 量-记录的IPv4 流量的总字节数。
* VpcFlowLogsIPv6Traffic — 记录的 IPv6 流量的总字节数。
* VpcFlowLogsRejectedTCPTraffic — 根据您的安全组或网络 ACL 拒绝的 TCP 连接。
* VpcFlowLogsRejectedTraffic — 根据您的安全组或网络 ACL 拒绝的流量。
* VpcFlowLogsSshRdpTraffic — SSH 和 RDP 流量。
* VpcFlowLogsTopTalkers — 记录流量最多的 50 个 IP 地址。
* vpcFlowLogStoptalkerSPacketLevel — 记录流量最多的 50 个数据包级 IP 地址。
* VpcFlowLogsTopTalkingInstances — 记录流量最多的 50 个实例的 ID。
* VpcFlowLogsTopTalkingSubnets — 记录流量最多的 50 个子网的 ID。
* VpcFlowLogsTopTCPTraffic — 为源 IP 地址记录的所有 TCP 流量。
* VpcFlowLogsTotalBytesTransferred — 记录字节数最多的 50 对源和目标 IP 地址。
* VpcFlowLogsTotalBytesTransferredPacketLevel 别 — 记录字节数最多的 50 对数据包级源和目标 IP 地址。
* VpcFlowLogsTrafficFrmSrcAddr — 记录的特定源 IP 地址的流量。
* VpcFlowLogsTrafficToDstAddr — 为特定目标 IP 地址记录的流量

## 遏制
### 用户识别
确定哪个用户或角色执行未经授权的网络更改

以下是识别创建 EC2 实例背后的用户或配置文件的示例：

1. 从步骤 1 中识别可疑 EC2 实例的其中一个实例 ID
1. 打开 AWS 控制台
1. 然后选择服务，然 CloudTrail 选择
1. 在左边栏中，选择 “Event History 记录”
1. 通过选择 “false” 右侧的 “X” 来清除当前筛选器
1. 在最右边（自定义下）点击齿轮图标
1. 向下滚动并启用 “AWS 访问密钥”
1. 在中间将过滤器下拉菜单更改为 “资源名称”
1. 在搜索框中输入 EC2 实例 ID，然后按回车键
1. 你应该看到 RunInstances 事件；点击这个
1. 记录你发现的用户名。
-可能需要浏览多个事件来查找原始事件，例如从 CloudFormation 模板创建的事件
1. 返回 Event History 并将下拉菜单更改为 “用户名”，然后将上一步中的条目发布到搜索中，然后点击回车
1. 识别来自可疑或受损用户的任何其他事件
1. 现在让我们转到服务，IAM（屏幕左上角）
1. 在 IAM 资源下，选择用户
1. 查找我们在 CloudTrail 中识别的账户并选择它
1. 选择安全凭证
1. 向下滚动到访问密钥并选择 “x” 以删除密钥
1. 前往挑战仪表板，然后选择检查我的进度按钮

## 根除
### 查看来自 [查看 CloudTrail 事件历史记录以查看受损的访问密钥的活动] 的调查结果] (. /#cloudtrail)
删除受损密钥创建的所有资源。 务必检查所有 AWS 区域，甚至是您从未启动 AWS 资源的区域。
* ** 重要 **：如果您需要保留任何资源进行调查，请考虑对其进行备份。 例如，如果您有保留 EC2 实例的监管、合规性或法律需求，请在终止实例之前拍摄 EBS 快照。

### 查看 [避免意外收费] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/checklistforunwantedcharges.html)
检查并删除账户中识别的任何服务。 特别注意以下资源：
* EC2 实例和 AMI，包括处于停止状态的实例
* EBS 卷和快照
* AWS Lambda 函数和层

要删除 Lambda 函数和层，请执行以下操作：
1. 打开 Lambda 控制台。
1. 在导航窗格中，选择函数。
1. 选择要删除的函数。
1. 对于操作，请选择删除。
1. 在导航窗格中，选择图层。
1. 选择要删除的图层。
1. 选择删除。

## 恢复
还原对网络资源发生的任何更改

识别接触到互联网的资源并保护它们：`。 /prowler-g group17`

## 预防性行动
### AWS Config 和 AWS 系统管理器
[如何使用 AWS Config 和 AWS 系统管理器自动修复互联网访问端口] (https://aws.amazon.com/blogs/security/how-to-auto-remediate-internet-accessible-ports-with-aws-config-and-aws-system-manager/)

### AWS Security Hub
[启用新的 AWS 基础安全最佳实践安全标准] (https://aws.amazon.com/blogs/security/aws-foundational-security-best-practices-standard-now-available-security-hub/?secd_dir1)

### 整体安全态势
针对环境执行 [Self-Service Security Assessment]（https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/），以进一步识别本行动手册中未发现的其他风险和其他潜在的公众风险。

## 吸取的教训
“这是一个添加不需要 “修复” 但在与运营和业务要求同时执行本行动手册时必须了解的特定项目的地方。 `

## 已解决积压项目
-作为事件响应者，我需要能够监控所有关键的 VPC 环境
-作为事件响应者，我需要一种与业务团队、基础设施团队和安全团队协调的方法
-作为事件响应者，我需要一本关于大规模查询 VPC FlowLogs 的行动手册
-作为事件响应者，我需要确保在所有账户中正确使用 Config

## 当前积压项目