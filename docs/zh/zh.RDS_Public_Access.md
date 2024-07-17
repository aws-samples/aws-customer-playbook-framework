# 事件响应行动手册：公共资源曝光-RDS
本文档仅供参考。 它代表了截至本文档发布之日 Amazon Web Services (AWS) 提供的当前产品和实践，这些产品和做法如有更改，恕不另行通知。 客户有责任对本文档中的信息以及对 AWS 产品或服务的任何使用情况进行自己的独立评估，每种产品或服务都是 “按原样” 提供的，无论是明示还是暗示的担保。 本文档不创建 AWS、其附属公司、供应商或许可方的任何担保、陈述、合同承诺、条件或保证。 AWS 对客户的责任和责任受 AWS 协议的控制，本文档既不是 AWS 与其客户之间的任何协议的一部分，也不修改。

© 2024 Amazon 网络服务公司或其附属公司。 保留所有权利。 本作品根据知识共享署名 4.0 国际许可协议进行许可。

提供此 AWS 内容须遵守 http://aws.amazon.com/agreement 提供的 AWS 客户协议的条款或客户与 Amazon Web Services, Inc. 或 Amazon 网络服务 EMEA SARL 或两者之间的其他书面协议。

## 联系点

作者：`作者姓名`\
批准者：`批准者姓名`\
最后批准日期：

## 执行摘要
本行动手册概述了识别公共资源所有者的过程，确定谁可能在公开时访问了这些资源，确定撤销对资源的访问权限的影响，以及确定公共可访问性的根本原因。

## 潜在的妥协指标
-来自 AWS 服务仪表板的公共访问警告
-CloudTrail 事件名称 “可公开访问”
-安全研究员关于公开访问资源的通知
-从公共互联网协议 (IP) 地址中删除资源

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
1. [** 准备 **] 创建资产库存
2. [** 准备 **] 创建 RDS 实例清单
3. [** 准备 **] 建立 RDS 安全性和日志记录检查
4. [** 准备 **] 实施培训计划来识别和评估 RDS 访问和日志分析
5. [** 准备 **] 识别、记录和测试上报程序 [** 检测和分析 **]
6. [** 检测和分析 **] 执行实例检查
7. [** 检测和分析 **] 查看 RDS 公共资源的 CloudTrail
8. [** 检测和分析 **] 查看 VPC Flow Logs
9. [** 检测和分析 **] 查看 RDS 端点/基于主机的日志
10. [** 包含 **] 包含 RDS 公开曝光
11. [**ERADICATION**] 删除任何无法识别或未经授权的公共快照或数据库
12. [** 准备 **] 其他预防措施：RDS 安全检查
13. [** 准备 **] 其他预防措施：安全控制策略-RDS 加密
14. [** 准备 **] 其他预防措施：AWS Config
15. [** 准备 **] 其他预防措施：总体安全状况

*** 响应步骤遵循 [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] 中的事件响应生命周期（https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf）

! [图片] (/images/nist_life_cycle.png) ***

### 事件分类和处理
* ** 策略、技术和程序 **：AWS 服务公共访问
* ** 类别 **：公共访问
* ** 资源 **：RDS
* ** 指标 **：网络威胁情报、第三方通知、Cloudwatch 指标
* ** 日志源 **：RDS Database Log Files、CloudTrail、CloudWatch
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

此工具提供了客户环境中当前安全状态的快速快照。 作为替代方案，[AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) 提供了自动合规性扫描，并可以 [与 Prowler 集成] (https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### 资产库存
确定所有现有资源并拥有更新的资产库存清单以及每个资源的拥有者

#### RDS 实例清单
-要清点 RDS 实例，请使用 AWS API [描述数据库实例] (https://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-instances.html) 列出给定区域中所有实例的名称：`aws rds 描述数据库实例 — 区域 us-east-1 — 查询 'dbInstance [*]。 [dbInstance标识符，ReadReplicadbInstance 标识符] '`

#### RDS 安全性和日志记录检查
-检查 RDS 实例存储是否已加密：`。 /prowler-c extra735`
-检查 RDS 实例是否启用了备份：`。 /prowler-c extra739`
-检查 RDS 实例是否与 CloudWatch 日志集成：`。 /prowler-c extra747`
-确保 RDS 实例启用次要版本升级：`。 /prowler-c extra7131`
-检查 RDS 实例是否已启用 [增强监控] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Monitoring.OS.html)：`。 /prowler-c extra7132`
-RDS 安全检查：`。 /prowler-g group13`

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

### RDS 实例检查

#### AWS Config
AWS Config 有几个 [用于评估 RDS 实例的托管规则] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
* rds-automatic-minor-version-upgrade-enabled
* rds-cluster-deletion-protection-enabled
* rds-cluster-iam-authentication-enabled
* rds-cluster-multi-az-enabled
* rds-enhanced-monitoring-enabled
* rds-instance-deletion-protection-enabled
* rds-instance-iam-authentication-enabled
* rds-instance-public-access-check
* rds-in-backup-plan
* rds-logging-enabled
* rds-multi-az-support
* rds-resources-protected-by-backup-plan
* rds-snapshots-public-prohibited
* rds-snapshot-encrypted
* rds-storage-encrypted
#### Prowler
-确保没有公共可访问 RDS 实例：`。 /prowler-c 额外 78`
-检查 RDS 快照和群集快照是否公开：`。 /prowler-c extra723`
-确定组织和账户中与外部实体共享的资源（例如 Amazon S3 存储桶或 IAM 角色）中与外部实体共享的资源：`。 /prowler-c extra769`
-查找暴露在互联网上的资源：`。 /prowler-g group17`

## 上报程序
-“谁在监控日志/警报，接收日志/警报并对其采取行动？ `
-“发现警报后谁会收到通知？ `
-“什么时候公共关系和法律参与这一过程？ `
-“你什么时候联系 AWS Support 寻求帮助？ `

## 分析
强烈建议将日志导出到安全事件事件管理 (SIEM) 解决方案（例如 Splunk、ELK stack 等），以帮助查看和分析各种日志，以进行更完整的攻击时间表分析。

### CloudTrail：RDS 公共
1. 导航到你的 [CloudTrail 控制面板]（https://console.aws.amazon.com/cloudtrail）
1. 在左边栏中选择 “事件历史记录”
1. 在下拉菜单中，从 “只读” 更改为 “事件名称”
1. 查看 CloudTrail 日志以获取事件名称 “修改 DBInstance`、“修改 DBSnapshot属性” 或 “修改 DBCluster Snapshot属性”，然后查找来自公有 IP 地址的 “可公开访问” 事件的值

### VPC Flow Logs
VPC Flow Logs 是一项功能，使您能够捕获有关进出 VPC 中网络接口的 IP 流量的信息。 这对于在 CloudTrail 中发现的 IP 地址非常有用，以确定与任何公共资源的外部连接类型。

有关更多信息和步骤，包括使用 Athena 进行查询，请参阅 [VPC Flow Logs 的 AWS 文档] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html)。 建议将 Athena 分析纳入单独的手册中，并与其他相关项目链接。

### 端点/基于主机
-查看 EC2 操作系统和应用程序日志是否存在不当登录、安装未知软件或是否存在无法识别的文件。

-强烈建议使用基于主机的第三方入侵检测系统 (HIDS) 解决方案（例如 OSSEC、Tripwire、Wazuh、[Amazon Inspector]（https://aws.amazon.com/inspector/）等）

## 遏制

### RDS 公开曝光
当您在 VPC 内启动数据库实例时，该数据库实例具有用于 VPC 内流量的私有 IP 地址。 此私有 IP 地址不可公开访问。 您可以使用公共访问选项来指定数据库实例除了私有 IP 地址之外是否还具有公有 IP 地址。

下图显示了 “附加连接配置” 部分中的 “公共访问” 选项。 要设置该选项，请打开连接部分中的其他连接配置部分。

! [图片] (/images/VPC-example.png)

## 根除

### RDS 未经授权/无法识别的资源
删除任何未识别或未经授权的公共快照或数据

1. 登录 AWS 管理控制台并通过 https://console.aws.amazon.com/rds/ 打开 Amazon RDS 控制台
1. 在导航窗格中，选择快照。
1. 此时将显示手动快照列表。
1. 选择要删除的数据库快照。
1. 对于操作，请选择删除快照。
1. 在确认页面上选择删除。

## 恢复
与为根除所列的程序相同

## 预防性行动

### RDS 安全检查
-RDS 安全检查：`。 /prowler-g group13`

### 安全控制策略：RDS 加密
[强制执行强制性 RDS 加密] (https://medium.com/@cbchhaya/aws-scp-to-mandate-rds-encryption-6b4dc8b036a)

### AWS Config
[AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) 有多个自动化规则来防止公共访问，包括 [rds-snapshots-public-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshots-public-prohibited.html)

### 整体安全态势
针对环境执行 [Self-Service Security Assessment]（https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/），以进一步识别本手册中未发现的其他风险和其他潜在的公众风险。

## 吸取的教训
“这里可以添加特定于贵公司的物品，这些物品不一定需要 “修复”，但在与运营和业务要求同时执行本行动手册时也很重要。 `

## 已解决积压项目
-作为事件响应者，我需要一本关于如何减少错误公开的资源的运行手册
-作为事件响应者，我需要能够检测公共资源（AMI、EBS 卷、ECR 回购等）
-作为事件响应者，我需要知道哪些角色能够在 AWS 中进行重大更改
-作为事件响应者，我需要确保所有快照（RDS、EBS、ECR）都需要加密

## 当前积压项目