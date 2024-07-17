# 事件响应行动手册：RDS 的赎金响应
本文档仅供参考。 它代表了截至本文档发布之日 Amazon Web Services (AWS) 提供的当前产品和实践，这些产品和做法如有更改，恕不另行通知。 客户有责任对本文档中的信息以及对 AWS 产品或服务的任何使用情况进行自己的独立评估，每种产品或服务都是 “按原样” 提供的，无论是明示还是暗示的担保。 本文档不创建 AWS、其附属公司、供应商或许可方的任何担保、陈述、合同承诺、条件或保证。 AWS 对客户的责任和责任受 AWS 协议的控制，本文档既不是 AWS 与其客户之间的任何协议的一部分，也不修改。

© 2024 Amazon 网络服务公司或其附属公司。 保留所有权利。 本作品根据知识共享署名 4.0 国际许可协议进行许可。

提供此 AWS 内容须遵守 http://aws.amazon.com/agreement 提供的 AWS 客户协议的条款或客户与 Amazon Web Services, Inc. 或 Amazon 网络服务 EMEA SARL 或两者之间的其他书面协议。

## 联系点

作者：`作者姓名`
批准者：`批准者姓名`
最后批准日期：

## 执行摘要
本行动手册概述了针对 Amazon 关系数据库服务 (RDS) 的赎金攻击的响应流程

有关更多信息，请查看 [AWS 安全事件响应指南] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

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
1. [** 准备 **] 使用 AWS Config 查看配置合规性
2. [** 准备 **] 识别、记录和测试上报程序
3. [** 包含 **] 立即隔离受影响的资源
5. [** 检测和分析 **] 使用 CloudWatch 指标确定数据是否已泄露
6. [** 检测和分析 **] 使用 vpcFlowLogs 识别来自外部 IP 地址的不当数据库访问
7. [** RECOVERY**] 酌情执行恢复程序

*** 响应步骤遵循 [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] 中的事件响应生命周期（https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf）

! [图片] (/images/nist_life_cycle.png) ***

### 事件分类和处理
* ** 策略、技术和程序 **：赎金和数据销毁
* ** 类别 **：赎金攻击
* ** 资源 **：RDS
* ** 指标 **：网络威胁情报、第三方通知、Cloudwatch 指标
* ** 日志源 **：RDS Database Log Files、S3 Access Logs、CloudTrail、CloudWatch、AWS Config
* ** 团队 **：安全运营中心 (SOC)、法医调查员、云工程

## 事件处理流程
### 事件响应流程有以下几个阶段：
* 准备
* 检测和分析
* 遏制和根除
* 恢复
* 事件后活动

## 准备
* 评估你的安全态势以识别和补救安全漏洞
* AWS 开发了一种新的开源 Self-Service Security Assessment (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) 工具，该工具为客户提供了时间点评估，以获得对其 AWS 安全状况的宝贵见解账户。
* 维护所有资源的完整资产清单，包括服务器、网络设备、网络/文件共享和开发人员机器
* 考虑使用 [AWS Config] (https://aws.amazon.com/config/)，这是一项使您能够评估、审计和评估 AWS 资源配置的服务
* 考虑实施 [AWS GuardDuty] (https://aws.amazon.com/guardduty/) 以持续监控恶意活动和未经授权的行为，以保护存储在 Amazon RDS 中的 AWS 账户、工作负载和数据
* [基于 Amazon VPC 服务在虚拟私有云 (VPC) 中运行数据库实例] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.html)，以实现最大限度的网络访问控制
* 使用 AWS 身份和访问管理 (IAM) 策略分配权限，以确定允许谁管理 Amazon RDS 资源。 例如，您可以使用 IAM 来确定允许谁创建、描述、修改和删除数据库实例、标记资源或修改安全组。
* 使用安全组控制哪些 IP 地址或 Amazon EC2 实例可以连接到数据库实例上的数据库。 首次创建数据库实例时，其防火墙会阻止任何数据库访问，除非通过关联安全组指定的规则进行访问。
* [对运行 MySQL、MariaDB、PostgreSQL、Oracle 或 Microsoft SQL Server 数据库引擎的数据库实例使用安全套接字层 (SSL) 或传输层安全性 (TLS) 连接] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html)
* [使用 Amazon RDS 加密] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Encryption.html) 保护静态数据库实例和快照。 Amazon RDS 加密使用行业标准 AES-256 加密算法对托管数据库实例的服务器上的数据加密
* [使用网络加密] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.NetworkEncryption.html) 和 [透明数据加密] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.AdvSecurity.html) 使用 Oracle 数据库实例
* 使用数据库引擎的安全功能来控制谁可以登录数据库实例上的数据库。 这些功能就像数据库在本地网络上一样工作。
* 强烈建议使用基于主机的第三方入侵检测系统 (HIDS) 解决方案（例如 OSSEC、Tripwire、Wazuh、[Amazon Inspector] (https://aws.amazon.com/inspector/))
* [Amazon RDS 中的安全性]（https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.html）中提供了其他参考和步骤

### 使用 AWS Config 查看配置合规性：
1. 登录 AWS 管理控制台并在 https://console.aws.amazon.com/config/ 打开 AWS Config 控制台
1. 在 AWS 管理控制台菜单中，验证区域选择器是否设置为支持 AWS Config 规则的区域。 有关受支持区域的列表，请参阅 Amazon Web 服务通用参考中的 AWS Config 区域和终端节点
1. 在导航窗格中，选择资源。 在资源库存页面上，您可以按资源类别、资源类型和合规性状态进行筛选。 如果适当，选择包括已删除的资 该表显示了资源类型的资源标识符以及该资源的资源合规性状态。 资源标识符可能是资源 ID 或资源名称
1. 从资源标识符列中选择资源
1. 选择资源时间轴按钮。 您可以按配置事件、合规性事件或 CloudTrail 事件进行筛选
1. 特别关注以下活动：
* rds-automatic-minor-version-upgrade-enabled
* rds-cluster-deletion-protection-enabled 了
* rds-cluster-iam-authentication-enabled
* rds-cluster-multi-az-enabled
* rds-enhanced-monitoring-enabled
* rds-instance-deletion-protection-enabled
* rds-instance-iam-authentication-enabled
* rds-instance-public-access-check
* rds-in-backup-plan
* rds-logging-enabled
* rds-multi-az-support
* rds-snapshots-public-prohibited
* rds-snapshot-encrypted
* rds-storage-encrypted

## 上报程序
-“我需要就什么时候进行 EC2 取证作出业务决定 `
-“谁在监控日志/警报，接收日志/警报并对其采取行动？ `
-“发现警报后谁会收到通知？ `
-“什么时候公共关系和法律参与这一过程？ `
-“你什么时候联系 AWS Support 寻求帮助？ `

## 检测和分析
* [AWS GuardDuty 机器学习] (https://aws.amazon.com/blogs/security/how-you-can-use-amazon-guardduty-to-detect-suspicious-activity-within-your-aws-account/) 可以检测可疑行为，包括作为数据泄露尝试的一部分生成亚马逊关系数据库服务 (Amazon RDS) 快照
* 查看 EC2 操作系统和应用程序日志是否存在不当登录、安装未知软件或是否存在无法识别的文件

### [使用 CloudWatch 指标] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html)
寻找数据泄露 “尖峰”。 攻击者可能会执行数据销毁并留下赎金票据，在这些情况下，与恶意行为者合作没有恢复数据的机会
1. 在 https://console.aws.amazon.com/cloudwatch/ 打开 CloudWatch 控制台
1. 在导航窗格中，选择量度，然后选择所有量度
1. 在所有指标选项卡上，选择实例部署在其中的区域
1. 在所有指标选项卡上，输入搜索词 `NetworkPacketsOut` 然后按 Enter
1. 选择搜索结果之一以查看指标
1. 要绘制一个或多个指标图表，请选中每个指标旁边的复选框。 要选择所有指标，请选中表格标题行中的复选框
1. （可选）要更改图表的类型，请选择图表选项。 然后，您可以在折线图、堆积面积图、条形图、饼图或数字之间进行选择
1. （可选）要添加显示指标预期值的异常检测范围，请选择指标旁边的 “操作” 下的异常检测图标

### 使用 VPCFlowLogs 识别来自外部 IP 地址的不当数据库访问
1. 使用 [AWS Security Analytics Bootstrap 程序] (https://github.com/awslabs/aws-security-analytics-bootstrap) 分析日志数据
1. 获取摘要，其中包含往返特定 IP 的所有记录中的每条 src_ip、src_port、dst_port 四边形的字节数
``
从 vpcflow 中选择源地址、目标地址、源端口、目标端口、总和（numbytes）作为 byte_count
哪里（源地址 = '192.0.2.1' 或目的地地址 = '192.0.2.1'）
和日期 _ 分区 >= '2020/07/01'
和日期 _ 分区 <= '2020/07/31'
和账户分区 = '111122223333'
和区域分区（'us-east-1'、'us-east-2'、'us-west-2'、'us-west-2'、'us-west-2'）
按源地址、目标地址、源端口、目标端口进行分组
按 byte_count 订购 DESC
``
1. [vpcflow_demo_queries.sql]（https://github.com/awslabs/aws-security-analytics-bootstrap/blob/main/AWSSecurityAnalyticsBootstrap/sql/dml/analytics/vpcflow/vpcflow_demo_queries.sql）中提供了其他示例查询


## 恢复
* 建议不要支付赎金
* 支付赎金是一场赌博，罪犯在收到付款后是否会兑现交易
* 如果不存在数据备份，那么您应该进行成本效益分析，并权衡数据/声誉损失与向攻击者付款的价值
* 如果你选择支付赎金，你可以直接让攻击者继续对你的公司或其他人进行操作
* 访问 https://www.nomoreransom.org/ 以确定是否可用于受感染的数据的恶意软件变体的解密
* [删除或轮换 IAM 用户密钥] (https://console.aws.amazon.com/iam/home#users) 和 [Root 用户密钥] (https://console.aws.amazon.com/iam/home#security_credential)；如果您无法识别已公开的特定密钥或密钥，则可能希望轮换账户中的所有密钥
* [删除未经授权的 IAM 用户] (https://console.aws.amazon.com/iam/home#users。)
* [删除未经授权的政策] (https://console.aws.amazon.com/iam/home#/policies)
* [删除未经授权的角色] (https://console.aws.amazon.com/iam/home#/roles)
* [吊销临时证书] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time)。 也可以通过删除 IAM 用户来吊销临时证书。 注意：删除 IAM 用户可能会影响生产工作负载，应谨慎完成
* 使用 CloudEndure 灾难恢复在勒索软件攻击或数据损坏之前选择最新的恢复点，以恢复 AWS 上的工作负载
* 如果使用备用数据备份策略，请验证备份没有被感染并从勒索软件事件发生之前的最后一个计划事件中恢复
* 删除任何未识别或未经授权的公共快照或数据库
* 删除受损的数据库并创建新的 RDS 数据库
* 确定有权访问数据库的 EC2 实例并调查这些实例

## 吸取的教训
“这里可以添加特定于贵公司的物品，这些物品不一定需要 “修复”，但在与运营和业务要求同时执行本行动手册时也很重要。 `

## 已解决积压项目
-作为事件响应者，我需要一本运行手册来进行 EC2 取证
-作为事件响应者，我需要就应该何时进行 EC2 取证作出业务决策
-作为事件响应者，我需要在所有已启用的区域启用日志记录，无论使用意图如何
-作为事件响应者，我需要能够检测我现有 EC2 实例上的加密挖矿

## 当前积压项目