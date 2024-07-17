本文档仅供参考。 它代表了截至本文档发布之日 Amazon Web Services (AWS) 提供的当前产品和实践，这些产品和做法如有更改，恕不另行通知。 客户有责任对本文档中的信息以及对 AWS 产品或服务的任何使用情况进行自己的独立评估，每种产品或服务都是 “按原样” 提供的，无论是明示还是暗示的担保。 本文档不创建 AWS、其附属公司、供应商或许可方的任何担保、陈述、合同承诺、条件或保证。 AWS 对客户的责任和责任受 AWS 协议的控制，本文档既不是 AWS 与其客户之间的任何协议的一部分，也不修改。

© 2024 Amazon 网络服务公司或其附属公司。 保留所有权利。 本作品根据知识共享署名 4.0 国际许可协议进行许可。

提供此 AWS 内容须遵守 http://aws.amazon.com/agreement 提供的 AWS 客户协议的条款或客户与 Amazon Web Services, Inc. 或 Amazon 网络服务 EMEA SARL 或两者之间的其他书面协议。

## 联系点

作者：`作者姓名`
批准者：`批准者姓名`
最后批准日期：

## 执行摘要
本行动手册概述了识别代码资源所有者、确定他们的暴露方式、谁可能访问了公开的代码、确定暴露的影响、所需的补救措施以及确定代码暴露根本原因的过程。

## 潜在的妥协指标
-数据丢失防护 (DLP) 警报
-已知的代码发布或曝光
-可疑的 CloudTrail 日志

## 潜在的 AWS GuardDuty 调查结果
-Behavior: EC2/NetworkPortUnusual
-Behavior: EC2/TrafficVolumeUnusual
-CredentialAccess: IAMUser/AnomalousBehavior
-DefenseEvasion: IAMUser/AnomalousBehavior
-Discovery: IAMUser/AnomalousBehavior
-Exfiltration: IAMUser/AnomalousBehavior
-Exfiltration: S3/MaliciousIPCaller
-Exfiltration: S3/ObjectRead.Unusual
-Impact: IAMUser/AnomalousBehavior
-InitialAccess: IAMUser/AnomalousBehavior
-Persistence: IAMUser/AnomalousBehavior
-PrivilegeEscalation: IAMUser/AnomalousBehavior
-UnauthorizedAccess: IAMUser/InstanceCredentialExfiltration.InsideAWS
-UnauthorizedAccess: IAMUser/InstanceCredentialExfiltration.OutsideAWS

### 目标
在整个行动手册的执行过程中，重点关注 _*** 预期结果 ***_，记下增强事件响应能力的注意事件。

#### 确定：
* ** 代码副本、转移和出版 **
* ** 凭据曝光 **
* ** 漏洞暴露 **
* ** 环境情报曝光 **
* ** 使用的工具 **
* ** 配置信息 **
* ** 演员的意图 **
* ** 演员的归因 **
* ** 对环境和业务造成的其他损害 **
* ** 为环境和商业创造的风险 **

#### 恢复：
* ** 过期或重置任何公开的身份验证凭据 **
* ** 基于暴露的环境情报列举潜在攻击媒介的风险 **
* ** 尽量减少或消除暴露代码的风险 **

#### 增强 CAF 安全视角组件：
[AWS Cloud Adoption Framework 安全角度] (https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* ** 指令 **
* ** 侦探 **
* ** 响应 **
* ** 预防性 **

! [图片] (/images/aws_caf.png)
* * *

### 响应步骤
1. [** 准备 **] 创建账户资产库存
2. [** 准备 **] 创建仓库清单
3. [** 准备 **] 酌情启用日志记录
4. [** 准备 **] 确定每个存储库中的数据类型
5. [** 准备 **] 识别、记录和测试上报程序
6. [** 准备 **] 实施培训以解决泄漏攻击
7. [** 检测和分析 **] 执行渗透和 DLP 检查
8. [** 检测和分析 **] 查看存储库 (CodeCommit) 读取和写入操作
9. [** 检测和分析 **] 查看 DNS 日志
10. [** 检测和分析 **] 查看 VPC 流日志
11. [** 检测和分析 **] 查看端点/基于主机的日志
12. [** 包含 **] 阻止受影响账户的访问
13. [** 根除 **] 删除存储库中无法识别和未经授权的对象
14. [** RECOVERY**] 酌情执行恢复过程

*** 响应步骤遵循 [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] 中的事件响应生命周期（https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf）

! [图片] (/images/nist_life_cycle.png) ***

### 事件分类和处理
* ** 策略、技术和程序 **：数据泄露、AWS 服务滥用
* ** 类别 **：数据丢失
* ** 资源 **：CodeCommit、S3、EC2
* ** 指标 **：网络威胁情报、第三方通知、CloudWatch Metrics
* ** 日志源 **：DNS 日志、VPC Flow Logs、CloudTrail、CloudWatch
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

### 资产库存
确定所有现有资源，并拥有更新的资产库存清单以及每个资源的拥有者。 源代码可以存储在以下资产之一中：
-（CodeCommit）[https://aws.amazon.com/codecommit/] 存储库
-(S3) [https://aws.amazon.com/s3/] 支持的代码存储
-(EC2) [https://aws.amazon.com/ec2/] 自托管的代码存储机制

#### 识别从每个资产中渗出了哪些数据
-可以使用（Amazon Athena）[https://aws.amazon.com/athena/] 查询存储在 S3 中的 CloudWatch 和 CloudTrail 日志，以识别不需要的操作
-（Amazon GuardDuty）[https://aws.amazon.com/guardduty/] 可能会自动检测异常流量。 可以通过（AWS Security Hub）[https://aws.amazon.com/security-hub/] 或（Amazon 侦探）[https://aws.amazon.com/detective/] 访问此功能
-还可以使用 Athena 查询存储在 S3 中的应用程序和实例日志。

### 训练
-“为了熟悉 AWS API（命令行环境）、CodeCommit、S3、RDS 和其他 AWS 服务，公司内的分析师提供了哪些培训？ `
>>>
威胁检测和事件响应的机会包括：\
[AWS RE: INFORCE] (https://reinforce.awsevents.com/faq/)\
[Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

-“哪些角色能够更改账户中的服务？ `
-“哪些用户有这些角色分配给他们？ 遵守的权限最少吗？还是存在超级管理员用户？ `
-“是否针对您的环境进行了安全评估？ 你有一个已知的基线来检测 “新” 或 “可疑” 的东西吗？ `

### 通信技术
-“团队/公司内部使用什么技术来沟通问题？ 有什么自动化的吗？ `
>>>
电话\
电子邮件\
短信\
AWS SES\
AWS SNS\
Slack\
Chime\
团队\
其他？
>>>

### DLP 实施

实施数据丢失防护 (DLP) 解决方案可能会提供额外的检测功能和警报。 DLP 解决方案可能在 EC2 环境中或评估网络流量提供最大的价值。 可以在 [AWS 市场]（https://aws.amazon.com/marketplace/search/results?searchTerms=dlp）中找到 DLP 解决方案。

## 检测

### 日志记录和 S3 存储桶检查
-确保在所有地区都启用了 CloudTrail：`。 /prowler-c check21`
-确保启用 CloudTrail 日志文件验证：`。 /prowler-c check22`
-确保在 CloudTrail S3 存储桶上启用 S3 存储桶访问日志记录：`。 /prowler-c check26`
-确保没有向所有人或任何 AWS 用户开放的 S3 存储桶：`。 /prowler-c Extra73`
-确定组织和账户中与外部实体共享的资源（例如 Amazon S3 存储桶或 IAM 角色）中与外部实体共享的资源：`。 /prowler-c extra769`
-查找暴露在互联网上的资源：`。 /prowler-g group17`

### 日志记录和 CodeCommit 事件
-[在 EventBridge 中监控 AWS CodeCommit 事件] (https://docs.aws.amazon.com/codecommit/latest/userguide/monitoring-events.html)，它提供了实时数据流。 这些事件与 Amazon CloudWatch Event 中显示的事件相同，后者提供了几乎实时的系统事件流，描述 AWS 资源的变化。
-[创建 CloudTrail 跟踪以使 CodeCommit 事件持续传输到 S3 存储桶] (https://docs.aws.amazon.com/codecommit/latest/userguide/integ-cloudtrail.html)。 CloudTrail 将 CodeCommit 的所有 API 调用作为事件捕获。

### DLP 警报

实施 DLP 解决方案可能会提供额外的检测功能和警报。 详细信息可从 DLP 解决方案提供商处获得。

## 上报程序
-“谁在监控日志/警报，接收日志/警报并对其采取行动？ `
-“发现警报后谁会收到通知？ `
-“什么时候公共关系和法律参与这一过程？ `
-“你什么时候联系 AWS Support 寻求帮助？ `

## 分析
强烈建议将日志导出到安全事件事件管理 (SIEM) 解决方案（例如 Splunk、ELK stack 等），以帮助查看和分析各种日志，以进行更完整的攻击时间表分析。

### CloudTrail
CloudTrail 为所有 AWS API 调用提供长达 90 天的事件日志。 此信息可用于识别和跟踪恶意或异常行为。 更多信息可在 [CloudTrail 日志事件参考]（https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference.html）中找到。

### CloudTrail：公共 S3 存储桶
默认情况下，CloudTrail 会记录过去 90 天内发出的 API 调用，但不记录对对象的请求。 您可以在 CloudTrail 控制台上查看存储桶级别的事件。 但是，默认情况下，您无法查看数据事件（Amazon S3 对象级调用），您必须在这些事件出现在 CloudTrail 之前启用对象级日志记录。

1. 导航到 [CloudTrail 控制面板]（https://console.aws.amazon.com/cloudtrail）
1. 在左边栏中选择 “事件历史记录”
1. 在下拉菜单中，从 “只读” 更改为 “事件名称”
1. 查看 CloudTrail 日志以获取事件名称 “获取公共访问区块” 和 “删除公共访问块”

### CloudTrail：公共 S3 对象
您还可以获取对象级 Amazon S3 操作的 CloudTrail 日志。 为此，[为 S3 存储桶启用数据事件] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-data-events-with-cloudtrail.html) 或账户中的所有存储桶。 当您的账户中发生对象级别的操作时，CloudTrail 会评估您的跟踪设置。 如果事件与您在跟踪中指定的对象匹配，则会记录该事件。

1. 导航到 [CloudTrail 控制面板]（https://console.aws.amazon.com/cloudtrail）
1. 在左边栏中选择 “事件历史记录”
1. 在下拉菜单中，从 “只读” 更改为 “事件名称”
1. 查看 CloudTrail 日志以获取事件名称 `getObjectacL` 和 `putObjectacL`

### VPC Flow Logs
VPC Flow Logs 是一项功能，使您能够捕获有关进出 VPC 中网络接口的 IP 流量的信息。 这对于在 CloudTrail 中发现的 IP 地址非常有用，可以确定与任何公共资源的外部连接类型。

有关更多信息和步骤，包括使用 Athena 进行查询，请参阅 [适用于 VPC Flow Logs 的 AWS 文档] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html)。 建议将 Athena 分析纳入单独的手册中，并与其他相关项目链接。

### DNS 日志
DNS 日志是一项功能，使您能够捕获有关进出 VPC 中网络接口的 DNS 流量的信息。 这对于识别异常或高风险域名非常有用。

### CloudWatch
来自 EC2 实例和其他来源的数据可能会 [被摄入到 CloudWatch 中]（https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html）。 此数据可用于触发警报或执行分析。 CloudWatch 还提供了 [异常检测] (https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Anomaly_Detection.html) 启用的地方。

### 端点/基于主机
1. 导航到 [CloudTrail 控制面板]（https://console.aws.amazon.com/cloudtrail）
1. 在左边栏中选择 “事件历史记录”
1. 在下拉菜单中，从 “只读” 更改为 “事件名称”
1. 查看 CloudTrail 是否有来自公有 IP 地址的 `putObject` 和 `删除Object` 请求

-查看 EC2 操作系统和应用程序日志是否存在不当登录、安装未知软件或是否存在无法识别的文件。

-强烈建议使用基于主机的第三方入侵检测系统 (HIDS) 解决方案（例如 OSSEC、Tripwire、Wazuh、[Amazon Inspector]（https://aws.amazon.com/inspector/）等）

### DLP

DLP 解决方案可以根据配置进行检测和警报。 DLP 解决方案可在 [AWS 市场]（https://aws.amazon.com/marketplace/search/results?searchTerms=dlp）上获得。

## 遏制

### S3 阻止公共访问
aws s3api put-public-access-block —bucket bucket-name-here —public-access-block-configuration “BlockPublicAcls=true, IgnorePublicAcls=true, BlockPublicPolicy=true, RestrictPublicBuckets=true”

您还可以查看 [阻止对 Amazon S3 存储的公共访问] (https://aws.amazon.com/s3/features/block-public-access/)，了解有关在您的账户中阻止公共 S3 访问的更多详细信息。

### CodeCommit

使用 [AWS 身份和访问管理 (IAM) 服务结合 CodeCommit] 审核所有用户的访问控制 (https://docs.aws.amazon.com/codecommit/latest/userguide/auth-and-access-control.html)。

还可以使用 [CodeCommit 权限参考]（https://docs.aws.amazon.com/codecommit/latest/userguide/auth-and-access-control-permissions-reference.html）来更改或限制权限。

## 根除

### S3 删除无法识别的/未经授权的对象
从存储桶中删除任何无法识别的

1. 登录 AWS 管理控制台，然后通过 https://console.aws.amazon.com/s3/ 打开 Amazon S3 控制台
1. 在存储桶名称列表中，选择要从中删除对象的存储桶的名称。
1. 选择要删除的对象的名称。
1. 要删除对象的当前版本，请选择 “最新版本”，然后选择垃圾桶图标。
1. 要删除对象的早期版本，请选择 “最新版本”，然后选择要删除的版本旁边的垃圾桶图标。

### AWS CodeCommit

[审核身份和对 CodeCommit 的访问权限。] (https://docs.aws.amazon.com/codecommit/latest/userguide/security-iam.html)

### Amazon EC2

如有可能：

1. [启动替换 EC2 实例] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-launch-snapshot.html) 使用 [EBS 快照] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html) 或 [Amazon 系统映像 (AMI) 备份] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html)从源代码创建
1. [附加 EBS 卷] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-attaching-volume.html) 从已终止的实例到新的 EC2 实例。

## 恢复
与为根除所列的程序相同

## 预防措施

### 身份验证 Prowler 检查
-确保启用多重身份验证 (MFA)：
`。 /prowler-c check12`

### S3 Prowler 检查
** 加密 **
-检查 S3 存储桶是否启用了默认加密 (SSE)：`。 /prowler-c extra734`

** 灾难恢复 **
-检查 S3 存储桶是否启用了对象版本控制：`。 /prowler-c extra763`

### S3 服务操作
[防止用户修改 S3 阻止公共访问设置] (https://asecure.cloud/a/scp_s3_block_public_access/)

定期每月查看存储桶访问权限和策略，并利用 [CloudWatch Event] (https://docs.aws.amazon.com/codepipeline/latest/userguide/create-cloudtrail-S3-source-console.html) 或安全中心进行自动检测

[在 S3 存储桶中使用版本控制] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) 以减少意外或故意删除顶级对象

[使用 ACL 管理访问] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/acls.html) 以限制对存储桶和对象级别的资源的未经授权的访问

### Amazon EC2

[对每个账户使用多重身份验证 (MFA)。] (https://aws.amazon.com/iam/features/mfa/)

使用 TLS 与 AWS 资源进行通信。 我们推荐 TLS 1.2 或更高版本。 某些服务默认情况下启用此功能，其他服务则需要实施（例如在 [JavaScript SDK]（https://docs.aws.amazon.com/sdk-for-script/v2/developer-guide/enforcing-tls.html）中）。

[使用 AWS CloudTrail 设置 API 和用户活动日志记录。] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitor-with-cloudtrail.html)

[使用 AWS 加密解决方案，例如 KMS] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html)，以及 AWS 服务中的所有默认安全控制措施。

[启用 EBS 快照] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html)

### AWS CodeCommit

[对每个账户使用多重身份验证 (MFA)。] (https://aws.amazon.com/iam/features/mfa/)

使用 TLS 与 AWS 资源进行通信。 我们推荐 TLS 1.2 或更高版本。 某些服务默认情况下启用此功能，其他服务则需要实施（例如在 [JavaScript SDK]（https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/enforcing-tls.html）中）。

[使用 AWS CloudTrail 设置 API 和用户活动日志记录。] (https://docs.aws.amazon.com/codecommit/latest/userguide/integ-cloudtrail.html)

[使用 AWS 加密解决方案，例如 KMS] (https://docs.aws.amazon.com/codecommit/latest/userguide/encryption.html)，以及 AWS 服务中的所有默认安全控制措施。

[使用 Amazon Macie 之类的高级托管安全服务] (https://aws.amazon.com/blogs/compute/discovering-sensitive-data-in-aws-codecommit-with-aws-lambda-2/)，这有助于发现和保护存储在 Amazon S3 中的个人数据。

### Amazon Macie
[Amazon Macie] (https://aws.amazon.com/macie/) 可以通过 [使用托管数据标识符] (https://docs.aws.amazon.com/macie/latest/user/managed-data-identifiers.html) 检测存储的凭据、私钥和其他访问数据。

### AWS Config
[AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) 有多个自动化规则来管理代码泄露，其中包括 [codebuild-项目envvar-awscred 检查] (https://docs.aws.amazon.com/config/latest/developerguide/codebuild-project-envvar-awscred-check.html)检查凭据是否存储在代码中。

### 整体安全态势
针对环境执行 [Self-Service Security Assessment]（https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/），以进一步识别本行动手册中未发现的其他风险和其他潜在的公众风险。

### DLP 实施

实施数据丢失防护 (DLP) 解决方案可能会提供额外的检测功能和警报。 DLP 解决方案可以在 [AWS 市场] (https://aws.amazon.com/marketplace/search/results?searchTerms=dlp) 中找到，应按照规定进行配置。

## 吸取的教训
“这里是一个添加特定于贵公司的物品的地方，这些物品不需要 “修复”，但是在与运营和业务要求同时执行本手册时必须了解的。 `

## 已解决积压项目
-作为事件响应者，我需要一本关于如何检测代码暴露的运行手册
-作为事件响应者，我需要一本关于如何检测代码泄露的运行手册
-作为事件响应者，我需要能够检测公共资源（AMI、EBS 卷、ECR 回购等）
-作为事件响应者，我需要知道哪些角色能够在 AWS 中进行重大更改
-作为事件响应者，我需要一本关于缓解代码泄露和所需升级点数的行动手册
-作为事件响应者，我需要关于不同数据分类所需日志的文档

## 当前积压项目