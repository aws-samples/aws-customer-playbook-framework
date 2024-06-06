# 事件响应行动手册：受损的 IAM 凭证
本文档仅供参考。 它代表了截至本文档发布之日 Amazon Web Services (AWS) 提供的当前产品和实践，这些产品和做法如有更改，恕不另行通知。 客户有责任对本文档中的信息以及对 AWS 产品或服务的任何使用情况进行自己的独立评估，每种产品或服务都是 “按原样” 提供的，无论是明示还是暗示的担保。 本文档不创建 AWS、其附属公司、供应商或许可方的任何担保、陈述、合同承诺、条件或保证。 AWS 对客户的责任和责任受 AWS 协议的控制，本文档既不是 AWS 与其客户之间的任何协议的一部分，也不修改。

© 2024 Amazon 网络服务公司或其附属公司。 保留所有权利。 本作品根据知识共享署名 4.0 国际许可协议进行许可。

提供此 AWS 内容须遵守 http://aws.amazon.com/agreement 提供的 AWS 客户协议的条款或客户与 Amazon Web Services, Inc. 或 Amazon 网络服务 EMEA SARL 或两者之间的其他书面协议。

## 联系点

作者：`作者姓名`\
批准者：`批准者姓名`\
最后批准日期：

## 执行摘要
本行动手册概述了当您在 AWS 账户内观察到未经授权的活动或者您认为未经授权的方访问了您的账户时应做出响应的流程。

## 潜在的妥协指标
-新的或无法识别的 IAM 用户
-无法识别或未经授权的资源（例如 EC2、Lambda）
-不寻常的账单增加
-来自安全研究员的通知
-通知我的 AWS 资源或账户可能会被盗用

## 潜在的 AWS GuardDuty 调查结果
-CredentialAccess: IAMUser/AnomalousBehavior
-DefenseEvasion: IAMUser/AnomalousBehavior
-Discovery: IAMUser/AnomalousBehavior
-Exfiltration: IAMUser/AnomalousBehavior
-Impact: IAMUser/AnomalousBehavior
-InitialAccess: IAMUser/AnomalousBehavior
-PenTest: IAMUser/KaliLinux
-PenTest: IAMUser/ParrotLinux
-PenTest: IAMUser/PentooLinux
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
1. [** 准备 **] 执行资产库存
2. [** 准备 **] 实施培训计划以识别和响应公开的 IAM 证书
3. [** 准备 **] 实施事件响应的沟通策略
4. [** 检测 **] 识别 Root 账户访问权限（已授权和未授权）
5. [** 检测 **] 识别新的或无法识别的 IAM 用户
6. [** 检测 **] 识别无法识别或未经授权的资源（例如 EC2、Lambda）
7. [** 检测 **] 识别并查找暴露的秘密
8. [** 检测 **] 识别不寻常的账单增加
9. [** 检测 **] 回复来自 AWS 或第三方关于我的 AWS 资源或账户可能遭到入侵的通知
10. [** 检测 **] 识别任何潜在未经授权的 IAM 用户凭证
11. [** 准备 **] 确定上报程序
12. [** 检测和分析 **] 查看 CloudTrail 日志
13. [** 检测和分析 **] 查看 VPC Flow Logs
14. [** 检测和分析 **] 查看端点/基于主机的日志
15. [** 包含 **] 执行适当的遏制操作
16. [**ERADICATION**] 查看来自被泄露的访问密钥查看 CloudTrail 事件历史记录中的调查结果以了解活动
17. [** 根除 **] 查看避免意外费用
18. [** 恢复 **] 执行适当的恢复操作
19. [** 准备 **] 执行 Prowler IAM 扫描
20. [** 准备 **] 启用 MFA
21. [** 准备 **] 验证您的账户信息
22. [** 准备 **] 使用 AWS Git 项目扫描未经授权使用的证据
23. [** 准备 **] 避免使用 root 用户进行日常操作
24. [** 准备 **] 评估你的整体安全状况

*** 响应步骤遵循 [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] 中的事件响应生命周期（https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf）

! [图片] (/images/nist_life_cycle.png) ***

### 事件分类和处理
* ** 策略、技术和程序 **：证书曝光
* ** 类别 **：IAM 凭证曝光
* ** 资源 **：IAM
* ** 指标 **：网络威胁情报，第三方通知
* ** 日志源 **：AWS CloudTrail、AWS Config、VPC Flow Logs、Amazon GuardDuty
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

此工具提供了客户环境中当前安全状态的快速快照。 作为替代方法，[AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) 提供了自动合规性扫描，并可以 [与 Prowler 集成] (https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### 资产库存
识别所有现有用户并获得每个账户用途的最新列表

1. 导航到 [AWS 控制台]（https://console.aws.amazon.com/）
1. 转到 “服务” 然后选择 `IAM`
1. 在左侧菜单中，选择 “凭据报告”
1. 选择 “下载报告”
1. 特别注意创建帐户的时间、上次使用的密码和上次更改密码列。
* ** 注意 ** 如果用户有多个访问密钥，请验证每个访问密钥的用途

### 训练
-“为了熟悉 AWS API（命令行环境）、S3、RDS 和其他 AWS 服务，公司内的分析师提供了哪些培训？ `
>>>
威胁检测和事件响应的机会包括：\
[AWS RE: INFORCE] (https://reinforce.awsevents.com/faq/)\
[Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

-“哪些角色能够更改账户中的服务？ `
-“哪些用户有这些角色分配给他们？ 遵守的权限最少吗？还是存在超级管理员用户？ `
-“是否针对您的环境进行了安全评估，您是否有已知的基准来检测 “新” 或 “可疑” 事物？ `

### 通信技术
-“团队/公司内部使用什么技术来沟通问题？ 有什么自动化的吗？ `
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

### Root 账户访问
建议删除与根账户关联的所有访问密钥：`。 /prowler-c check_112`

### 新的或无法识别的 IAM 用户
查看 [资产库存] 中的 IAM 凭证报告 (. /COMMED_iam_Credentials.md/ #asset-库存）
检查 IAM 用户是否有两个活动访问密钥：`。 /prowler-c check_extra712`
确保未创建允许完全\ "*: *\” 管理权限的 IAM 策略：`。 /prowler-c check_122`
检查 IAM 访问分析器是否已启用及其发现：`。 /prowler-c check_extra769`

### 无法识别或未经授权的资源（例如 EC2、Lambda）
aws ec2 describe-instances
aws lambda list-functions

### 寻找秘密
在 EC2 实例中找到潜在的秘密用户数据：`。 /prowler-c check_extra741`
在 Lambda 函数变量中找到的潜在秘密：`。 /prowler-c check_extra759`
ECS 任务定义变量中的潜在秘密：`。 /prowler-c check_extra768`
在 AutoScaling 配置中找到潜在的秘密：`。 /prowler-c check_extra775`

### 不寻常的账单增加
要查看 AWS 账单，请打开账单和成本管理控制台的 [账单] (https://console.aws.amazon.com/billing/home #) 窗格，然后从下拉菜单中选择要查看的月份。

您可以在账单和成本管理控制台的 [订单和发票] (https://console.aws.amazon.com/billing/home?/paymenthistory) 窗格中查看 AWS 付款历史记录。

您还可以使用 [AWS Cost Anomaly Detection] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-ad.html) 进行动态监控和警报。

检查账单以获取以下信息：
* 您通常不使用的 AWS 服务
* AWS 区域中通常不使用的资源
* 账单的大小发生了重大变化

您可以使用此信息来帮助您删除或终止任何不想保留的资源。

### 我的 AWS 资源或账户可能遭到泄露的通知
如果您收到来自 AWS 的有关账户的通知，请登录 AWS 支持中心，然后回复通知。

如果您无法登录账户，请使用联系我们表单向 AWS Support 请求帮助。

如果您有任何问题或疑虑，请在 AWS 支持中心创建一个新的 AWS Support 案例。
* ** 注意 **：请勿在信件中包含敏感信息，例如 AWS 访问密钥、密码或信用卡信息。
使用 AWS Git 项目扫描未经授权使用的证据

### 识别任何潜在未经授权的 IAM 用户凭证
1. 打开 IAM 控制台。
1. 在导航窗格中选择用户。
1. 从列表中选择每个 IAM 用户，然后在权限策略下查看名为 AWSExposedCredentialPolicy_DO_NOT_REMOVE 的策略。1. 如果用户具有此附加策略，则必须轮换用户的访问密钥。

## 上报程序
-“谁在监控日志/警报，接收日志/警报并对其采取行动？ `
-“发现警报后谁会收到通知？ `
-“什么时候公共关系和法律参与这一过程？ `
-“你什么时候联系 AWS Support 寻求帮助？ `

## 分析
强烈建议将日志导出到安全事件事件管理 (SIEM) 解决方案（例如 Splunk、ELK stack 等），以帮助查看和分析各种日志，以进行更完整的攻击时间表分析。

### CloudTrail
寻找不寻常的登录活动
1. 导航到 [CloudTrail 控制面板]（https://console.aws.amazon.com/cloudtrail）
1. 在左边栏中选择 “事件历史记录”
1. 在下拉菜单中，从 “只读” 更改为 “事件名称”
1. 在搜索字段中输入 “ConsoleLOGINE” 或 “GetFenedationToken” 或 “获取凭据报告” 或 “生成证书报告”，然后查看可用事件是否有任何可疑活动
* ** 注意 ** userIdentity 将显示为 “类型”：“root” `表示 Root 目录或 “” 类型”：“iAMUser"` 对于用户

找到用于启动可疑 EC2 实例的 IAM 访问密钥 ID 和用户名
1. 打开 CloudTrail 控制台，然后选择事件历史记录。
1. 选择筛选器下拉菜单，然后选择资源名称。
1. 在输入资源名称字段中，粘贴 EC2 实例 ID，然后在设备上选择 Enter。
1. 展开 RunInstances 的事件名称。
1. 复制 AWS 访问密钥，然后记下用户名。

通过受损访问密钥查看 CloudTrail 事件历史记录中的活动
1. 打开 CloudTrail 控制台，然后从导航窗格中选择事件历史记录。
1. 选择 Filter 下拉菜单，然后选择 AWS 访问密钥过滤器。
1. 在输入 AWS 访问密钥字段中，输入受损的 IAM 访问密钥 ID。
1. 展开 API 调用 RunInstances 的事件名称。
* ** 注意 **：除非您之前配置了保存到 S3 存储桶中的跟踪，否则您只能查看过去 90 天的事件历史记录

### VPC Flow Logs
VPC Flow Logs 是一项功能，使您能够捕获有关进出 VPC 中网络接口的 IP 流量的信息。 这对于在 CloudTrail 中发现的 IP 地址非常有用，可以确定与任何公共资源的外部连接类型。

有关更多信息和步骤，包括使用 Athena 进行查询，请参阅 [适用于 VPC Flow Logs 的 AWS 文档] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html)。 建议将 Athena 分析包含在单独的手册中，并与其他相关内容链接。

### 端点/基于主机
* ** 识别给定实例名称的上次创建时间 **：`aws ec2 描述实例 — 查询 '预留 []。实例 []。 {ip：PublicipAddress，tm：LaunchTime} '— 过滤器 “名称 = 标签名称，值 = 我的实例名称” | jq 'sort_by (.tm) | 反向 |. [0] '`

* ** 识别 lambda 函数的上次修改时间 **：`aws lambda 列表函数 | grep “最后修改” `

## 遏制
1. 禁用 IAM 用户，创建备份 IAM 访问密钥，然后禁用受损的访问密钥
1. 打开 [IAM 控制台]（https://console.aws.amazon.com/iam/），然后将 IAM 访问密钥 ID 粘贴到搜索 IAM 栏中。
1. 选择用户名，然后选择 “安全凭据” 选项卡。
1. 在控制台密码中，选择管理。
* ** 注意 **：如果 AWS 管理控制台密码设置为禁用，则可以跳过此步骤。
1. 在控制台访问权限中，选择禁用，然后选择应用。
1. 重要提示：账户被禁用的用户无法访问 AWS 管理控制台。 但是，如果用户拥有活动访问密钥，他们仍然可以使用 API 调用访问 AWS 服务。
1. 对于受损的 IAM 访问密钥，请选择设为非活动状态。
1. 禁用应用程序 IAM 密钥，创建备份 IAM 访问密钥，然后禁用受损的访问密钥
1. 首先，创建第二个密钥。 然后，修改应用程序以使用新密钥。
1. 禁用（但不要删除）第一个密钥。
1. 如果应用程序有任何问题，请暂时重新激活密钥。 当您的应用程序完全正常工作且第一个密钥处于禁用状态时，请删除第一个密钥。
1. 旋转并删除所有不活动/受损的 AWS 访问密钥
* **!! 确保记录所有已删除的访问密钥，以便您可以继续在 CloudTrail 中搜索它们！ **

## 根除
### 查看来自 [查看 CloudTrail 事件历史记录以查看受损的访问密钥的活动] 的调查结果] (. /#cloudtrail)
删除受损密钥创建的所有资源。 务必检查所有 AWS 区域，甚至是您从未启动 AWS 资源的区域。
* ** 重要 **：如果您需要保留任何资源进行调查，请考虑备份它们。 例如，如果您有保留 EC2 实例的监管、合规性或法律需求，请在终止实例之前拍摄 EBS 快照。

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

删除您没有创建的任何 IAM 用户
1. 登录 AWS 管理控制台并打开 [IAM 控制台] (https://console.aws.amazon.com/iam/)
1. 在导航窗格中，选择用户，然后选中要删除的用户名旁边的复选框，而不是名称或行本身。
1. 在页面顶部，选择删除用户。
1. 在确认对话框中，等待上次访问的信息加载，然后再查看数据。 该对话框显示每个选定用户上次访问 AWS 服务的时间。 如果您尝试删除过去 30 天内处于活动状态的用户，则必须选中其他复选框以确认要删除活动用户。 如果要继续，请选择是，删除。

有关删除其他服务的文档可以在 [终止我的所有资源] (https://aws.amazon.com/premiumsupport/knowledge-center/terminate-resources-account-closure/) 页面上找到。

## 恢复
AWS 发布了关于 [如果我注意到我的 AWS 账户中存在未经授权的活动该怎么办？] (https://aws.amazon.com/premiumsupport/knowledge-center/potential-account-compromise/)

## 预防性行动
### Prowler IAM 扫描
`。 /prowler-g 检查 122、检查 111、检查 110、检查 19、检查 18、检查 17、检查 16、检查 15、检查 11、检查 116、检查 12、检查 114、检查 14、检查 13、检查 112、119、额外 71、额外 7100、额外 7123、额外 7125、额外 769、额外 769、额外 774`

### 启用 MFA
为了提高安全性，最佳做法是配置 MFA 以帮助保护您的 AWS 资源。 您可以启用 [IAM 用户的 MFA] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html) 或 [AWS 账户根用户] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html)。 为根用户启用 MFA 仅影响根用户凭证。 账户中的 IAM 用户是具有自己凭证的独特身份，每个身份都有自己的 MFA 配置。

### 验证你的账户信息
AWS 需要准确的账户信息才能与您联系并帮助解决任何账户问题。 检查您账户上的信息是否正确。
* 帐户名称和电子邮件地址。
* 你的联系信息，尤其是你的电话号码。
* 账户的备用联系人。

### 使用 AWS Git 项目扫描未经授权使用的证据
AWS 提供了可以安装的 Git 项目以帮助您保护账户：
* [Git Secrets ret] (https://github.com/awslabs/git-secrets) 可以扫描合并、提交和提交消息以获取秘密信息（即访问密钥）。 如果 Git Secrets 检测到禁止的正则表达式，它可以拒绝将这些提交发布到公共仓库。
* 使用 [AWS 步骤函数和 AWS Lambda 生成 Amazon CloudWatch 事件] (https://aws.amazon.com/step-functions) 来自 AWS Health 或 AWS 信任顾问。 如果有证据表明您的访问密钥已泄露，那么这些项目可以帮助您自动检测、记录和缓解事件。

### 避免使用 root 用户进行日常操作
AWS 账户根用户的访问密钥可以完全访问您的所有 AWS 资源，包括账单信息。 您无法减少与 AWS 账户根用户访问密钥关联的权限。 除非绝对必要，否则不要使用 root 用户访问权限是最佳做法。

如果您还没有 AWS 账户根用户的访问密钥，除非绝对必要，否则不要创建访问密钥。 相反，应为自己创建具有管理权限的 IAM 用户。 您可以使用 AWS 账户电子邮件地址和密码登录 AWS 管理控制台以创建 IAM 用户。

### 整体安全态势
针对环境执行 [Self-Service Security Assessment]（https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/），以进一步识别本手册中未发现的其他风险和其他潜在的公众风险。

## 吸取的教训
“这里可以添加特定于贵公司的物品，这些物品不一定需要 “修复”，但在与运营和业务要求同时执行本行动手册时也很重要。 `

## 已解决积压项目
-作为事件响应者，我需要一本关于如何在事件发生后处理会话令牌和访问密钥的行动手册
-作为事件响应者，我需要一种方法来扫描和从公共代码存储库中删除密钥
-作为事件响应者，我需要一个明确的决策者，了解何时破坏环境与允许持续曝光
## 当前积压项目