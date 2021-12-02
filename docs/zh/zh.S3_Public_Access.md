# 事件响应行动手册：公共资源曝光-S3
本文档仅供参考。 它代表了截至本文档发布之日 Amazon Web Services (AWS) 提供的当前产品和实践，这些产品和做法如有更改，恕不另行通知。 客户有责任对本文档中的信息以及对 AWS 产品或服务的任何使用情况进行自己的独立评估，每种产品或服务都是 “按原样” 提供的，无论是明示还是暗示的担保。 本文档不创建 AWS、其附属公司、供应商或许可方的任何担保、陈述、合同承诺、条件或保证。 AWS 对客户的责任和责任受 AWS 协议的控制，本文档既不是 AWS 与其客户之间的任何协议的一部分，也不修改。

© 2021 Amazon 网络服务公司或其附属公司。 保留所有权利。 本作品根据知识共享署名 4.0 国际许可协议进行许可。

提供此 AWS 内容须遵守 http://aws.amazon.com/agreement 提供的 AWS 客户协议的条款或客户与 Amazon Web Services, Inc. 或 Amazon 网络服务 EMEA SARL 或两者之间的其他书面协议。

## 联系点

作者：`作者姓名`
批准者：`批准者姓名`
最后批准日期：

## 执行摘要
本行动手册概述了识别公共资源所有者的过程，确定谁可能在公开时访问了这些资源，确定撤销对资源的访问权限的影响，以及确定公共可访问性的根本原因的过程。

## 潜在的妥协指标
-来自 AWS 服务仪表板的公共访问警告
-CloudTrail “获取公共访问块”、“删除公共访问块”、“获取 OBJECTACL”、`PUT OBJECTACL `
-安全研究员关于公开访问资源的通知
-从公共互联网协议 (IP) 地址中删除资源

## 潜在的 AWS GuardDuty 调查结果
-Discovery: S3/MaliciousIPCaller
-Discovery: S3/MaliciousIPCaller.Custom
-Discovery: S3/TorIPCaller
-Exfiltration: S3/MaliciousIPCaller
-Exfiltration: S3/ObjectRead.Unusual
-Impact: S3/MaliciousIPCaller
-PenTest: S3/KaliLinux
-PenTest: S3/ParrotLinux
-PenTest: S3/PentooLinux
-Policy: S3/AccountBlockPublicAccessDisabled
-Policy: S3/BucketAnonymousAccessGranted
-Policy: S3/BucketBlockPublicAccessDisabled
-Policy: S3/BucketPublicAccessGranted
-Stealth: S3/ServerAccessLoggingDisabled
-UnauthorizedAccess: S3/MaliciousIPCaller.Custom
-UnauthorizedAccess: S3/TorIPCaller

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
1. [** 准备 **] 创建账户资产库存
2. [** 准备 **] 创建 S3 存储桶清单
3. [** 准备 **] 酌情启用日志记录
4. [** 准备 **] 确定每个存储桶中的数据类型
5. [** 准备 **] 识别、记录和测试上报程序
6. [** 准备 **] 实施培训以解决 DoS/DDoS 攻击
7. [** 检测和分析 **] 执行 S3 存储桶检查
8. [** 检测和分析 **] 查看 CloudTrail：公共 S3 存储桶
9. [** 检测和分析 **] 查看 CloudTrail：公共 S3 对象
10. [** 检测和分析 **] 查看 VPC Flow Logs
11. [** 检测和分析 **] 查看基于端点/主机
12. [** 包含 **] S3 阻止公共访问
13. [** 根除 **] S3 删除无法识别/未经授权的对象
14. [** RECOVERY**] 酌情执行恢复过程

*** 响应步骤遵循 [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] 中的事件响应生命周期（https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf）

! [图片] (/images/nist_life_cycle.png) ***

### 事件分类和处理
* ** 策略、技术和程序 **：AWS 服务公共访问
* ** 类别 **：公共访问
* ** 资源 **：S3
* ** 指标 **：网络威胁情报、第三方通知、Cloudwatch 指标
* ** 日志源 **：S3 服务器日志、S3 Access Logs、CloudTrail、CloudWatch
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
确定所有现有资源并拥有更新的资产库存清单以及每个资源的拥有者

#### S3 存储桶清单
-使用 AWS API [list-buckets] (https://docs.aws.amazon.com/cli/latest/reference/s3api/list-buckets.html) 显示所有 Amazon S3 存储桶的名称（跨所有区域）：`aws s3api 列表存储桶 — 查询 “存储桶 [] .Name" `

#### 启用日志记录
-检查 S3 存储桶是否在 CloudTrail 中启用了服务器级日志记录：`。 /prowler-c extra718`
-检查 S3 存储桶是否在 CloudTrail 中启用了对象级日志记录：`。 /prowler-c extra725`

#### 识别每个存储桶中的数据类型
** 选项 **
-要查看 S3 存储桶的所有文件，请使用命令：`aws s3 ls s3: //your_bucket_name —recursive`
-** 注意 ** 此 API 调用仅限于前 1000 个匹配项，不包括超过该阈值的对象
-手动分类哪些存储桶和对象对公司很重要
-向关键存储桶添加标签，以便元数据存在供将来参考

** 选项 B **
-[Amazon Macie] (https://aws.amazon.com/macie/) 是一项完全托管的数据安全和数据隐私服务，它使用机器学习和模式匹配来发现和保护 AWS 中的敏感数据。 Amazon Macie 使用机器学习和模式匹配来经济高效地大规模发现敏感数据。

### 训练
-“为了熟悉 AWS API（命令行环境）、S3、RDS 和其他 AWS 服务，公司内的分析师提供了哪些培训？ `
>>>
威胁检测和事件响应的机会包括：\
[AWS RE: INFORCE] (https://reinforce.awsevents.com/faq/)\
[Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

-“哪些角色能够对账户中的服务进行更改？ `
-“哪些用户有这些角色分配给他们？ 遵守的权限最少吗？还是存在超级管理员用户？ `
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

### S3 存储桶检查
-确保在 CloudTrail S3 存储桶上启用 S3 存储桶访问日志记录：`。 /prowler-c check26`
-确保没有向所有人或任何 AWS 用户开放的 S3 存储桶：`。 /prowler-c extra73`
-确定组织和账户中与外部实体共享的资源（例如 Amazon S3 存储桶或 IAM 角色）中与外部实体共享的资源：`。 /prowler-c extra769`
-查找暴露在互联网上的资源：`。 /prowler-g group17`

## 上报程序
-“谁在监控日志/警报，接收日志/警报并对其采取行动？ `
-“发现警报后谁会收到通知？ `
-“什么时候公共关系和法律参与这一过程？ `
-“你什么时候联系 AWS Support 寻求帮助？ `

## 分析
强烈建议将日志导出到安全事件事件管理 (SIEM) 解决方案（例如 Splunk、ELK stack 等），以帮助查看和分析各种日志，以进行更完整的攻击时间表分析。

### CloudTrail：公共 S3 存储桶
默认情况下，CloudTrail 会记录过去 90 天内发出的 API 调用，但不记录对对象的请求。 您可以在 CloudTrail 控制台上查看存储桶级别的事件。 但是，您无法在那里查看数据事件（Amazon S3 对象级调用），您必须解析或查询 CloudTrail 日志以获取这些事件。

1. 导航到你的 [CloudTrail 控制面板]（https://console.aws.amazon.com/cloudtrail）
1. 在左边栏中选择 “事件历史记录”
1. 在下拉菜单中，从 “只读” 更改为 “事件名称”
1. 查看 CloudTrail 日志以获取事件名称 “获取公共访问区块” 和 “删除公共访问块”

### CloudTrail：公共 S3 对象
您还可以获取对象级 Amazon S3 操作的 CloudTrail 日志。 为此，请为 S3 存储桶或账户中的所有存储桶启用数据事件。 当您的账户中发生对象级别的操作时，CloudTrail 会评估您的跟踪设置。 如果事件与您在跟踪中指定的对象匹配，则会记录该事件。

1. 导航到你的 [CloudTrail 控制面板]（https://console.aws.amazon.com/cloudtrail）
1. 在左边栏中选择 “事件历史记录”
1. 在下拉菜单中，从 “只读” 更改为 “事件名称”
1. 查看 CloudTrail 日志以获取事件名称 `getObjectacL` 和 `putObjectacL`

### VPC Flow Logs
VPC Flow Logs 是一项功能，使您能够捕获有关进出 VPC 中网络接口的 IP 流量的信息。 这对于在 CloudTrail 中发现的 IP 地址非常有用，以确定与任何公共资源的外部连接类型。

有关更多信息和步骤，包括使用 Athena 进行查询，请参阅 [适用于 VPC Flow Logs 的 AWS 文档] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html)。 建议将 Athena 分析纳入单独的手册中，并与其他相关内容链接。

### 端点/基于主机
1. 导航到你的 [CloudTrail 控制面板]（https://console.aws.amazon.com/cloudtrail）
1. 在左边栏中选择 “事件历史记录”
1. 在下拉菜单中，从 “只读” 更改为 “事件名称”
1. 查看 CloudTrail 是否有来自公有 IP 地址的 `putObject` 和 `删除Object` 请求

-查看 EC2 操作系统和应用程序日志是否存在不当登录、安装未知软件或是否存在无法识别的文件。

-强烈建议使用基于主机的第三方入侵检测系统 (HIDS) 解决方案（例如 OSSEC、Tripwire、Wazuh、[Amazon Inspector]（https://aws.amazon.com/inspector/）等）

## 遏制

### S3 阻止公共访问
aws s3api 放入公共访问块 — 存储桶存储桶名称 — 这里 — 公共访问区块配置 “block PublicLs=True，ignopublic acls=True，区块公共策略 = 真，限制公共Buckets=True”

您还可以查看 [阻止对 Amazon S3 存储的公共访问] (https://aws.amazon.com/s3/features/block-public-access/)，了解有关在您的账户中阻止公共 S3 访问的更多详细信息。

## 根除

### S3 删除无法识别的/未经授权的对象
从存储桶中删除任何无法识别的

1. 登录 AWS 管理控制台并通过 https://console.aws.amazon.com/s3/ 打开 Amazon S3 控制台
1. 在存储桶名称列表中，选择要从中删除对象的存储桶的名称。
1. 选择要删除的对象的名称。
1. 要删除对象的当前版本，请选择 “最新版本”，然后选择垃圾桶图标。
1. 要删除对象的早期版本，请选择 “最新版本”，然后选择要删除的版本旁边的垃圾桶图标。

## 恢复
与为根除所列的程序相同

## 预防性行动

### S3 Prowler 检查
** 加密 **
-检查 S3 存储桶是否启用了默认加密 (SSE)：`。 /prowler-c extra734`

** 灾难恢复 **
-检查 S3 存储桶是否启用了对象版本控制：`。 /prowler-c extra763`

### S3 服务操作
[防止用户修改 S3 阻止公共访问设置] (https://asecure.cloud/a/scp_s3_block_public_access/)

定期每月查看存储桶访问权限和策略，并利用 [CloudWatch Event] (https://docs.aws.amazon.com/codepipeline/latest/userguide/create-cloudtrail-S3-source-console.html) 或安全中心进行自动检测

[在 S3 存储桶中使用版本控制] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) 以减轻意外或故意删除顶级对象

[使用 ACL 管理访问] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/acls.html) 以限制对存储桶和对象级别的资源的未经授权的访问

### AWS Config
[AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) 有多个自动化规则来防止公共访问，包括 [s3-bucket-level-public-access-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-level-public-access-prohibited.html)。

### 整体安全态势
针对环境执行 [Self-Service Security Assessment]（https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/），以进一步识别本行动手册中未发现的其他风险和其他潜在的公众风险。

## 吸取的教训
“这是一个添加不需要 “修复” 但在与运营和业务要求同时执行本行动手册时必须了解的特定项目的地方。 `

## 已解决积压项目
-作为事件响应者，我需要一本关于如何减少错误公开的资源的运行手册
-作为事件响应者，我需要能够检测公共资源（AMI、EBS 卷、ECR 回购等）
-作为事件响应者，我需要知道哪些角色能够在 AWS 中进行重大更改
-作为事件响应者，我需要一本关于减少公共存储桶风险和所需升级积分的行动手册
-作为事件响应者，我需要有关不同存储桶分类所需日志的文档

## 当前积压项目