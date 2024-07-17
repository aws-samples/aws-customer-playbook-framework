# 事件响应行动手册：EC2 取证
本文档仅供参考。 它代表了截至本文档发布之日 Amazon Web Services (AWS) 提供的当前产品和实践，这些产品和做法如有更改，恕不另行通知。 客户有责任对本文档中的信息以及对 AWS 产品或服务的任何使用情况进行自己的独立评估，每种产品或服务都是 “按原样” 提供的，无论是明示还是暗示的担保。 本文档不创建 AWS、其附属公司、供应商或许可方的任何担保、陈述、合同承诺、条件或保证。 AWS 对客户的责任和责任受 AWS 协议的控制，本文档既不是 AWS 与其客户之间的任何协议的一部分，也不修改。

© 2024 Amazon 网络服务公司或其附属公司。 保留所有权利。 本作品根据知识共享署名 4.0 国际许可协议进行许可。

提供此 AWS 内容须遵守 http://aws.amazon.com/agreement 提供的 AWS 客户协议的条款或客户与 Amazon Web Services, Inc. 或 Amazon 网络服务 EMEA SARL 或两者之间的其他书面协议。

[[_TOC_]]

## 联系点

作者：`作者姓名`
批准者：`批准者姓名`
最后批准日期：

## 执行摘要
本行动手册概述了对被识别为恶意或具有值得进一步调查的妥协指标的 EC2 实例执行取证的过程。

最佳做法是在事件发生的同一 AWS 区域执行调查，而不是将数据复制到另一个 AWS 区域。 我们建议采用这种做法，主要是因为在区域之间传输数据需要额外的时间。 对于您选择运营的每个 AWS 区域，请确保您的事件响应流程和响应者都遵守相关的数据隐私法。 如果您确实需要在 AWS 区域之间移动数据，请考虑在司法管辖区之间移动数据的法律影响。 将数据保存在同一个国家管辖范围内通常是最佳做法。

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
1. [** 准备 **] 在执行取证程序之前确定最佳实践
2. [** 准备 **] 创建取证 VPC
3. [** 准备 **] 为 SSM 添加 VPC Endpoint
4. [** 准备 **] 为取证 VPC 创建网络访问控制列表
5. [** 准备 **] 创建取证 EC2 黄金 AMI
6. [** 准备 **] 实施培训计划来识别和评估云取证能力
7. [** 准备 **] 识别、记录和测试上报程序
8. [** 检测和分析 **] 创建 Amazon EBS Snapshot
9. [** 检测和分析 **] 共享 Amazon EBS Snapshot
10. [** 检测和分析 **] 使用控制台共享未加密的快照
11. [** 检测和分析 **] 使用与您私下共享的未加密快照
12. [** 检测和分析 **] 使用控制台共享加密快照
13. [** 检测和分析 **] 使用与您共享的加密快照
14. [** 检测和分析 **] 使用取证 Golden AMI 创建取证 EC2
15. [** 检测和分析 **] 将可疑的 EBS 卷挂载到取证 EC2
16. [** 检测和分析 **] 执行取证流程
17. [** 包含 **] 立即隔离受影响的资源
18. [** RECOVERY**] 酌情执行恢复过程

*** 响应步骤遵循 [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] 中的事件响应生命周期（https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf）

! [图片] (/images/nist_life_cycle.png) ***

### 事件分类和处理
* ** 策略、技术和程序 **：工具：取证
* ** 类别 **：取证
* ** 资源 **：EC2、VPC
* ** 指标 **：网络威胁情报、第三方通知、Cloudwatch 指标、AWS Shield
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
### [最佳实践] (https://d1.awsstatic.com/Marketplace/scenarios/security/SEC_11_TSB_Final.pdf)
* 在每个地区打开 AWS CloudTrail
不要将 CloudTrail 日志记录限制在您正在使用的 AWS 区域。 在每个 AWS 区域启用 CloudTrail 将允许您更轻松地识别异常行为，例如从组织不使用的 AWS 区域预配置 AWS 服务。

* 保护日志信息
验证系统组件的审计跟踪是否已启用并处于活动状态，并定期对其进行备份。 考虑将它们存储在需要不同凭据的位置，这样攻击者就无法删除可能提供恶意活动证据的日志文件。

* 永远不要忽略 AWS 滥用通信
当提出滥用案件时，AWS 将自动向您注册的电子邮件地址发送一封电子邮件。 立即回复，并考虑设置专用的电子邮件回复地址。 您应对安全事件的速度越快，就越能限制其对业务的影响。 最好在由多个利益相关者组成的账户中有一个 [安全备用联系人的通讯组列表]（https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-account-payment.html#manage-account-payment-alternate-contacts），这样消息就不会被 “卡住” 在一个单一的内容员工的收件箱（例如休假、解雇、生病等）。

### 创建取证 VPC
** 注意 **：建议在可能需要根据生产资源进行取证的每个区域内设置取证 VPC。

1. 打开 [Amazon VPC 控制台]（https://console.aws.amazon.com/vpc/）
1. 在导航窗格中，选择您的 VPC，创建 VPC。
1. 根据需要指定以下 VPC 详细信息。
* 名称标签：可选地为您的 VPC 提供名称。 这样做会创建一个带有 Name 键和您指定的值的标签。
* IPv4 CIDR 块：为 VPC 指定 IPv4 CIDR 块。 您可以指定的最小 CIDR 块是 /28，最大的是 /16。 我们建议您从 RFC 1918 中指定的私有（非公开路由）IP 地址范围指定一个 CIDR 块；例如 10.0.0.0/16 或 192.168.0.0/16 或 192.168.0.0/16。
* 注意：您可以指定一系列可公开路由的 IPv4 地址。 但是，我们目前不支持从 VPC 中公开路由的 CIDR 块直接访问互联网。
* 如果启动到范围从 224.0.0.0 到 255.255.255.255（D 类和 E 类 IP 地址范围）的 VPC 中，Windows 实例将无法正确启动。
* IPv6 CIDR 块：可以选择将 IPv6 CIDR 块与您的 VPC 关联起来。 选择以下选项之一，然后选择选择 CIDR：
* Amazon 提供的 IPv6 CIDR 块：从亚马逊的 IPv6 地址池请求 IPv6 CIDR 块。 对于网络边界组，选择 AWS 从中发布 IP 地址的组。
* 我拥有的 IPv6 CIDR：(BYOIP) 从你的 IPv6 地址池中分配 IPv6 CIDR 块。 对于池，选择要从中分配 IPv6 CIDR 块的 IPv6 地址池。
* 租赁：选择租赁选项。
* 选择专用以确保在此 VPC 中启动的实例是专用租赁实例，而不管启动时指定的租赁属性如何。
* 选择默认值以确保在此 VPC 中启动的实例使用启动时指定的租赁属性。
*（可选）添加或删除标签。
* 选择创建。

### [为 SSM 添加 VPC Endpoint] (https://aws.amazon.com/premiumsupport/knowledge-center/ec2-systems-manager-vpc-endpoints/)

1. 验证实例上是否安装了 SSM 代理。
2. 为 Systems Manager 创建 AWS 身份和访问管理 (IAM) 实例配置文件。 您可以创建新角色，也可以向现有角色添加所需的权限。
3. 将 IAM 角色附加到您的私有 EC2 实例。
4. 打开 Amazon EC2 控制台，然后选择您的实例。 在描述选项卡上，记下 VPC ID 和 Subnet ID。
5. 为 Systems Manager 创建 VPC 终端节点。
* 对于服务名称，选择 com.amazonaws。 [区域] .ssm（例如，com.amazonaws.us-east-1.ssm）。 有关地区代码的完整列表，请参阅可用区域。
* 对于 VPC，请为您的实例选择 VPC ID。
* 对于 Subnets，请在您的 VPC 中选择 Subnet ID。 要获得高可用性，请从区域内的不同可用区中至少选择两个子网。
* 注意：如果您在同一可用区中有多个子网，则无需为额外的子网创建 VPC endpoints。 同一可用区内的任何其他子网都可以访问和使用该接口。
* 对于启用 DNS 名称，请为此终端节点选择启用。 有关更多信息，请参阅接口终端节点的私有 DNS。
* 对于安全组，请选择现有安全组，或创建一个新安全组。 安全组必须允许来自 VPC 中与服务通信的资源的入站 HTTPS（端口 443）流量。
* 如果您创建了新安全组，请打开 VPC 控制台，选择安全组，然后选择新的安全组。 在入站规则选项卡上，选择编辑入站规则。 添加包含以下详细信息的规则，然后选择保存规则：
* 对于 “类型”，选择 HTTPS。
* 对于来源，请选择您的 VPC CIDR。 对于高级配置，您可以允许 EC2 实例使用特定子网的 CIDR。
* 请注意安全组 ID。 您将将此 ID 与其他终端节点一起使用。
6. 为 AWS Systems Manager 创建 VPC 接口终端节点策略。
* 重复步骤 5 并进行以下更改：对于服务名称，请选择 com.amazonaws。 [区域] .ec2 消息。
* 重复步骤 5 并进行以下更改：对于服务名称，请选择 com.amazonaws。 [区域] .ssmmessages。 如果要使用 Session Manager，必须执行此操作。

### 为取证 VPC 创建网络访问控制列表
此过程将隔离 VPC，因此流量既不能进入也不能从中包含的实例离开。

1. 打开 [Amazon VPC 控制台]（https://console.aws.amazon.com/vpc/）
1. 在导航窗格中，选择安全性、Network ACLs。
1. 选中取证 VPC 旁边的复选框
1. 在右上角选择下拉菜单操作并编辑入站规则
1. 将规则 100 从 “所有流量” 更改为 “SSH”
1. 选择保存更改
1. 对出站规则重复这些步骤

### 创建取证 EC2 黄金 AMI
1. 打开 [Amazon EC2 控制台]（https://console.aws.amazon.com/ec2/）
1. 选择 “启动实例”
1. 在搜索栏中键入 `Ubuntu` 然后按回车键
1. 选择 `Ubuntu 服务器 18.04 LTS
1. 选择实例类型
* 注意：出于取证目的，建议选择 M5 类型的实例
1. 选择 “下一步：配置实例详细信息”
* 网络：选择取证 VPC 网络
* 自动分配公共 IP：禁用
* IAM 角色：为取证目的选择适当的角色
* 启用终止保护：确保选中此框
1. 选择 “下一步：添加存储”
* 选择 “添加新卷” 并创建一个足够大小的卷以处理正在捕获的取证数据。 良好的起始价值是 100GB，但是这个价值基于您的组织需求和体验
1. 选择 “下一步：添加标签”
1. 选择 “查看并启动”
1. 选择 “启动”
1. 通过 EC2 Session Manager 登录 EC2 实例
1. 在实例上安装取证工具。 以下是开源取证工具的几个示例：
1. 安装 SANS SIFT Toolkit
* sudo su-ubuntu
* sudo curl-Lo /usr/local/bin/sift https://github.com/teamdfir/sift-cli/releases/download/v1.10.0-rc5/sift-cli-linux https://github.com/teamdfir/sift-cli/releases/download/v1.10.0-rc5/sift-cli-linux
* sudo chmod +x /usr/local/bin/sift
* sudo /usr/local/bin/sift install —mode=server —user=ubuntu
* sudo /usr/local/bin/sift upgrade
1. 安装 Google Rapid Response
* sudo apt install mysql-server
* sudo mysql_secure_installation
* create database grr; grant all privileges on grr.* to grr @localhost identified by 'password'; flush privileges; @localhost
* wget https://storage.googleapis.com/releases.grr-response.com/grr-server_3.2.4-6_amd64.deb https://storage.googleapis.com/releases.grr-response.com/grr-server_3.2.4-6_amd64.deb
* sudo apt 安装。 /grr-server_3.2.4-6_amd64.deb
* sudo systemctl restart grr-server
* sudo systemctl status grr-server
1. 有关更多信息，请参阅 [如何在 Ubuntu 18.04 上安装和设置 GRR 客户端]（https://kifarunix.com/how-to-install-and-setup-grr-clients-on-ubuntu-18-04-debian-9/）
1. 打开 [Amazon EC2 控制台]（https://console.aws.amazon.com/ec2/）
1. 导航到实例
1. 右键单击要用作 AMI 基础的实例，然后从上下文菜单中选择创建映像。
1. 在 “创建映像” 对话框中，键入唯一的名称和描述，然后选择 “创建映像”。
* 默认情况下，Amazon EC2 会关闭实例，拍摄任何附加卷的快照，创建并注册 AMI，然后重新启动实例。
* 如果您不希望关闭实例，请选择不重启。
* 如果您选择不重启，我们将无法保证创建映像的文件系统完整性。

创建 AMI 可能需要几分钟时间。 创建完成后，它将显示在 AWS 资源管理器的 AMI 视图中。 要显示此视图，请双击 AWS 资源管理器中的 Amazon EC2 | AMI 节点。 要查看 AMI，请从查看下拉列表中选择 “由我拥有”。 您可能需要选择刷新才能查看 AMI。 当 AMI 首次出现时，它可能处于待处于待处理状态，但是过了一会儿，它将转换为可用状态。

### 训练
-“负有法医责任的人员对知识的期望是什么？ （知识/技能/能力）`
-“我的员工接受哪些法医培训和程序？ `
-“我的公司适用哪些法医法规要求？ `
-“为了熟悉 AWS API（命令行环境）、S3、RDS 和其他 AWS 服务，公司内的分析师提供了哪些培训？ `

** 需要在这里添加更多的上下文和项目，因为取证是一种技能，而不只是一本剧本

## 上报程序
-“我需要就什么时候进行 EC2 取证作出业务决定 `
-“谁在监控日志/警报，接收日志/警报并对其采取行动？ `
-“发现警报后谁会收到通知？ `
-“什么时候公共关系和法律参与这一过程？ `
-“你什么时候联系 AWS Support 寻求帮助？ `

## 检测和分析
### 警报的可能来源
* 账单
* CloudWatch（EC2 指标/事件规则）
* 配置
* GuardDuty
* 督察
* Macie
* 安全中心
* 可信赖的顾问
* 外部通知

### 数据保护
你应该 ** 始终 **：
* 将证据设为只读
* 证据应该 ** 永远不会修改 **
* 创建证据副本并仅从那些证据中工作
* ** 从（直接访问）源证据中工作进行分析
* 只读挂载所有证据
* 跟踪世界卫生组织访问什么时间和时间
* 对以下事项做大量调查记录：
* 你分析了什么（哪条数据）
* 当你分析它时（日期/时间）
* 你执行了什么（特定命令/过程/过程）
* 你发现了什么（结果）

### 元数据获取
捕获可能对调查有用的与实例相关的信息
* 实例类型/ID
* IP 地址
* 安全组
* VPC /子Subnet ID
* 地区
* AMI ID
* 启动时间

### 内存获取
建议使用第三方实用程序从正在运行的实例中获取内存。
为了捕获所有可用的挥发性（和有价值的）数据，必须在 ** 之前捕获 ** 系统隔离/关闭内存

### [创建 Amazon EBS Snapshot] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-modifying-snapshot-permissions.html)
1. 打开 [Amazon EC2 控制台]（https://console.aws.amazon.com/ec2/）
1. 在导航窗格中，选择弹性块存储下的快照。
1. 选择创建快照。
1. 对于选择资源类型，选择 “卷”。
1. 对于音量，选择卷。
1. （可选）输入快照的描述。
1. （可选）选择添加标签以向快照添加标签。 对于每个标签，请提供标签密钥和标签值。
1. 选择创建快照。

### [共享 Amazon EBS Snapshot] (https://d1.awsstatic.com/whitepapers/aws_security_incident_response.pdf)
#### 使用控制台共享未加密的快照
1. 打开 [Amazon EC2 控制台]（https://console.aws.amazon.com/ec2/）
1. 在导航窗格中选择快照。
1. 选择快照，然后依次选择操作、修改权限。
1. 将快照公开或与特定 AWS 账户共享快照，如下所示：
* 要将快照公开，请选择 “公共”。
* 此选项对于加密快照或具有 AWS Marketplace 产品代码的快照无效。
* 要与一个或多个 AWS 账户共享快照，请选择私有，在 AWS 账户号码中输入 AWS 账户 ID（不带连字符），然后选择添加权限。 对任何其他 AWS 账户重复此操作。
1. 选择 “保存”。
#### 使用与您私下共享的未加密快照
1. 打开 [Amazon EC2 控制台]（https://console.aws.amazon.com/ec2/）
1. 在导航窗格中选择快照。
1. 选择私有快照筛选器。
1. 按 ID 或描述查找快照。 您可以像使用任何其他快照一样使用此快照；例如，您可以从快照创建卷或将快照复制到其他区域。

#### 使用控制台共享加密快照
1. 打开 [AWS KMS 控制台]（https://console.aws.amazon.com/kms）
1. 要更改 AWS 区域，请使用页面右上角的区域选择器。
1. 在导航窗格中选择客户管理的密钥。
1. 在别名列中，选择用于加密快照的客户托管密钥的别名（文本链接）。 关键细节将在新页面中打开。
1. 在 “关键策略” 部分中，您可以看到策略视图或默认视图。 策略视图显示关键策略文档。 默认视图显示密钥管理员、密钥删除、密钥使用和其他 AWS 账户的部分。 如果您在控制台中创建了策略但尚未对其进行自定义，则会显示默认视图。 如果默认视图不可用，则需要在策略视图中手动编辑策略。 有关更多信息，请参阅 AWS Key Management Service 开发人员指南中的查看密钥策略 (控制台)。
根据您可以访问的视图，使用策略视图或默认视图向策略添加一个或多个 AWS 账户 ID，如下所示：
*（默认视图）向下滚动至其他 AWS 账户。 选择添加其他 AWS 账户，然后根据提示输入 AWS 账户 ID。 要添加另一个账户，请选择添加另一个 AWS 账户并输入 AWS 账户 ID。 添加所有 AWS 账户后，请选择保存更改。
*（策略视图）选择编辑。 将一个或多个 AWS 账户 ID 添加到以下语句中：“允许使用密钥” 和 “允许附加持久性资源”。 选择保存更改。 在以下示例中，AWS 账户 ID 444455556666 已添加到策略中。
``
{
“Sid”：“允许使用钥匙”，
“效果”：“允许”，
“负责人”: {"AWS”: [
“arn: aw: iam። 11112222333: 用户/CMKUSER”,
“arn: aw: iam። 444455556666: 根”
]}，
“行动”：[
“KMS: 加密”,
“KMS: 解密”,
“KMS: 重新加密 *”,
“KMS: 生成数据键 *”,
“KMS: 说明 CRIBEKEY”
]，
“资源”：“*”
}，
{
“Sid”：“允许附加持久资源”，
“效果”：“允许”，
“负责人”: {"AWS”: [
“arn: aw: iam። 11112222333: 用户/CMKUSER”,
“arn: aw: iam። 444455556666: 根”
]}，
“行动”：[
“KMS：创建格兰特”,
“KMS: 列表赠款”,
“KMS：吊销格兰特”
]，
“资源”：“*”，
“条件”: {"Bool”: {"kms: grantiforawsResource”: 真}}
}
``
1. 打开 [Amazon EC2 控制台]（https://console.aws.amazon.com/ec2/）
1. 在导航窗格中选择快照。
1. 选择快照，然后依次选择操作、修改权限。
1. 对于每个 AWS 账户，请在 AWS 账户号码中输入 AWS 账户 ID，然后选择添加权限。 添加所有 AWS 账户后，请选择 “保存”。

#### 使用与您共享的加密快照
1. 打开 [Amazon EC2 控制台]（https://console.aws.amazon.com/ec2/）
1. 在导航窗格中选择快照。
1. 选择私有快照筛选器。 也可以添加加密过滤器。
1. 按 ID 或描述查找快照。
1. 选择快照，然后选择操作、复制。
1. （可选）选择目标地区。
1. 快照副本由主密钥中显示的密钥加密。 默认情况下，选定的密钥是您账户的默认 CMK。 要选择客户管理的 CMK，请在输入框内单击以查看可用密钥的列表。
1. 选择复制。

### 从取证金 AMI 创建取证 EC2
使用之前创建的取证金 AMI 创建新的取证实例

### 将可疑的 EBS 卷挂载到取证 EC2
1. 打开 [Amazon EC2 控制台]（https://console.aws.amazon.com/ec2/）
1. 在导航窗格中，选择弹性块存储区、卷。
1. 选择可用卷，然后选择操作、附加卷。
1. 例如，开始键入实例的名称或 ID。
1. 从选项列表中选择实例（仅显示与卷位于同一可用区的实例）。
1. 对于设备，您可以保留建议的设备名称，也可以键入其他受支持的设备名称。
* 有关更多信息，请参阅 [Linux 实例上的设备名称]（https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/device_naming.html）。
1. 选择 “附加”。
1. 连接到您的实例并挂载卷。
* 有关更多信息，请参阅 [使 Amazon EBS 卷可用于 Linux]（https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html）。

### 执行取证流程
* “该公司在寻找什么取证输出？ `
* “最终报告中将包括什么内容？ `
* “报告向谁发送？ `
* `是否有法律或监管要求来识别取证调查所需的信息？ `
* “公司中是否有人员经过适当培训/获得执照/认证来执行取证？ `
* `对日志、数据或证据有任何保留要求吗？ 可能会查看冰川保险库以支持 `
* SANS 研究所提供了 [Amazon Linux EC2 实例的数字取证分析]（https://www.sans.org/reading-room/whitepapers/cloud/digital-forensic-analysis-amazon-linux-ec2-instances-38235）的示例。

## 遏制
### 立即隔离受影响的资源
** 注意 **：确保您拥有请求上报和批准的流程，以隔离资源，以确保首先就隔离将如何影响当前运营和收入流进行业务影响分析。

### 实例停用
启用终止保护
* [禁用终止实例（通过 Console/API/CLI）] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html)
1. 将实例关闭行为设置为停止（与终止）
1. 对所有实例卷禁用 “Delete On 终止”
* [从 Auto-Scaling group 中删除（分离）] (https://docs.aws.amazon.com/autoscaling/ec2/userguide/detach-instance-asg.html)
1. 在 https://console.aws.amazon.com/ec2autoscaling/ 打开 Amazon EC2 Auto Scaling 控制台
1. 选中 Auto Scaling 组旁边的复选框。
1. Auto Scaling 组页面底部将打开一个拆分窗格，其中显示有关所选组的信息。
1. 在实例管理选项卡的实例中，选择一个实例，然后选择操作、分离。
1. 在 “分离实例” 对话框中，选中该复选框以启动替换实例，或者将其保持不选中状态以减少所需容量。 选择分离实例。
* [从负载均衡器注销] (https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-deregister-register-instances.html)
1. 在 https://console.aws.amazon.com/ec2/ 打开 Amazon EC2 控制台
1. 在导航窗格的负载平衡下，选择负载均衡器。
1. 选择负载均衡器。
1. 在底部窗格中，选择实例选项卡。
1. 选择编辑实例。
1. 选择要向负载均衡器注册的实例。
1. 选择 “保存”。
1. 创建一个阻止所有入口和出口流量的 * 新 * 安全组；确保删除出口流量的默认 `允许 all` 规则
* 将 * new* 安全组附加到受影响的实例

## 根除
此流程没有适用于此 Runbook 的步骤。

## 恢复
取证活动结束后，在最终报告发布之后，您应终止取证 EC2 实例并删除 EBS 快照 ** 除非 ** 有维护数据的法律或监管要求。

## 吸取的教训
“这里可以添加特定于贵公司的物品，这些物品不一定需要 “修复”，但在与运营和业务要求同时执行本行动手册时也很重要。 `

## 已解决积压项目
-作为事件响应者，我需要一本运行手册来进行 EC2 取证
-作为事件响应者，我需要就应该何时进行 EC2 取证作出业务决策
-作为事件响应者，我需要在所有已启用的区域启用日志记录，无论使用意图如何
-作为事件响应者，我需要确保只使用批准的 AMI
-作为事件响应者，我需要能够检测我现有 EC2 实例上的加密挖矿
-作为事件响应者，我需要一种准备好的方法来删除大量实例

## 当前积压项目