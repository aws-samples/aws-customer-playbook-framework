# 事件响应行动手册：拒绝服务/分布式拒绝服务 (dos/DDoS)
本文档仅供参考。 它代表了截至本文档发布之日 Amazon Web Services (AWS) 提供的当前产品和实践，这些产品和做法如有更改，恕不另行通知。 客户有责任对本文档中的信息以及对 AWS 产品或服务的任何使用情况进行自己的独立评估，每种产品或服务都是 “按原样” 提供的，无论是明示还是暗示的担保。 本文档不创建 AWS、其附属公司、供应商或许可方的任何担保、陈述、合同承诺、条件或保证。 AWS 对客户的责任和责任受 AWS 协议的控制，本文档既不是 AWS 与其客户之间的任何协议的一部分，也不修改。

© 2024 Amazon 网络服务公司或其附属公司。 保留所有权利。 本作品根据知识共享署名 4.0 国际许可协议进行许可。

提供此 AWS 内容须遵守 http://aws.amazon.com/agreement 提供的 AWS 客户协议的条款或客户与 Amazon Web Services, Inc. 或 Amazon 网络服务 EMEA SARL 或两者之间的其他书面协议。

## 联系点

作者：`作者姓名`\
批准者：`批准者姓名`\
最后批准日期：

## 执行摘要
本行动手册概述了应对针对 AWS 托管资源的拒绝服务/分布式拒绝服务 (DoS/DDoS) 攻击的流程。

## 潜在的妥协指标
-面向互联网的应用程序承受沉重负载，原因是受欢迎程度或恶意意图
-正在使用单个 EC2 实例托管应用程序，我们正在丢弃/调整流量（通过 AWS Abuse 通知客户）
-AWS Shield 控制面板中显示的结果

## 潜在的 AWS GuardDuty 调查结果
-Backdoor: EC2/DenialOfService.Dns
-Backdoor: EC2/DenialOfService.Tcp
-Backdoor: EC2/DenialOfService.Udp
-Backdoor: EC2/DenialOfService.UdpOnTcpPorts
-Backdoor: EC2/DenialOfService.UnusualProtocol
-Backdoor: EC2/Spambot
-Behavior: EC2/NetworkPortUnusual
-Behavior: EC2/TrafficVolumeUnusual

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
[AWS Cloud Adoption Framework 安全角度] (https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* ** 指令 **
* ** 侦探 **
* ** 响应 **
* ** 预防性 **

! [图片] (/images/aws_caf.png)
* * *

### 响应步骤
1. [** 准备 **] 创建公开资产库存
2. [** 准备 **] 实施培训以解决 DoS/DDoS 攻击
3. [** 准备 **] 制定沟通策略来上报和报告事件
4. [** 准备 **] 执行文档审查以确保程序得到维持和最新
5. [** 检测和分析 **] 实施 AWS Shield
6. [** 检测和分析 **] 利用 AWS Shield 的 Global Threat Environment Dashboard
7. [** 检测和分析 **] 考虑使用 AWS Shield Advanced
8. [** 准备 **] 实施上报和通知程序
9. [** 检测和分析 **] 执行检测和缓解
10. [** 检测和分析 **] 识别主要贡献者
11. [** 包含 **] 执行遏制
12. [** 根除 **] 执行根除
13. [** 事故后活动 **] 在 AWS Shield Advanced 中申请积分
14. [** 准备 **] 其他预防措施：缓解技术
15. [** 准备 **] 其他预防措施：减少攻击面
16. [** 准备 **] 其他预防措施：操作技术
17. [** 准备 **] 其他预防措施：总体安全状况

*** 响应步骤遵循 [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] 中的事件响应生命周期（https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf）

! [图片] (/images/nist_life_cycle.png) ***

### 事件分类和处理

* ** 策略、技术和程序 **：拒绝服务/分布式拒绝服务攻击
* ** 类别 **：dos/DDoS
* ** 资源 **：网络
* ** 指标 **：网络威胁情报、第三方通知、Cloudwatch 指标、AWS Shield
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

此工具提供了客户环境中当前安全状态的快速快照。 此外，[AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) 提供了自动合规性扫描并可以 [与 Prowler 集成] (https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### 公开资产库存
查找在互联网上暴露的资源：`。 /prowler-g group17`

您可以使用 Security Hub 和防火墙管理器来 [为 DDoS 事件设置集中式监控并自动修复不合规资源] (https://aws.amazon.com/blogs/security/set-up-centralized-monitoring-for-ddos-events-and-auto-remediate-noncompliant-resources/)

### 训练
-“为了熟悉 AWS API（命令行环境）、S3、RDS 和其他 AWS 服务，公司内的分析师提供了哪些培训？ `
>>>
威胁检测和事件响应的机会包括：\
[AWS RE: INFORCE] (https://reinforce.awsevents.com/faq/)\
[Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

-“哪些角色能够更改账户中的服务？ `
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

### 文档审查
1. 熟悉 [AWS Shield 文档]（https://docs.aws.amazon.com/waf/latest/developerguide/shield-chapter.html）
1. 如果已订阅，请遵循 [AWS Shield Advanced 入门指南]（https://docs.aws.amazon.com/waf/latest/developerguide/getting-started-ddos.html）

## 检测
### Cloudwatch 指标用于检测 doS/DDoS 事件
推荐的 Amazon CloudWatch Metrics 表列出了 Amazon CloudWatch 指标的描述，这些指标通常用于检测和应对 DDoS 攻击。

* AWS Shield Advanced: DDoSDetected
* 表示特定 Amazon Resource Name (ARN) 的 DDoS 事件。
* AWS Shield Advanced: DDoSAttackBitsPerSecond
* 特定 ARN 在 DDoS 事件期间观察到的字节数。 此指标仅适用于第 3/4 层 DDoS 事件。
* AWS Shield Advanced: DDoSAttackPacketsPerSecond
* 在 DDoS 事件期间观察到的特定 ARN 的数据包数。 此指标仅适用于第 3/4 层 DDoS 事件。
* AWS Shield Advanced: DDoSAttackRequestsPerSecond
* 在 DDoS 事件期间观察到的特定 ARN 的请求数。 此指标仅适用于第 7 层 DDoS 事件，仅针对最重要的第 7 层事件报告。
* AWS WAF: AllowedRequests
* 允许的 Web 请求的数量。
* AWS WAF: BlockedRequests
* 被阻止的 Web 请求的数量。
* AWS WAF: CountedRequests
* 计数的网络请求的数量。
* AWS WAF: PassedRequests
* 已通过的请求数。 这仅用于在未与任何规则组规则匹配的情况下通过规则组评估的请求。
* CloudFront: Requests
* HTTP/S 请求的数量。
* CloudFront: TotalErrorRate
* HTTP 状态代码为 4xx 或 5xx 的所有请求的百分比。
* Amazon Route 53: HealthCheckStatus
* 运行状况检查终端节点的状态。
* ALB: ActiveConnectionCount
* 从客户端到负载均衡器以及从负载均衡器到目标处于活动状态的并发 TCP 连接总数。
* ALB: ConsumedLCUs
* 负载均衡器使用的负载均衡器容量单位 (LCU) 的数量。
* ALB: HTTPCode_ELB_4XX_Count HTTPCode_ELB_5XX_Count
* 负载均衡器生成的 HTTP 4xx 或 5xx 客户端错误代码的数量。
* ALB: NewConnectionCount
* 从客户端到负载均衡器以及从负载均衡器到目标之间建立的新 TCP 连接总数。
* ALB: ProcessedBytes
* 负载均衡器处理的总字节数。
* ALB: RejectedConnectionCount
* 由于负载均衡器达到了最大连接数而被拒绝的连接数。
* ALB: RequestCount
* 已处理的请求数。
* ALB: TargetConnectionErrorCount
* 负载均衡器和目标之间未成功建立的连接数。
* ALB: TargetResponseTime
* 在请求离开负载均衡器之后，直到收到来自目标的响应的时间（以秒为单位）。
* ALB: UnHealthyHostCount
* 被认为不健康的目标数量。
* NLB: ActiveFlowCount
* 从客户端到目标的并发 TCP 流（或连接）总数。
* NLB: ConsumedLCUs
* 负载均衡器使用的负载均衡器容量单位 (LCU) 的数量。
* NLB: NewFlowCount
* 在该时间段内从客户端到目标的新 TCP 流（或连接）的总数。
* NLB: ProcessedBytes
* 负载均衡器处理的总字节数，包括 TCP/IP 标头。
* Global Accelerator NewFlowCount
* 在该时间段内从客户端到终端建立的新 TCP 和 UDP 流（或连接）的总数。
* Global Accelerator: ProcessedBytesIn
* 加速器处理的传入字节总数，包括 TCP/IP 标头。
* Auto Scaling: GroupMaxSize
* Auto Scaling 组的最大大小。
* Amazon EC2: CPUUtilization
* 当前正在使用的已分配 EC2 计算单元的百分比。
* Amazon EC2: NetworkIn
* 实例在所有网络接口上接收的字节数。
### AWS Shield
[AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/shieldv2) 或 API 提供有关检测到的攻击的摘要和详细信息。
1. 登录 AWS 管理控制台并打开 [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. 在 AWS Shield 导航栏中，选择概述或活动

### Global Threat Environment Dashboard
全 Global Threat Environment Dashboard 提供有关 AWS 检测到的所有 DDoS 攻击的摘要信息。
1. 登录 AWS 管理控制台并打开 [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. 在 AWS Shield 导航栏中，选择全球威胁控制面板。
1. 选择一个时间段。

### AWS Shield Advanced
1. 登录 AWS 管理控制台并打开 [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. 在 AWS Shield 导航栏中，选择概述或活动

您可以访问许多 CloudWatch 指标，这些指标可以表明您的应用程序正在成为目标。
1. 您可以配置 DDoSDetected etect 指标以告诉您是否检测到了攻击。
1. 如果你想根据攻击量收到警报，你也可以使用 DDoSAttackBitsPerSecond、DDoSAttackPacketsPerSecond 或 DDoSAttackRequestsPerSecond 指标。

[DDoS 可见性]（https://docs.aws.amazon.com/whitepapers/latest/aws-best-practices-ddos-resiliency/visibility.html）的表格中提供了可用指标的完整列表

## 上报程序

### AWS Shield
-“谁在监控日志/警报，接收日志/警报并对其采取行动？ `
-“发现警报后谁会收到通知？ `
-“什么时候公共关系和法律参与这一过程？ `
-“你什么时候联系 AWS Support 寻求帮助？ `

### AWS Shield Advanced
-“谁在监控日志/警报，接收日志/警报并对其采取行动？ `
-“发现警报后谁会收到通知？ `
-“什么时候公共关系和法律参与这一过程？ `
-“您是否使用 Shield Advanced 指定了要保护的 AWS 资源？ `
1. 登录 AWS 管理控制台并打开 [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. 在 AWS Shield 导航栏中，选择受保护的资源
-“你是否授权 DDoS 响应小组 (DRT) 访问你的 WAF 规则和日志？ 当您请求帮助时，这有助于他们缓解攻击。 `
1. 登录 AWS 管理控制台并打开 [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. 在 AWS Shield 导航栏中，选择概述
1. 配置 AWS DRT 支持 “编辑 DRT 访问权限”
-“您需要 AWS DDoS 响应团队的支持吗？ `
1. 您可以在 AWS 支持中心的 AWS Shield 下开启案例
* 要获得 DDoS 响应团队 (DRT) 支持，请联系 AWS 支持中心并选择以下选项：
1. 案例类型：技术支持
1. 服务：分布式拒绝服务 (DDoS)
1. 类别：入库到 AWS
* 借助 AWS Shield Advanced 主动参与，如果与受保护资源关联的 Amazon Route 53 运行状况检查在 Shield Advanced 检测到的事件中变得不健康，DRT 会直接与您联系。

## 分析
1. 登录 AWS 管理控制台并打开 [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. 在 AWS Shield 导航栏中，选择事件

### 检测和缓解
检测和缓解选项卡显示的图表显示了 AWS Shield 在报告此事件之前评估的流量以及所发出的任何缓解措施的影响。 它还为您自己的 Amazon 虚拟私有云 VPC 和 AWS 全球加速器中的资源提供基础设施层分布式拒绝服务 (DDoS) 事件的指标。 不包括针对 Amazon CloudFront 或 Amazon Route 53 资源的检测和缓解指标。

### 顶级贡献者
“顶级贡献者” 选项卡可以深入了解检测到的事件期间流量来自何处。 您可以查看数量最高的贡献者，这些贡献者按协议、源端口和 TCP 标志等方面进行排序。 您可以下载信息，然后调整使用的单位和要显示的贡献者数量。

## 遏制

### [AWS DDoS 弹性最佳实践] (https://d0.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf)
在本白皮书中，AWS 为您提供了规范性的 DDoS 指南，以提高在 AWS 上运行的应用程序的弹性。 这包括一个 DDoS 弹性参考体系结构，可用作帮助保护应用程序可用性的指南。 本白皮书还介绍了不同的攻击类型，例如基础架构层攻击和应用程序层攻击。 AWS 解释了哪些最佳实践最有效地管理每种攻击类型。 此外，还概述了适合 DDoS 缓解策略的服务和功能，以及如何使用每个服务和功能来帮助保护应用程序。
遏制过程假定使用 AWS Web 应用程序防火墙。 如果正在使用第三方 WAF 或防火墙，请在此自定义这些过程以匹配您的使用案例。

### AWS WAF
1. 在 AWS WAF 中创建符合异常行为的条件。
1. 将这些条件添加到一个或多个 AWS WAF 规则中。
1. 将这些规则添加到 Web ACL 中，然后配置 Web ACL 以计算与规则匹配的请求。
* ** 注意 ** 您应始终首先使用 Count 而不是 Block 来测试规则。 在您感到满意的是新规则确定了正确的请求之后，您可以修改规则以阻止这些请求。
1. 监控这些计数以确定是否应该阻止请求的来源。 如果请求量仍然异常高，请更改 Web ACL 以阻止这些请求。

## 根除
不适用

## 恢复
### 在 AWS Shield Advanced 中申请积分
如果您订阅了 AWS Shield Advanced，并且遇到了 DDoS 攻击，这些攻击会提高 Shield Advanced 受保护资源的利用率，则可以请求扣除与提高利用率相关的费用，但以 Shield Advanced 无法减轻的情况为限。

其他信息可在 AWS 文档中找到 [在 AWS Shield Advanced 中申请积分] (https://docs.aws.amazon.com/waf/latest/developerguide/request-refund.html)

## 预防性行动
### 缓解技术
在 AWS 上，自动提供 DDoS 缓解功能
1. AWS Shield Standard 可防御针对您的网站或应用程序的最常见、经常发生的网络和传输层 DDoS 攻击。 这适用于所有 AWS 服务和每个 AWS 区域，无需额外费用。
1. 通过订阅 AWS Shield Advanced，您可以提高应对和缓解 DDoS 攻击的准备程度。 此可选的 DDoS 缓解服务可帮助您保护任何 AWS 区域托管的应用程序。 该服务在全球范围内可用于 Amazon CloudFront 和 Amazon Route 53。 它也可在经典负载均衡器 (CLB)、应用程序负载均衡器 (ALB) 和弹性 IP 地址 (EIP) 的指定 AWS 区域中提供。 将 AWS Shield Advanced 与 EIP 结合使用允许您保护网络负载均衡器 (NLB) 或 Amazon EC2 实例。
1. 帮助缓解大量 DDoS 攻击的关键考虑因素包括确保有足够的传输容量和多样性可用，以及保护 AWS 资源（如 Amazon EC2 实例）免受攻击流量
1. 大型 DDoS 攻击可能会压倒单个 Amazon EC2 实例的容量，因此添加负载均衡可以帮助您的恢复能力。
1. Amazon CloudFront 允许您缓存静态内容并从 AWS 节点提供静态内容，这有助于减少源的负载。
1. 通过使用 AWS WAF，您可以在 CloudFront 分配或应用程序负载均衡器上配置 Web 访问控制列表 (Web ACL)，以根据请求签名筛选和阻止请求。
### 攻击面减少
1. 您可以在启动实例时指定安全组，也可以稍后将该实例与安全组关联。 除非您创建允许规则允许流量，否则将隐式拒绝向安全组发送的所有互联网流量。
* 如果您订阅了 AWS Shield Advanced，则可以将弹性 IP (EIP) 注册为受保护的资源。
1. 如果您使用的 Amazon CloudFront 源位于 VPC 内部，则应使用 AWS Lambda 函数自动更新您的安全组规则，以便仅允许 Amazon CloudFront 流量。
1. 通常，当您必须向公众公开 API 时，API 前端有可能成为 DDoS 攻击目标的风险。 为了帮助降低风险，您可以使用 Amazon API Gateway 作为在 Amazon EC2、AWS Lambda 或其他地方运行的应用程序的入口。
* 当您将 Amazon CloudFront 和 AWS WAF 与 Amazon API Gateway 结合使用时，请配置以下选项：
1. 配置分配的缓存行为，以便将所有标头转发到 API Gateway 区域终端节点。 通过这样做，CloudFront 将把内容视为动态内容并跳过缓存内容。
1. 通过在 API Gateway 中设置 API 密 keyvalue，将分配配置为包含源自定义标头 x-api-key，保护 API Gateway 免受直接访问的影响。
1. 通过在 RESTAPIs 中为每种方法配置标准或突发速率限制，保护后端免受过多流量的影响。

### 操作技巧
当关键操作指标与预期值大大偏离时，攻击者可能会试图以应用程序的可用性为目标。 如果您熟悉应用程序的正常行为，则可以在检测到异常情况时更快地采取行动。
1. 如果您按照 DDoS 弹性参考体系结构构建了应用程序，则在到达应用程序之前，将阻止常见的基础设施层攻击。
1. 如果您使用的是 AWS WAF，则可以使用 CloudWatch 监控和警报您在 WAF 中设置为允许、计数或阻止的请求的增加情况。 这允许您在流量超过应用程序可处理的流量时收到通知。

有关常用于检测和应对 DDoS 攻击的 Amazon CloudWatch 指标的描述，请参阅 [DDoS 可见性]（https://docs.aws.amazon.com/whitepapers/latest/aws-best-practices-ddos-resiliency/visibility.html）中的表格
### 整体安全态势
针对环境执行 [Self-Service Security Assessment]（https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/），以进一步识别本手册中未发现的其他风险和其他潜在的公众风险。

## 吸取的教训
“这是一个添加不需要 “修复” 但在与运营和业务要求同时执行本行动手册时必须了解的特定项目的地方。 `

## 已解决积压项目
-作为事件响应者，我需要记录在内部和外部的 AWS 上报路径

## 当前积压项目