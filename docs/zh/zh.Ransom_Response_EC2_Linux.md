# 事件响应行动手册：EC2 Linux/Unix 的赎金响应
本文档仅供参考。 它代表了截至本文档发布之日 Amazon Web Services (AWS) 提供的当前产品和实践，这些产品和做法如有更改，恕不另行通知。 客户有责任对本文档中的信息以及对 AWS 产品或服务的任何使用情况进行自己的独立评估，每种产品或服务都是 “按原样” 提供的，无论是明示还是暗示的担保。 本文档不创建 AWS、其附属公司、供应商或许可方的任何担保、陈述、合同承诺、条件或保证。 AWS 对客户的责任和责任受 AWS 协议的控制，本文档既不是 AWS 与其客户之间的任何协议的一部分，也不修改。

© 2024 Amazon 网络服务公司或其附属公司。 保留所有权利。 本作品根据知识共享署名 4.0 国际许可协议进行许可。

提供此 AWS 内容须遵守 http://aws.amazon.com/agreement 提供的 AWS 客户协议的条款或客户与 Amazon Web Services, Inc. 或 Amazon 网络服务 EMEA SARL 或两者之间的其他书面协议。

## 联系点

作者：`作者姓名`
批准者：`批准者姓名`
最后批准日期：

## 执行摘要
本行动手册概述了应对针对 EC2 实例的赎金攻击的流程。

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
[AWS Cloud Adoption Framework 安全角度] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* ** 指令 **
* ** 侦探 **
* ** 响应 **
* ** 预防性 **

! [图片] (/images/aws_caf.png)
* * *

### 响应步骤
1. [** 准备 **] 使用 AWS Config 查看配置合规性
2. [** 准备 **] 识别、记录和测试上报程序
3. [** 检测和分析 **] 使用 CloudWatch 指标确定数据是否已泄露
4. [** 检测和分析 **] 使用 vpcFlowLogs 识别来自外部 IP 地址的不当数据库访问
5. [** 包含 **] 立即隔离受影响的资源
6. [** 根除 **] 从网络中移除任何受损的系统。
7. [** 根除 **] 基于网络 IOC 强制执行 NACL 以防止进一步流量
8. [** 根除 **] 其他感兴趣的物品
9. [** RECOVERY**] 酌情执行恢复程序


*** 响应步骤遵循 [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] 中的事件响应生命周期（https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf）

! [图片] (/images/nist_life_cycle.png) ***

### 事件分类和处理
* ** 策略、技术和程序 **：赎金和数据销毁
* ** 类别 **：赎金攻击
* ** 资源 **：EC2
* ** 指标 **：网络威胁情报、第三方通知、Cloudwatch 指标
* ** 日志源 **：CloudTrail、CloudWatch、AWS Config
* ** 团队 **：安全运营中心 (SOC)、法医调查员、云工程

## 事件处理流程
### 事件响应流程有以下几个阶段：
* 准备
* 检测和分析
* 遏制和根除
* 恢复
* 事件后活动

## 准备
* 评估账户的安全状况，以识别和补救安全漏洞
* AWS 开发了一种新的开源 Self-Service Security Assessment (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) 工具，该工具为客户提供了时间点评估，以获得对其 AWS 安全状况的宝贵见解帐户
* 维护所有资源的完整资产清单，包括域控制器、Microsoft Windows EC2 实例、Microsoft Windows 服务器和数据库，以及与外部身份提供商的任何集成
* 使用诸如 [Amazon Inspector] 之类的实用程序对房东进行反复性漏洞分析 (https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/)
* 执行 EC2 实例的备份
* 考虑使用 [AWS Backup] (https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) 或 [AWS CloudEndure] (https://aws.amazon.com/cloudendure-disaster-recovery/)
* [使用文件历史记录备份文件] (https://support.microsoft.com/en-us/windows/file-history-in-windows-5de0e203-ebae-05ab-db85-d5aa0a199255)
* 验证备份并确保感染没有传播到备份中
* 定期备份重要文件。 使用 3-2-1 规则。 将数据的三个备份保存在两种不同的存储类型上，至少有一个异地备份
* 将最新更新应用于操作系统和应用程序
* 教育员工，以便他们能够识别社交工程和矛头网络钓鱼攻击
* 阻止已知的勒索软件文件类型
* 使用 [Systems Manager 和 Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/) 检查 EC2 实例是否包含任何常见漏洞和风险 (CVE)
* 在所有系统上启用恶意软件检测和防病毒软件-首选商业、订阅付费端点检测和响应 (EDR) 解决方案。
* 示例指南可以在如何安装和使用 Linux 恶意软件检测 (LMD) 以 ClamAV 作为防病毒引擎 (https://www.tecmint.com/install-linux-malware-detect-lmd-in-rhel-centos-and-fedora/)
* 加固面向互联网的资产，并确保它们具有最新的安全更新。 使用威胁和漏洞管理定期审核这些资产，以了解漏洞、错误配置和可疑活动。
* 实践最低特权原则并保持凭证卫生。 避免使用域范围内的管理员级服务帐户。 强制执行强大的随机、即时的本地管理员密码。
* 监控暴力尝试。 检查过度失败的身份验证尝试（/var/log/auth.log 或 /var/log/secure）/var/log/auth.log
* 监控清除事件日志
* 打开篡改保护功能以防止攻击者停止安全服务
* 确定高度特权的帐户在哪里登录并公开凭据。
* 使用终端安全客户端，例如 [Wazuh] (https://documentation.wazuh.com/current/getting-started/components/index.html)
* 使用文件完整性监视器（例如 [Tripwire] (https://github.com/Tripwire/tripwire-open-source) 来检测对关键文件的更改

### 使用 AWS Config 查看配置合规性：
1. 登录 AWS 管理控制台并通过 https://console.aws.amazon.com/config/ 打开 AWS Config 控制台
1. 在 AWS 管理控制台菜单中，验证区域选择器是否设置为支持 AWS Config 规则的区域。 有关受支持区域的列表，请参阅 Amazon Web 服务通用参考中的 AWS Config 区域和终端节点
1. 在导航窗格中，选择资源。 在资源库存页面上，您可以按资源类别、资源类型和合规性状态进行筛选。 如果适当，选择包括已删除的资 该表显示了资源类型的资源标识符以及该资源的资源合规性状态。 资源标识符可能是资源 ID 或资源名称
1. 从资源标识符列中选择资源
1. 选择资源时间轴按钮。 您可以按配置事件、合规性事件或 CloudTrail 事件进行筛选
1. 特别关注以下活动：
* ebs-in-backup-plan
* ebs-optimized-instance
* ebs-snapshot-public-restorable-check
* ec2-ebs-encryption-by-default
* ec2-imdsv2-check
* ec2-instance-detailed-monitoring-enabled
* ec2-instance-managed-by-systems-manager
* ec2-instance-multiple-eni-check
* ec2-instance-no-public-ip
* 附加了 ec2-instance-profile-attached
* ec2-managedinstance-applications-blacklisted
* ec2-managedinstance-applications-required
* ec2-managedinstance-association-compliance-status-check
* ec2-managedinstance-inventory-blacklisted
* ec2-managedinstance-patch-compliance-status-check
* ec2-managedinstance-platform-check
* ec2-security-group-attached-to-eni
* ec2-stopped-instance
* ec2-volume-inuse-check

## 上报程序
-“我需要就什么时候进行 EC2 取证作出业务决定 `
-“谁在监控日志/警报，接收日志/警报并对其采取行动？ `
-“发现警报后谁会收到通知？ `
-“什么时候公共关系和法律参与这一过程？ `
-“你什么时候联系 AWS Support 寻求帮助？ `

## 检测和分析
### [使用 CloudWatch Metrics] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html)
寻找数据泄露 “尖峰”。 攻击者可能会执行数据销毁并留下赎金票据，在这些情况下，与恶意行为者合作没有恢复数据的机会
1. 在 https://console.aws.amazon.com/cloudwatch/ 打开 CloudWatch 控制台
1. 在导航窗格中，选择量度，然后选择所有量度
1. 在所有指标选项卡上，选择实例部署在其中的区域
1. 在所有指标选项卡上，输入搜索词 `NetworkPacketsOut` 然后按 Enter
1. 选择搜索结果之一以查看指标
1. 要绘制一个或多个指标的图表，请选中每个指标旁边的复选框。 要选择所有指标，请选中表格标题行中的复选框
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

## 遏制
### 立即隔离受影响的资源
** 注意 **：确保您拥有请求上报和批准的流程，以隔离资源，以确保首先就隔离将如何影响当前运营和收入来源进行业务影响分析。
1. 确定实例是属于 Auto Scaling 组还是附加到负载均衡器
* AutoScaling 组：从组中分离实例
* 弹性负载均衡器：从 ELB 中注销实例，然后从目标组中删除实例
1. 创建一个阻止所有入口和出口流量的 * 新 * 安全组；确保删除出口流量的默认 `允许 all` 规则
1. 将 *new* 安全组附加到受影响的实例

## 根除
### 从网络中移除任何受损的系统。
* 遵循监管要求或公司内部政策以确定是否需要 EC2 实例的取证
* 如果需要对实例进行取证或需要恢复数据，请遵循 [Playbook：EC2 取证] (. /docs/EC2_Fransics.md)

### 基于网络 IOC 强制执行 NACL 以防止进一步的流量
1. 打开 [Amazon VPC 控制台]（https://console.aws.amazon.com/vpc/）
1. 在导航窗格中，选择 Network ACLs
1. 选择创建 Network ACL
1. 在创建 Network ACL 对话框中，可以选择命名您的网络 ACL，然后从 VPC 列表中选择 VPC 的 ID。 然后选择是，创建
1. 在详细信息窗格中，根据需要添加的规则类型，选择 “入站规则” 或 “出站规则” 选项卡，然后选择 “编辑”
1. 在规则 # 中，输入规则编号（例如 100）。 网络 ACL 中必须尚未使用规则编号。 我们按顺序处理规则，从最低的数字开始
* 我们建议您在规则编号（例如 100、200、300）之间保留空白，而不是使用顺序编号（101、102、103）。 这使得添加新规则变得更容易，而无需重新编号现有规则
1. 从 “类型” 列表中选择一个规则。 例如，要为 HTTP 添加规则，请选择 HTTP。 要添加 allow all TCP 流量的规则，请选择所有 TCP。 对于其中一些选项（例如 HTTP），我们为您填写端口。 要使用未列出的协议，请选择自定义协议规则
1. （可选）如果要创建自定义协议规则，请从协议列表中选择协议的编号和名称。 有关更多信息，请参阅 IANA 协议编号列表
1. （可选）如果您选择的协议需要端口号，请输入以连字符分隔的端口号或端口范围（例如，49152-65535）。
1. 在 “源” 或 “目标” 字段中（取决于这是入站规则还是出站规则），输入规则应用的 CIDR 范围
1. 从 “允许/拒绝” 列表中，选择允许以允许指定的流量，或者选择拒绝拒绝指定的流量
1. （可选）要添加另一个规则，请选择添加另一个规则，然后根据需要重复之前的步骤
1. 完成后，选择 “保存”
1. 在导航窗格中，选择 Network ACLs，然后选择网络 ACL
1. 在详细信息窗格的子网关联选项卡上，选择编辑。 选中要与网络 ACL 关联的子网对应的 “关联” 复选框，然后选择 “保存”

### 其他感兴趣的物品
* 理查德·霍恩（Richard Horne）的 [No Strings on Me: Linux and Ransomware]（https://www.sans.org/reading-room/whitepapers/tools/strings-me-linux-ransomware-39870）中的表 1 确定了要监控的多个指标，包括：
* 可能在 /tmp 目录内创建和运行进程
* 以前有外部网络连接请求的终端上从未见过的新过程
* 在同一目录树中多次重命名文件
* 在子进程之前终止父进程的大型进程树
* 权限升级或尝试获得 sudo 访问权限
* 使用字符串 cmd 尝试在感染之前或感染期间对通信进行编码
* 使用熵生成的文件名或快速连续的文件名
* 修改启动选项
* 尝试使用找到熵的 DNS 名称或直接使用裸 IP 进行通信
* 在非基于用户的主目录中创建 .sh 文件
* 使用 chmod cmd 将可执行文件更改为过于宽容的权限
* 更改分布 Yum 或 Apt 仓库
* 快速连续的目录枚举
* 短期或持续时间内存使用量高
* 重点放在典型的日常运营范围之外的异常值和事件
* 外部和内部网络通信
* 使用 wget 或 curl cmds
* 枚举共享 Web、数据库或文件存储目录树
* 用奇数文件扩展名创建的文件
* 在单个目录中进行多次修改
* 多个目录中同一文件的多个副本
* 可能要求加密库
* 使用通配符或未经确认从目录中删除文件
* 使用 chmod 与通配符（或 chmod 777）

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
* [吊销临时证书] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time)。 也可以通过删除 IAM 用户来吊销临时证书。 注意：删除 IAM 用户可能会影响生产工作负载，应谨慎完成 * 在勒索软件攻击或数据损坏之前，使用 CloudEndure 灾难恢复选择最新的恢复点，以恢复 AWS 上的工作负载
* 如果使用备用数据备份策略，请验证备份没有被感染并从勒索软件事件发生之前的最后一个计划事件中恢复
* 从受信任的 AMI 创建新的 EC2 实例
* 使用 CloudEndure 灾难恢复在勒索软件攻击或数据损坏之前选择最新的恢复点，以恢复 AWS 上的工作负载
* 如果使用备用数据备份策略，请验证备份没有被感染并从勒索软件事件发生之前的最后一个计划事件中恢复

## 吸取的教训
“这里可以添加特定于贵公司的物品，这些物品不一定需要 “修复”，但在与运营和业务要求同时执行本行动手册时也很重要。 `

## 已解决积压项目
-作为事件响应者，我需要一本运行手册来进行 EC2 取证
-作为事件响应者，我需要就应该何时进行 EC2 取证作出业务决策
-作为事件响应者，我需要在所有已启用的区域启用日志记录，无论使用意图如何
-作为事件响应者，我需要能够检测我现有 EC2 实例上的加密挖矿

## 当前积压项目