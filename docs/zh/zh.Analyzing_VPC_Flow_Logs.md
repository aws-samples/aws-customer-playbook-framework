# 事件响应行动手册：分析 VPC Flow Logs
本文档仅供参考。 它代表了截至本文档发布之日 Amazon Web Services (AWS) 提供的当前产品和实践，这些产品和做法如有更改，恕不另行通知。 客户有责任对本文档中的信息以及对 AWS 产品或服务的任何使用情况进行自己的独立评估，每种产品或服务都是 “按原样” 提供的，无论是明示还是暗示的担保。 本文档不创建 AWS、其附属公司、供应商或许可方的任何担保、陈述、合同承诺、条件或保证。 AWS 对客户的责任和责任受 AWS 协议的控制，本文档既不是 AWS 与其客户之间的任何协议的一部分，也不修改。

© 2024 Amazon 网络服务公司或其附属公司。 保留所有权利。 本作品根据知识共享署名 4.0 国际许可协议进行许可。

提供此 AWS 内容须遵守 http://aws.amazon.com/agreement 提供的 AWS 客户协议的条款或客户与 Amazon Web Services, Inc. 或 Amazon 网络服务 EMEA SARL 或两者之间的其他书面协议。

## 联系点

作者：`作者姓名`\
批准者：`批准者姓名`\
最后批准日期：

## 执行摘要
本行动手册概述了用于分析 AWS VPC Flow 日志的示例流程和查询。

## 准备
本行动手册在可能的情况下引用并集成了 [Prowler] (https://github.com/toniblyx/prowler)，这是一种命令行工具，可以帮助您进行 AWS 安全评估、审计、强化和事件响应。

它遵循 CIS Amazon Web Services Foundations Benchmark 指南（49 个支票），并有 100 多项额外支票，包括与 GDPR、HIPAA、PCI-DSS、ISO-27001、FFIEC、SOC2 和其他相关的支票。

此工具提供了客户环境中当前安全状态的快速快照。 此外，[AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) 提供了自动合规性扫描并可以 [与 Prowler 集成] (https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

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

### 事件分类和处理
* ** 策略、技巧和程序 **：工具：Athena
* ** 类别 **：日志分析
* ** 资源 **：Athena
* ** 指标 **：网络威胁情报，第三方通知
* ** 日志源 **：VPC Flow Logs
* ** 团队 **：安全运营中心 (SOC)、法医调查员、云工程

## 事件处理流程
### 事件响应流程有以下几个阶段：
* 准备
* 检测和分析
* 遏制和根除
* 恢复
* 事件后活动

### 响应步骤
1. [** 准备 **] 创建公开资产库存
2. [** 准备 **] 安装日志记录
3. [** 准备 **] 实施培训计划来识别和评估日志分析能力
4. [** 检测和分析 **] 在控制台中查看 VPC Flow Logs
5. [** 检测和分析 **] 使用 Amazon Athena 查询流日志
6. [** 检测和分析 **] 将 VPC Flow Logs 表作为 DDL 创建
7. [** 检测和分析 **] 示例 Athena 查询：将新的日志表作为 DDL 创建
8. [** 检测和分析 **] 示例 Athena 查询：检查它是否已加载以及文件中有多少行
9. [** 检测和分析 **] 示例 Athena 查询：检查是否返回了可查看和可用的数据：
10. [** 检测和分析 **] 示例 Athena 查询：检查顶级 IP
11. [** 检测和分析 **] 示例 Athena 查询：用于执行调查的标准查询
12. [** 检测和分析 **] 示例 Athena 查询：查找源/目标 IP 的数据比率
13. [** 检测和分析 **] 示例 Athena 查询：搜索已知的 IOC
14. [** 检测和分析 **] 示例 Athena 查询：威胁表
15. [** 检测和分析 **] 示例 Athena 查询：查找目标威胁类型的视图

## 准备
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

Prowler 还可以帮助你找到许多暴露在互联网上的资源：”。 /prowler-g group17"

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
-例如，要在名为 my-bucket 的存储桶中指定名为 my-log 的子文件夹，请使用以下 ARN arn: aws: s3።: my-bucket/my-logs/
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
-例如，要在名为 my-bucket 的存储桶中指定名为 my-log 的子文件夹，请使用以下 ARN：“arn: aws: s3።: my-bucket/my-logs/
-如果您拥有存储桶，我们会自动创建资源策略并将其附加到存储桶。 有关更多信息，请参阅针对流日志的 Amazon S3 存储桶权限。
1. 对于日志记录格式，请选择流日志记录的格式。
* 要使用默认格式，请选择 AWS default format。
* 要使用自定义格式，请选择自定义格式，然后从日志格式中选择字段。
1. 选择 Create flow log。

** 监控 NAT 网关：** 您可以使用 CloudWatch 监控 NAT 网关，CloudWatch 从 NAT 网关收集信息并创建可读、接近实时的指标。
1. NAT 网关指标每隔 1 分钟发送到 CloudWatch。
1. 您可以按如下方式查看 NAT 网关的指标。
* 打开 [Amazon CloudWatch 控制台]（https://console.aws.amazon.com/cloudwatch/）
* 在导航窗格中，选择量度。
* 在 “所有指标” 下，选择 NAT 网关指标命名空间。
* 要查看指标，请选择指标维度。
1. 要为通过 NAT 网关的出站流量创建警报：
* 打开 [Amazon CloudWatch 控制台]（https://console.aws.amazon.com/cloudwatch/）
* 在导航窗格中，选择警报，创建警报。
* 选择 NAT 网关。
* 选择 NAT 网关和 “BytesOutToDestination” 指标，然后选择 “下一步”。
* 按如下方式配置警报，然后在完成后选择创建警报：
* 在警报阈值下，输入警报的名称和描述。 对于任何时候，选择 >= 然后输入 5000000。 连续时间输入 1。
* 在 “操作” 下，选择现有的通知列表或选择 “新建列表” 以创建新列表。
* 在警报预览下，选择 15 分钟的时间段并指定 Sum 的统计数据。
1. 创建警报以监控端口分配错误
* 打开 [Amazon CloudWatch 控制台]（https://console.aws.amazon.com/cloudwatch/）
* 在导航窗格中，选择警报，创建警报。
* 选择 NAT 网关。
* 选择 NAT 网关和 “ErrorPortAllocation” 指标，然后选择下一步。
* 按如下方式配置警报，然后在完成后选择创建警报：
* 在警报阈值下，输入警报的名称和描述。 对于任何时候，选择 > 然后输入 0。 连续时间输入 3。
* 在 “操作” 下，选择现有的通知列表或选择 “新建列表” 以创建新列表。
* 在警报预览下，选择 5 分钟的时间段并指定最大值统计数据。

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

## 检测和分析
### 在控制台中查看 VPC Flow Logs
** 查看网络接口的流日志信息 **
1. 打开 Amazon [EC2 控制台]（https://console.aws.amazon.com/ec2/）
1. 在导航窗格中，选择网络接口。
1. 选择网络接口，然后选择 Flow Logs。 有关流日志的信息显示在选项卡上。 “目标类型” 列指示流日志发布到的目标。

** 查看 VPC 或子网的流日志信息 **
1. 打开 [Amazon VPC 控制台]（https://console.aws.amazon.com/vpc/）
1. 在导航窗格中，选择您的 VPC 或 Subnets。
1. 选择您的 VPC 或子网，然后选择 Flow Logs。
-有关流程日志的信息显示在选项卡上。
-“目标类型” 列指示流日志发布到的目标。

** 查看发布到亚马逊 S3 的流日志记录 **
1. 打开 [Amazon S3 控制台]（https://console.aws.amazon.com/s3/）
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

生成的 CloudFormation 模板提供了一组预定义的查询，您可以运行这些查询，以便快速获得有关 AWS 网络中流量的有意义的见解。 创建堆栈并验证所有资源创建正确后，您可以运行其中一个预定义的查询。

1. 使用控制台运行预定义查询
-打开 Athena 控制台。 在 “工作组” 面板中，选择由 CloudFormation 模板创建的工作组。
-选择一个预定义的查询，根据需要修改参数，然后运行查询。
-打开 Amazon S3 控制台。 导航到您为查询结果指定的存储桶，然后查看查询结果。

以下是生成的 CloudFormation 模板提供的 Athena 命名查询：
* VpcFlowLogsAcceptedTraffic uctions — 基于您的安全组和网络 ACL 允许的 TCP 连接。
* VpcFlowLogsAdminPortTraffic uck — 管理 Web 应用程序端口上记录的流量。
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
* VpcFlowLogsTotalBytesTransferredPacketLevel 别 — 50 对数据包级源和目标 IP 地址，记录字节数最多。
* VpcFlowLogsTrafficFrmSrcAddr — 记录的特定源 IP 地址的流量。
* VpcFlowLogsTrafficToDstAddr — 为特定目标 IP 地址记录的流量

## 深潜分析：

参考：https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html

### 将 VPC Flow Logs 表作为 DDL 创建
以下代码用于为将要检查的 VPC Flow Logs 创建表格作为 DDL。 （请在线注意 TODO 项目）：
``
/*
为直接传送到 S3 存储桶的 VPC 流日志创建 VPC 流表
包括多账户、多区域部署的分区配置以及 YYYY/MM/DD 格式的日期
包括所有可用字段，包括非默认 v3 和 v4 字段。
注意：VPC 流量字段必须按配置时列出的顺序进行配置，如果它们按不同的顺序配置，则需要调整它们在下面的 DDL 中列出的顺序
*/

/*
TODO：可以选择将表名 “vpcflow” 更新为您想用于 VPC 流日志表的名称
*/
如果不存在，请创建外部表 vpcflow (
/*
TODO：验证 VPC 流日志是否已配置为按此顺序生成，如果不是，则需要重新排列以下顺序以匹配它们的生成顺序
提示：下载样本并检查每个日志文件的第一行以获取字段顺序，
别担心，如果他们的字段名称与日志顶行的名称不完全匹配，Athena 将根据他们的顺序导入它们
注意：这些 v2 字段采用默认格式，通常不需要调整，请更加注意下面 v3/v4/v5 字段的顺序，因为这些字段没有默认顺序。 字段和说明可以在 https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html 找到
*/
版本 int，
账户字符串，
接口 ID 字符串，
源地址字符串，
目的地地址字符串，
源端口 int，
目的地港 int，
协议 int，
数字包 int，
numbytes bigint，
开始时间 int，
结束时间 int，
动作字符串，
logstatus 字符串，
/*
注意：非默认 v3 和 v4 字段的开始
不用担心，如果你的源日志不全部包含这些字段，它们只会显示为空白
TODO：如果您的 VPC 流日志包含这些字段，请务必检查下面列出的顺序与流日志格式配置的顺序相同
*/
vpcid 字符串，
键入字符串，
tcpflags 小，
subnetid 字符串，
子位置类型字符串，
sublocation ID 字符串，
区域字符串，
pktsrcaddr 字符串，
pktdstaddr 字符串，
实例 ID 字符串，
azid 字符串，
/*
注意：非默认 v5 字段的开始
不用担心，如果你的源日志不全部包含这些字段，它们只会显示为空白
TODO：如果您的 VPC 流日志包含这些字段，请务必检查下面列出的顺序与流日志格式配置的顺序相同
*/
pkt-src-aws 服务字符串，
pkt-dst-aws 服务字符串，
流向字符串，
流量路径 int
))
分区由
(
date_分区字符串，
区域_分区字符串，
帐户_分区字符串
))
行格式分隔
被 “” 终止的字段
/*
待办事项：在位置值中替换 bucket_name 和可选 _ 前缀，
如果没有前缀那么删除额外的/
例如：s3: //my_Central_LOG_LOG_Bucket/awsLOGS/ 或 s3: //my_Central_LOG_Bucket/prod/awsLOGSS/
*/
位置 '3://<bucket_name>/<optional_prefix>/awsLOGS/'
TBL属性
(
“跳过 .header.line.count" = "1"，
“投影。启用” = “真”，
“投影 .date_partition.type” = “日期”，
/* 待办事项：用日<YYYY><MM><DD>志的第一个日期替换//，例如：2020/11/30 */
“投影 .date_partition.range” = "<YYYY>/<MM>/<DD>，现在”，
“投影 .date_partition.format” = “yyyy/MM/DD”，
“投影 .date_partition. 间隔” = “1”，
“投影 .date_partition.interval.unit” = “天”，
“投影 .区域_partition.type” = “枚举”，
“projection.区域_分区.值” = “us-east-2、us-东-1、us-西-1、us-西-2、af-南-1、ap-东-1、ap-东-1、ap-东-3、AP-东北-2、ap-东南-1、ap-东南-2、ap-东南-2、ap-东 -1、ca-Central-1、cn-West-1、cn-West-1、欧洲-西北-1、欧洲-西北-1、欧洲-西北-1、欧洲-西北-1、欧洲-西北-1、欧洲-西北-1、欧洲-西北-1、欧洲-西北-1、欧洲-西北-1、欧洲-西北-1、欧洲-西北-1、欧1、欧盟-西 -1、欧盟-西 -2、欧盟-南 1、欧盟-西 -3、欧盟-北 1、me-南1、SA-东 -1，
“投影 .帐户_partition.type” = “枚举”，
/*
待办事项：用您希望包含在此表中的 AWS 账号列表替换 projection.count _partion.值中的值
例如：“0123456789,0123456788,0123456777"
注意：不要使用任何空格，只用逗号分隔值（包括空格会导致语法错误）
如果只有一个账户，则单独包含它不带逗号，例如：“0123456789”
*/
“投影 .count _partition.value” = "<account_num_1><account_num_2>， ... “，
/*
待办事项：与 LOCATION 相同，在 storage.location .模板值中替换bucket_name 和可选 _ 前缀，
如果没有前缀那么删除额外的/
确保只替换 <> 括号内的值，{} 括号内的值是变量，必须按下面的定义保留。
例如：s3: //my_Central_LOG_LOG_Bucket/awsLOGSSs/... 或 s3: //my_Central_LOG_Bucket/prod/awsLOGSSs/...
*/
“storage.location .模板” = “s3://<bucket_name>/<optional_prefix>/awsLOGS/$ {count _分区} /vpcflowlogs/$ {区域_分区} /$ {date_分区} /$ {date_分区}”
);
``
### 示例查询：
#### 作为 DDL 创建新的日志表
通过执行以下查询来创建一个新的 Athena 表。 请记住：

* <bucket_name>用目标存储桶替换。
* 检查位置以确保它包含任何相关的存储桶前缀及其尾随斜杠 (“/”)
``
如果不存在，请创建外部表 vpc_flow_logs (
is_拒绝_包字符串，
avg_in_packet_size INT，
vpc_id 字符串，
local_port INT，
source_dc 字符串，
ip 字符串，
end_time 字符串，
remote_port INT，
start_time 字符串，
协议 INT，
in_bytes BIGINT，
source_tag 字符串，
count _id 字符串，
instance_id 字符串，
interface_public_ips 字符串，
out_bytes BIGINT，
is_skipped_拒绝_数据包字符串，
source_az 字符串，
source_区域字符串，
avg_out_packet_size INT，
instance_type 字符串，
Interface_local_ips 字符串
))
行格式 SERDE 'org.openx.data.jsonserde.jsonserde'
使用 SERDEPROPERTIES（'忽略 .malformed.json' = '真'）
位置 '3://<bucket_name>/';
``
#### 检查它是否已加载以及文件中有多少行：
``
从 vpc_flow_logs 中选择计数 (*)；
``
#### 检查是否返回了可查看和可用的数据：
``
从 vpc_flow_logs 限制 1000 中选择 *；
``
#### 检查顶级 IP
``
选择 ip，计数 (*) cnt
来自 vpc_flow_log
按 IP 分组
按 cnt desc 订购
限制 100;

取一些顶级 IP 并按时订购（换掉下面的 IP）

从 vpc_flow_logs 中选择 *
其中 ip = '123.123.123.123.123'
按 start_time 订购;
``

### “S3 Gateway/VPC Interface 注意事项”
对于 “VPC 终端节点”，连接将在流日志中显示为通过其私有地址的流量

S3 Gateway 在流程日志中显示为连接到前缀列表中公有范围的私有地址

### 用于执行调查的标准查询

#### 查询源/目标IP的数据比率
以下查询按 IP、记录计数（同一源或目标地址的总记录数）和总字节（源地址或目标地址之间的通信总字节数）汇总 VPC Flow Log 记录。 该查询还给出了数据比率，即每次通信发送的字节数（总字节数除以记录计数）。
``
选择源地址，
目的地地址，
count (*) 作为记录/count，
SUM（numbytes）作为 total_bytes，
SUM (numbytes) /count (*) 作为 data_ratio，
date_format（从 _unixtime（最小（开始时间）），
'%y/%m/%d%h: %i: %s) 如第一次看到的那样，date_format（从 _unixtime（最大（结束时间））、'%Y/%m/%d%h: %i: %i: %s”) 如上次看到
来自 “<database>“。” <table>”
哪里（源地址 = '10.10.10.10'
或目的地地址 = '10.10.10.10')
按源地址、目的地地址进行分组
按 total_bytes 排序 DESC
``
#### 搜索已知的 IOC
以下查询假定 IOC 是外部 IP 地址，并使用查找表来管理这些地址：

#### 威胁表
``
如果不存在，请创建外部表 incident_iocs (
reat_ip 字符串，
reat_评论字符串
))

行格式 SERDE
'org.apache.hadoop.hive.serde2.opencsvserde'

使用 SERDE属性（
'分隔符' = '、'、
'QUOTECHAR '='\ "'，
'逃生查尔' = '\\'
))
存储为文本文件
位置 '3：//存储桶分析 /iocs/iocfile 这里 /'
``
#### 目标威胁类型的查找视图
``
创建查看事件 _vpc_flow _ 定向 AS
选择 vpc.Account，
vpc.界面id，
vpc.source 地址 || ':' || CAST (vpc.sourceport AS VARCHAR) 作为源代码，
vpc.目的地址 || ':' || CAST (vpc.目的地Port AS VARCHAR) 作为目的地，
“入库” 作为方向，
vpc.source 地址为连接_ip，
vpc.source 端口 AS 连接 _port，
vpc.协议，
vpc.num包，
vpc.numbytes，
vpc.action，
vpc.log 状态，
CAST（从 _unixtime (vpc.starttime) 作为时间戳）作为 StartTime，
演员（从 _unixtime（vpc.endtime）作为时间戳）作为结束时间，
date_diff（'秒'、从 _unixtime（vpc.start 时间）、从 _unixtime（vpc.endtime））作为会话时间

来自 “<database>“。” <table>“AS VPC

哪里
vpc.目标地址输入（从 incident_iocs 中选择 reat_ip）

联盟所有

选择 vpc.Account，
vpc.界面id，
vpc.source 地址 || ':' || CAST (vpc.sourceport AS VARCHAR) 作为源代码，
vpc.目的地址 || ':' || CAST (vpc.目的地Port AS VARCHAR) 作为目的地，
“出站” 作为方向，
vpc.目标地址作为连接_ip，
vpc.目标端口AS 连接_port，
vpc.协议，
vpc.num包，
vpc.numbytes，
vpc.action，
vpc.log 状态，
CAST（从 _unixtime (vpc.starttime) 作为时间戳）作为 StartTime，
演员（从 _unixtime（vpc.endtime）作为时间戳）作为结束时间，
date_diff（'秒'、从 _unixtime（vpc.start 时间）、从 _unixtime（vpc.endtime））作为会话时间

来自 “<database>“。” <table>“AS VPC
哪里
vpc.source 地址输入（从 incident_iocs 中选择 reat_ip）

按 StartTime、结束时间、会话时间、接口 ID、connect _ip、方向排序
``

## 已解决积压项目
-作为事件响应者，我需要能够监控所有关键的 VPC 环境
-作为事件响应者，我需要一本关于大规模查询 VPC FlowLogs 的行动手册

## 当前积压项目