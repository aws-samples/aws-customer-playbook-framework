# 应对 AWS 内的赎金攻击

## 通知

本文档仅供参考。 它代表
来自 Amazon 网络服务的当前产品和做法
(AWS) 截至本文档发布之日，该文档须遵守
更改恕不另行通知。 客户有责任自行制作
对本文档中的信息和任何用途进行独立评估
的 AWS 产品或服务，其中每个产品或服务都是 “按原样” 提供的，没有
任何形式的担保，无论是明示还是暗示的。 本文档没有
创建任何担保、陈述、合同承诺，
来自 AWS、其附属公司、供应商的条件或保证
许可方。 AWS 对客户的责任和责任
受 AWS 协议控制，且本文档不是其中的一部分，也不是
它是否修改了 AWS 与其客户之间的任何协议。 \
\
© 2024，Amazon 网络服务公司或其附属公司。 所有权利
保留。

## 勒索软件

自 1989 年第一次被记录的勒索软件攻击名为 PC Cyborg 以来，
勒索软件已成为犯罪分子使用的突出的货币化策略
互联网上的组织和威胁行为者。 其他例子
勒索软件包括：

-CryptoLocker\ [2014\]
-Petya\ [2016\]
-WannaCry\ [2017\]
-NotPetya\ [2017\]
-Ryuk\ [2019\]）

威胁行为者利用整个客户的问题和弱点
基础设施、利用易受攻击的终端、不安全的服务以及
社交工程员工。

勒索软件攻击正在使政府、非营利组织和企业付出代价
数十亿美元并中断了运营。 NotPetya 被迫
运送巨头马士基以重新安装 4,000 台服务器和 45,000 台 PC
\ 3 亿美元，原因是 “严重的业务中断”。 针对的勒索软件攻击
巴尔的摩市的成本超过 1800 万美元，地方政府
佛罗里达州里维埃拉海滩和湖城将支付黑客\ 100 万美元加起来
恢复其系统和数据。 美国联邦调查局
（FBI）预计勒索软件的威胁将变得 “更有针对性，
在不久的将来，成本高昂”。 这些警告到达
超越美国边界，欧洲刑警组织也称勒索软件为 “最多”
广泛而且具有财务损害性的网络攻击形式。”

## 什么是赎金攻击？

赎金攻击是一种货币化策略，而不是特定的技术。
攻击通常会利用特定的恶意软件或勒索软件，
威胁在赎金之前发布受害者信息或阻止访问
已支付。

理论上，如果在规定的时间内支付赎金，系统和
数据被解密并再次提供以及正常操作
继续。 但是，如果赎金不能满足，组织面临风险
永久销毁或面向公众的数据泄露由
攻击者。 攻击者还可以进一步勒索客户以释放
向第三方或公众提供客户敏感数据和系统。

## 勒索软件不在乎

许多勒索金攻击本质上都是机会主义的，这意味着勒索软件
通过人类和/或不区分地感染任何可访问的网络
机器矢量。 面向教育机构、州和
地方政府和医疗保健组织正在加强措施
以确保他们的数据免受勒索软件攻击的增加。 威胁
演员了解如何识别行业垂直行业的弱点。 很多
教育和政府组织更容易出现勒索软件到期
预算缩减、安全资源缺口以及
具有已知未修补漏洞的传统 IT 系统。 同样，
赎金攻击可能针对不容忍停机的行业，例如
医院，希望增加支付的可能性。

## 为什么赎金攻击有效？

-员工的安全意识很低
-组织没有备份数据，或者未能测试现有备份
-攻击需要的技能很少，导致显著的支出
-组织在修复关键的常见漏洞和风险 (CVE) 方面速度缓慢
-负担过重的技术人员无法解决或预计所有的安全漏洞
-在一次攻击中使用多个媒体或频道
-窃取数据（数据泄露、未经授权的复制）不会停止业务流程，客户可能不会在加密或删除信息之前做出反应

## 要付还是不支付？

网络安全专业人员之间存在着关于
决定支付赎金。 包括联邦调查局在内的许多专家建议
组织不支付赎金，辩称支付不会
保证将提供锁定的系统或加密数据
而且只会继续激励邪恶的行为。 尽管
支付赎金后无法保证系统和数据访问权限，有些
组织承担计算出来的风险来支付，希望恢复正常
操作。 通过这样做，他们希望降低潜在的辅助成本
包括生产力丧失在内的攻击、收入随着时间的推移减少
敏感数据的暴露以及声誉损害。 勒索软件
威胁是严重的，但是明智的准备和持续的警惕
有效的反击它。 数据安全的全部盔甲包括
包括人力控制和技术控制，但是 AWS 的一些功能
有助于缓解勒索软件攻击的云。 AWS 致力于
为您提供工具、最佳实践和服务，以帮助您实现高水平
可用性、安全性和弹性，以解决
互联网。

## 保护 AWS 上的系统和数据是 [共同责任] (https://aws.amazon.com/compliance/shared-responsibility-model)

当您在 AWS 云中部署应用程序和基础设施时，AWS
通过与你分担安全责任来提供帮助。 AWS 工程师
使用安全设计原则的底层云基础设施
客户必须为工作负载实施自己的安全架构
在 AWS 上部署。 AWS 负责保护基础设施
它运行 AWS 云上提供的所有服务。 该基础设施
由硬件、软件、网络和设施组成
运行 AWS 云服务。 客户选择的 AWS 云服务
确定客户必须执行的配置工作量
他们的一部分安全责任。 客户总是
负责管理和保护其数据，对他们的数据进行分类
资产，并使用 AWS 身份访问管理 (IAM) 应用
适当的权限。

## AWS 支持

如果你或你的客户认为某个账户处于有效赎金之下
攻击，受影响的客户应该从 AWS Support 中创建一个案例
从受影响的账户中进行中心。 如果 root 用户不能
已访问或被入侵，那么客户应该打开一个新的 AWS
账户然后从那里开一张支持票。 此过程允许 AWS
与客户进行身份验证并开始参与
内部资源，以便为修复事件提供最佳帮助。

## AWS Security Reference Architecture (AWS SRA)

[Amazon Web 服务 (AWS) 安全参考体系结构 (AWS)
SRA)] (https://docs.aws.amazon.com/prescriptive-guidance/latest/security-reference-architecture/welcome.html)
是一套用于部署 AWS 全部补充的整体指南
多账户环境中的安全服务。 它可以用来帮助
设计、实施和管理 AWS 安全服务，以便它们保持一致
使用 AWS 最佳实践。

## 美国网络安全与基础设施安全局

[勒索软件指南（9 月。
2020)] (https://www.cisa.gov/sites/default/files/publications/CISA_MS-ISAC_Ransomware%20Guide_S508C.pdf)
提供最佳实践和参考资料，以帮助管理由
勒索软件并支持组织的协调和高效
对勒索软件事件的响应。

## 欧盟网络安全机构（ENISA）

2020 年 [ENISA 威胁格局-
勒索软件] (https://www.enisa.europa.eu/publications/ransomware)
概述了有关勒索软件的调查结果，提供了描述和分析
，并列出了最近的相关事件。 一系列提议
提供了缓解措施。

## 澳大利亚网络安全中心（ACSC）

ACSC 提供了几个指南，包括对 [勒索软件的回应
澳大利亚] (https://www.cyber.gov.au/sites/default/files/2020-10/Ransomware%20in%20Australia%20%28October%202020%29.pdf), [勒索软件
攻击紧急响应
指南] (https://www.cyber.gov.au/sites/default/files/2024-07/11515_ACSC_Emergency-Response-Guide_Accessible_08.12.20.pdf)，
和 [勒索软件攻击的预防和保护
指南] (https://www.cyber.gov.au/sites/default/files/2024-07/11515_ACSC_Prevention-And-Protection-Guide_Accessible_08.12.20.pdf)。

## NIST 网络安全框架

该指南，NIST 网络安全框架（CSF）：与 NIST CSF 保持一致
在 AWS 云中，旨在帮助商业和公共部门
世界任何地方的任何规模的实体都与 CSF 保持一致
使用 AWS 服务和资源。

## 勒索软件缓解：5 大保护和恢复准备措施

AWS 提供了一个公共安全博客，内容涵盖 [勒索软件缓解措施：
5 大保护和恢复准备
操作。] (https://aws.amazon.com/blogs/security/ransomware-mitigation-top-5-protections-and-recovery-preparation-actions/)
这些包括：

-设置恢复应用程序和数据的功能
-加密你的数据
-应用关键补丁
-遵循安全标准
-确保你正在监控并自动执行响应

您还应该对暴露的远程访问使用强身份验证
服务，尤其是远程桌面协议和安全外壳。 [多因素
身份验证与单个结合
登录] (https://docs.aws.amazon.com/singlesignon/latest/userguide/mfa-how-to.html)
是一种能够保护远程连接的技术机制
和资源请求。

## 开发、安全和运营（DevSecOps）

* 背景 *：DevSecOps 的目的是安全地帮助客户
加快与客户（即最终用户）的反馈循环。 By
加速这些反馈循环，客户可以发货更多软件
以更高的质量、安全性和稳定性改变生产。 By
在开发过程中建立安全性，客户可以防止
在生产环境中向最终用户部署漏洞。
根据 NIST 的说法，一旦安全缺陷修复缺陷的成本
使其投入生产的价格可能比期间高出 60 倍
开发周期
\ [[来源] (https://securityboulevard.com/2020/09/the-importance-of-fixing-and-finding-vulnerabilities-in-development/)\]。

根据 AWS 客户事件响应团队的数据，大多数
客户赎金攻击分为以下三类之一：1/ 坏
actor 扫描源代码存储库（例如 GitHub）以获取 AWS 访问密钥。
使用这些长期访问密钥，他们可 AWS 访问
客户的账户。 风险根据访问级别而增加
访问密钥允许。2/ 通过扫描公共 S3 存储桶和
对象，坏行为者可以阻止访问和加密 S3 存储桶，防止
所有者无法访问数据。3/ 坏角色扫描并利用网络
应用程序漏洞（例如代码注入和跨站点）
脚本）在公共端口上获取对客户数据的访问权限。

客户错误地认为 AWS 提供了其中许多保障措施
自动作为拥有 AWS 账户的一部分。 虽然 AWS 提供
数千种安全功能，包括默认情况下的私有
（例如，EC2 安全组和 S3 存储桶），作为 [共享的
责任
模型]（https://aws.amazon.com/compliance/shared-responsibility-model/），
客户必须检测并修复其中的安全漏洞
代码和/或其 AWS 环境中。 有 AWS 服务和工具
用于检测和修复这些不合规的资源，但
客户必须具备自动应用实践的知识
跨越他们的 AWS 资源。

部署管道分解了构建、测试、部署和发布操作
进入一系列阶段。 每个阶段都提供了增强信心，
通常需要额外的时间。 早期阶段可以找到大多数问题
获得更快的反馈，而后期阶段提供的反馈速度越来越慢
彻底探测。 构建者自动执行这些中的许多操作
部署管道；部署管道是关键的体系结构
[连续] 的构造
送货] (https://aws.amazon.com/devops/continuous-delivery/)。 该
部署管道的任务是检测任何可能导致的更改
生产中的问题。 这些可能包括性能、安全性或
可用性问题
\ [[来源] (https://martinfowler.com/bliki/DeploymentPipeline.html)\]。
为了本文件的目的，我们参考了建筑物的做法
安全测试并检查部署管道，以便他们可以防止
并将赎金攻击作为持续安全。

使用持续安全性来防止和
减轻赎金攻击是：1/ 防止向来源提交秘密
代码存储库，以便不良行为者没有访问 AWS 的密钥
资源。2/ 防止常见的开发人员错误导致网络
应用程序易受攻击（静态和运行时）。3/ 确保存在
备份受损的 AWS 资源，以便不良行为者没有
阻止对资源的访问有很多好处。4/ 全部加密
相关数据，以便如果坏行为者获得了数据的访问权限，他们有
通过敲诈获得的少得多了。5/ 检测何时未经授权的用户
访问 AWS 资源以便进行缓解措施。6/ 确保 IAM
访问密钥使用 IAM 角色的寿命很短，因此访问权限有限
受损资源的持续时间。7/ 确保 IAM 权限使用最少
如果坏演员可以防止真正不好的事情发生
获得对密钥的访问权限。8/ 防止或检测错误配置
使 AWS 资源容易受到攻击。

* 在向源代码存储库提交秘密时防止和检测 ***：**
从他们的开发人员环境（例如，他们的 IDE），客户应该
运行密钥检测静态应用程序安全测试 (SAST) 工具
在将代码提交给源代码仓库之前。 此外，第一个
自动部署管道的阶段运行秘密检测 SAST
用于检测秘密何时被提交到源代码仓库的工具。
发现机密时，请将管道配置为失败，通知
开发人员，并在应用修复之前停止将来的提交。 此外，
运行自动化以清理机密的 git 历史记录并禁用 AWS
访问密钥。 此解决方案的示例工具包括 AWS CodePipeline、AWS
CodeBuild、git-secrets 和用于修复机密的自定义代码。

* 防止和检测制作应用程序的常见开发者错误
易受攻击 ***：** 从他们的开发人员环境（例如，他们的 IDE），
客户应运行 SAST 和代码审查工具来检测 Web
漏洞，例如代码注入、跨站点脚本和其他
不安全的编码做法。 此外，自动化的第一阶段
部署管道运行这些工具来检测 Web 漏洞的时间
提交给源代码仓库。 发现这些网络时
漏洞、将管道配置为失败、通知开发人员
在应用修复之前停止将来的提交。 更重要的是，客户还可以
配置这些 Web 漏洞的运行时检测和修复
作为共享服务部署管道的一部分，该管道配置和
将这些服务配置为代码。 此解决方案的示例工具有：
Amazon CodeGuru、AWS CloudFormation、AWS CodePipeline、AWS CodeBuild、
Sonarqube/Checkmarx，以及 AWS WAF 和 WAF 安全自动化。

* 备份受损的 AWS 资源：* 在运行时，检测未标记
和/或没有备份的相关 AWS 资源。 此外，
客户可以配置 AWS 资源的配置，以提供
作为共享服务部署管道的一部分的运行时检测
配置这些服务并将其配置为代码。 这方面的示例工具
解决方案包括 AWS Backup、AWS CloudFormation、AWS CodePipeline、AWS
CodeBuild，Amazon EventBridge，[AWS Config
规则] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
（例如，required-tags、redshift-backup-enabled、
aurora-resources-protected-by-backup-plan，
backup-plan-min-frequency-and-min-retention-check，
backup-recovery-point-manual-deletion-disabled，
支持 db-instance-backup-enabled、备份中的 dynamodb-in-backup-plan，
ebs-in-backup-plan), Amazon SNS, and-.

* 加密传输中和静态数据 ***：** 来自开发人员
环境（例如，他们的 IDE），客户应运行 SAST 工具
检测 Web 漏洞，例如代码注入和跨站点
脚本编写。 此外，自动部署的第一阶段
管道运行 iAC linting 工具来检测开发人员何时定义
未加密的 AWS 资源作为代码并将配置代码提交给
源代码回购。 当发现这些未加密的资源时，
将管道配置为失败、通知开发人员并停止将来
提交直到应用修复。 此外，客户可以配置
配置 AWS 资源，以提供运行时检测和
作为共享的一部分对这些未加密的 AWS 资源进行修复
配置和配置这些服务的服务部署管道
服务即代码。 此解决方案的示例工具是 AWS
CloudFormation、AWS CodePipeline、AWS CloudFormation Guard 士、AWS
CodeBuild，Amazon EventBridge，[AWS Config
规则] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
（例如，ec2-ebs-encryption-by-default，dynamodb-table-encrypted-kms，
rds-snapshot-encrypted，cloudwatch-log-group-encrypted，
cloud-trail-encryption-enabled，
s3-bucket-server-side-encryption-enabled，启用 api-gw-ssl-enabled，
cloudfront-custom-ssl-certificate）、Amazon SNS、AWS KMS 和 AWS Lambda。

* 检测未经授权的用户何时访问 AWS 资源：* 在运行时，
检测数据泄露\ “尖峰\”（例如，使用 CloudWatch 指标
具体而言
[网络输出]（https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html））。
例如，攻击者可能会执行数据销毁，
留下了赎金票据，在这些情况下，没有获得数据的机会
通过与恶意行为者合作进行恢复。 更重要的是，客户
可以配置提供运行时间的 AWS 资源的配置
作为共享服务部署管道的一部分进行检测
配置这些服务并将其配置为代码。 这方面的示例工具
解决方案包括 AWS CloudFormation、AWS CodePipeline、AWS CodeBuild、Amazon
EventBridge、Amazon GuardDuty、Amazon SNS 和 Amazon CloudWatch
指标。

* 确保 IAM 访问密钥有效时间很短 ***：** 在运行时，运行以下工具：
检测 IAM 访问密钥和策略的各种状态，包括轮换
长期访问密钥。 客户可以配置 AWS 的配置
作为共享服务的一部分提供运行时检测的资源
将这些服务配置和配置为
代码。 此解决方案的示例工具包括 AWS CloudFormation、AWS
CodePipeline、AWS CodeBuild、Amazon EventBridge、[AWS Config
规则] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
（例如，iam-policy-no-statements-with-admin-access，
iam-policy-no-statements-with-full-access，iam-root-access-key-check，
iam-user-no-policies-check /iam-user-unused-credentials-check，
access-keys-rotated）和 AWS Lambda。

* 确保 IAM 权限使用的权限最少 ***：** 来自开发人员
环境（例如，他们的 IDE），客户应运行 SAST 工具
在提交之前检测过于宽容的权限定义
代码到源代码库。 此外，自动化的第一阶段
部署管道运行 SAST 工具来检测过于宽容
将代码提交到源代码之后的策略定义
回购。 发现过于宽容的策略定义时，请配置
管道将失败、通知开发人员并停止将来的提交，直到
应用修复程序。 此外，检测整个过于宽松的政策
AWS 账户或账户（即源代码库之外）。 反过来，
客户可以配置 AWS 资源的配置，以提供
作为共享服务部署管道的一部分的运行时检测
配置这些服务并将其配置为代码。 这方面的示例工具
解决方案包括 AWS CloudFormation、AWS CodePipeline、AWS CodeBuild、
pmapper/AWSPX、AWS IAM Access Analyzer、Amazon EventBridge、AWS Config
规则和 AWS Lambda。

* 防止使 AWS 资源易受攻击的错误配置 *：
错误配置或不启用某些 AWS 资源可能会造成
客户容易受到赎金攻击。 最值得注意的资源是
那些与网络、计算、存储和数据库相关的内容。 什么是
更多，客户可以配置 AWS 资源的预配置
作为共享服务部署的一部分提供运行时检测
将这些服务配置和配置为代码的管道。 示例
该解决方案的工具包括 AWS CloudFormation、AWS CodePipeline、AWS
CodeBuild、Amazon EventBridge、AWS Config Rules（例如
s3-account-level-public-access-blocks、s3-bucket-default-lock-enabled、
s3-bucket-level-public-access-prohibited，s3-bucket-logging-enabled，
s3-bucket-policy-not-more-permissive，s3-bucket-public-read-prohibited，
s3-bucket-public-write-prohibited、s3-bucket-replication-enabled、
cloud-trail-log-file-validation-enabled，
ec2-security-group-attached-to-eni，vpc-default-security-group-closed，
和 vpc 流日志启用、启用 guardduty-enabled-centralized），Amazon
检查器网络可达性、Amazon VPC Reachability Analyzer、Amazon
S3 和 AWS Lambda。

## Amazon 弹性计算云（* 亚马逊 EC2*）-Microsoft Windows

### 识别
-评估账户的安全状况，以识别和补救安全漏洞
-AWS 开发了一种新的开源 [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) 工具，该工具为客户提供了时间点评估，以获得对其 AWS 安全状况的宝贵见解账户。
-维护所有资源的完整资产清单，包括域控制器、Microsoft Windows EC2 实例、Microsoft Windows 服务器和数据库，以及与外部身份提供商的任何集成。
-使用诸如 [Amazon Inspector] 之类的实用程序对房东进行反复性漏洞分析 (https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/)
-为了保护你的组织，Microsoft 建议你使用 [** 人工操作的勒索软件缓解项目计划 **]（https://download.microsoft.com/download/7/5/1/751682ca-5aae-405b-afa0-e4832138e436/RansomwareRecommendations.pptx）PowerPoint 演示文稿中的信息，其中包括 [安全特权访问] (https://aka.ms/spa)。
-使用 [CloudWatch 指标]（https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html），特别是 NetworkPacketsOut，查找数据泄露的 “峰值”。 攻击者可能会执行数据销毁并留下赎金票据，在这种情况下，与恶意行为者合作没有恢复数据的机会

### 保护
-将最新更新应用于操作系统和应用程序
-如果您的电脑制造商尚未打开文件，请使用文件历史记录备份文件。 [了解有关文件历史记录的更多信息] (https://support.microsoft.com/en-us/windows/file-history-in-windows-5de0e203-ebae-05ab-db85-d5aa0a199255)
-定期备份重要文件。 使用 3-2-1 规则。 将数据的三个备份保存在两种不同的存储类型上，至少有一个异地备份
-阻止已知的勒索软件文件类型
-[阻止 Office 文档中的宏] (https://support.microsoft.com/en-us/topic/enable-or-disable-macros-in-office-files-12b036fd-d140-4e74-b45e-16fed1a7e5c6)
-教育员工，以便他们能够识别社交工程和矛头网络钓鱼攻击
-[遵循保护活动目录服务的最佳实践] (https://www.calcomsoftware.com/how-to-secure-your-active-directory-when-anonymous-users-must-have-access/)
-[关注十大最重要的组策略设置以防止安全漏洞] (https://www.lepide.com/blog/top-10-most-important-group-policy-settings-for-preventing-security-breaches/)
-[实现受控文件夹访问权限]（https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/controlled-folders）。 它可以阻止勒索软件加密文件并保留文件以获取赎金
-执行 EC2 实例的备份
-考虑使用 [AWS Backup] (https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) 或 [AWS CloudEndure] (https://aws.amazon.com/cloudendure-disaster-recovery/)
-Microsoft 365 Defender Threat Intelligence Team 已经提供了 [综合报告] (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) 在勒索软件事件发生之前识别为保护你的 Microsoft 资源而采取的措施
-加固面向互联网的资产，并确保它们具有最新的安全更新。 使用威胁和漏洞管理定期审核这些资产，以了解漏洞、错误配置和可疑活动。
-监控暴力尝试。 检查过度失败的身份验证尝试（Windows 安全事件 ID 4625）
-监控清除事件日志，尤其是安全事件日志和 PowerShell 操作日志。 Microsoft Defender ATP 发出警报 “Event log was cleared”，当发生这种情况时，Windows 会生成一个事件 ID 1102
-实践最低特权原则并保持凭证卫生。 避免使用域范围内的管理员级别的服务帐户。 强制执行强大的随机、即时的本地管理员密码。 使用 LAPS 之类的工具
-使用 Azure 多重身份验证 (MFA) 等解决方案安全远程桌面网关。 如果没有 MFA 网关，请启用网络级身份验证 (NLA)
-如果你有 Office 365，则为 Office VBA 打开 AMSI
-启用攻击面减少规则，包括阻止凭据盗窃、勒索软件活动以及可疑使用 PsExec 和 WMI 的规则。 要解决通过武器化 Office 文档发起的恶意活动，请使用阻止 Office 应用程序启动的高级宏活动、可执行内容、进程创建和进程注入的规则。 要评估这些规则的影响，请在审计模式下部署它们
-在 Windows Defender 防病毒软件上打开云交付的保护和自动样品提交。 这些功能使用人工智能和机器学习来快速识别和阻止新的和未知的威胁
-打开篡改保护功能以防止攻击者停止安全服务
-尽可能利用 Windows Defender Firewall 和网络安全系统来防止端点之间的 RPC 和 SMB 通信。 这限制了横向移动以及其他攻击活动。
-打开和更新 Windows Defender 防病毒软件-首选商业、订阅付费端点检测和响应 (EDR) 解决方案
-在 Windows 10 中打开 [受控文件夹访问权限] (https://support.microsoft.com/en-us/topic/ransomware-protection-in-windows-security-445039d6-537a-488a-ad53-48906f346363) 以保护您的重要本地文件夹免受勒索软件或其他恶意软件等未经授权的程序的侵害
-使用 [Systems Manager 和 Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/) 检查运行 Microsoft Windows 的 EC2 实例是否包含任何常见漏洞和风险 (CVE)
-验证备份并确保感染没有传播到备份中

### 检测
-Microsoft 365 Defender Threat Intelligence Team 已经提供了一份 [综合报告]（https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/），确定了针对多种勒索软件变体要查找的内容
-使用 vpcFlowLogs 识别来自外部 IP 地址的不当数据库访问
-你可以 [利用 Amazon 侦探调查 VPC 流程] (https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) 或 [Amazon Athena] (https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)

### 回应
-基于网络 IOC 强制执行 NACL 以防止进一步的流量
-隔离受感染的系统
-检查备份是否有潜在的感染
-从网络中移除任何受损的系统。
-遵循监管要求或公司内部政策以确定是否需要 EC2 实例的取证
-如果需要对实例进行取证或需要恢复数据，则 [隔离 EC2 实例]（https://support.alertlogic.com/hc/en-us/articles/115005534366-Quarantine-an-EC2-Instance）
-[从域中删除受损的域控制器元数据] (https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/ad-ds-metadata-cleanup)
-使用流日志来确定泄漏 IP，查找与数据从 CloudTrail 泄露/访问的 IP 进行通信的任何其他实例

### 恢复
-建议不要支付赎金
-支付赎金是一场赌博，罪犯在收到付款后是否会兑现交易
-如果没有数据备份，那么你应该进行成本效益分析，权衡数据/声誉损失的价值与向攻击者付款的价值
-如果你选择支付赎金，你可以直接让攻击者继续对你的公司或其他人进行操作
-从受信任的 AMI 创建新的 EC2 实例
-使用 CloudEndure 灾难恢复在勒索软件攻击或数据损坏之前选择最新的恢复点，以恢复 AWS 上的工作负载
-如果使用备用数据备份策略，请验证备份没有被感染并从勒索软件事件发生之前的最后一个计划事件中恢复
-访问 < https://www.nomoreransom.org/ > 以确定是否可用于受感染的数据的恶意软件变体的解密

## Amazon 弹性计算云 (* 亚马逊 EC2*)-Unix/Linux

### 识别
-评估你的安全态势以识别和补救安全漏洞
-AWS 开发了一种新的开源 [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) 工具，该工具为客户提供了时间点评估，以便快速获得有关他们的 AWS 账户。
-维护所有资源的完整资产库存，包括服务器、网络设备、网络/文件共享和开发人员机器
-使用诸如 [Amazon Inspector] 之类的实用程序对房东进行反复性漏洞分析 (https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/)
-使用 [CloudWatch 指标]（https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html），特别是 NetworkOut，查找数据泄露的 “峰值”。 攻击者可能会执行数据销毁并留下赎金票据，在这种情况下，与恶意行为者合作没有恢复数据的机会

### 保护
-将最新更新应用于操作系统和应用程序
-定期备份重要文件。 使用 3-2-1 规则。 将数据的三个备份保存在两种不同的存储类型上，至少有一个异地备份
-阻止已知的勒索软件文件类型
-考虑使用 AWS Config 创建规则来监控 AWS 服务的更改。 AWS Config 预先构建了几个用于监控 EBS 和 EC2 的托管规则，其中包括：
-[ebs-in-backup-plan] (https://docs.aws.amazon.com/config/latest/developerguide/ebs-in-backup-plan.html)
-[ebs-optimized-instance] (https://docs.aws.amazon.com/config/latest/developerguide/ebs-optimized-instance.html)
-[ebs-snapshot-public-restorable-check] (https://docs.aws.amazon.com/config/latest/developerguide/ebs-snapshot-public-restorable-check.html)
-[ec2-ebs-encryption-by-default] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-ebs-encryption-by-default.html)
-[ec2-imdsv2-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-imdsv2-check.html)
-[ec2-instance-detailed-monitoring-enabled 功能] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-detailed-monitoring-enabled.html)
-[ec2-instance-managed-by-systems-manager] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-managed-by-systems-manager.html)
-[ec2-instance-multiple-eni-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-multiple-eni-check.html)
-[ec2-instance-no-public-ip] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-no-public-ip.html)
-[ec2-instance-profile-attached] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-profile-attached.html)
-[ec2-managedinstance-applications-blacklisted] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-applications-blacklisted.html)
-[ec2-managedinstance-applications-required] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-applications-required.html)
-[ec2-managedinstance-association-compliance-status-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-association-compliance-status-check.html)
-[ec2-managedinstance-inventory-blacklisted] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-inventory-blacklisted.html)
-[ec2-managedinstance-patch-compliance-status-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-patch-compliance-status-check.html)
-[ec2-managedinstance-platform-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-platform-check.html)
-[ec2-security-group-attached-to-eni] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-security-group-attached-to-eni.html)
-[ec2-stopped-instance] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-stopped-instance.html)
-[ec2-volume-inuse-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-volume-inuse-check.html)
-教育员工，以便他们能够识别社交工程和矛头网络钓鱼攻击
-在所有系统上启用恶意软件检测和防病毒软件-首选商业、订阅付费端点检测和响应 (EDR) 解决方案。
-示例指南可以在 [如何安装和使用 Linux 恶意软件检测 (LMD) 以 ClamAV 作为防病毒引擎] (https://www.tecmint.com/install-linux-malware-detect-lmd-in-rhel-centos-and-fedora/)
-执行 EC2 实例的备份
-考虑使用 [AWS Backup] (https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) 或 [AWS CloudEndure] (https://aws.amazon.com/cloudendure-disaster-recovery/)
-Microsoft 365 Defender Threat Intelligence Team 提供了一份 [综合报告]（https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/），确定了在勒索软件事件发生之前保护 Microsoft 资源的行动。 其中许多建议都可以适用于 Unix 和 Linux，包括：
-确定高度特权的帐户在哪里登录并公开凭据。
-加固面向互联网的资产，并确保它们具有最新的安全更新。 使用威胁和漏洞管理定期审核这些资产，以了解漏洞、错误配置和可疑活动。
-监控暴力尝试。 检查过度失败的身份验证尝试（/var/log/auth.log 或 /var/log/secure）/var/log/auth.log
-监控清除事件日志
-实践最低特权原则并保持凭证卫生。 避免使用域范围内的管理员级别的服务帐户。 强制执行强大的随机、即时的本地管理员密码。
-打开篡改保护功能以防止攻击者停止安全服务
-使用终端安全代理，例如 [Wazuh]（https://documentation.wazuh.com/current/getting-started/components/index.html）
-使用文件完整性监视器，例如 [Tripwire] (https://github.com/Tripwire/tripwire-open-source) 来检测对关键文件的更改
-使用 [Systems Manager 和 Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/) 检查 EC2 实例是否包含任何常见漏洞和风险 (CVE)
-验证备份并确保感染没有传播到备份中

### 检测
-理查德·霍恩（Richard Horne）的 [No Strings on Me: Linux and Ransomware]（https://www.sans.org/reading-room/whitepapers/tools/strings-me-linux-ransomware-39870）中的表 1 确定了要监控的多个指标，包括：
-以前有外部网络连接请求的终端上从未见过的新过程
-尝试使用找到熵的 DNS 名称或直接使用裸 IP 进行通信
-更改分布 Yum 或 Apt 仓库
-在非基于用户的主目录中创建 .sh 文件
-枚举共享 Web、数据库或文件存储目录树
-权限升级或尝试获得 sudo 访问权限
-外部和内部网络通信
-使用熵生成或快速连续生成的文件名
-在同一目录树中多次重命名文件
-用奇数文件扩展名创建的文件
-重点放在典型的日常运营范围之外的异常值和事件
-短期或持续的时间内存使用量较高
-在子进程之前终止父进程的大型进程树
-修改启动选项
-多个目录中同一文件的多个副本
-在单个目录中进行多次修改
-可能要求加密库
-可能在 /tmp 目录内创建和运行进程
-快速连续的目录枚举
-使用通配符或未经确认从目录中删除文件
-使用 chmod 与通配符（或 chmod 777）
-使用 wget 或 curl cmds
-使用 chmod cmd 将可执行文件更改为过于宽容的权限
-使用字符串 cmd 尝试在感染之前或感染期间对通信进行编码
-使用 vpcFlowLogs 识别来自外部 IP 地址的不当数据库访问
-你可以 [利用 Amazon 侦探调查 VPC 流程] (https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) 或 [Amazon Athena] (https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)

### 回应
-检查备份是否有潜在的感染
-隔离受感染的系统
-从网络中移除任何受损的系统。
-遵循监管要求或公司内部政策以确定是否需要 EC2 实例的取证
-如果需要对实例进行取证或需要恢复数据，则 [隔离 EC2 实例]（https://support.alertlogic.com/hc/en-us/articles/115005534366-Quarantine-an-EC2-Instance）

### 恢复
-建议不要支付赎金
-支付赎金是一场赌博，罪犯在收到付款后是否会兑现交易
-如果没有数据备份，那么你应该进行成本效益分析，权衡数据/声誉损失的价值与向攻击者付款的价值
-如果你选择支付赎金，你可以直接让攻击者继续对你的公司或其他人进行操作
-从受信任的 AMI 创建新的 EC2 实例
-遵循监管要求或公司内部政策以确定是否需要 EC2 实例的取证
-使用 CloudEndure 灾难恢复在勒索软件攻击或数据损坏之前选择最新的恢复点，以恢复 AWS 上的工作负载
-如果使用备用数据备份策略，请验证备份没有被感染并从勒索软件事件发生之前的最后一个计划事件中恢复
-访问 < https://www.nomoreransom.org/ > 以确定是否可用于受感染的数据的恶意软件变体的解密

## Amazon 简单存储服务 (S3)

### 识别
-评估你的安全态势以识别和补救安全漏洞
-AWS 开发了一种新的开源 [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) 工具，该工具为客户提供了时间点评估，以获得对其 AWS 安全状况的宝贵见解账户。
-考虑实施 [AWS GuardDuty] (https://aws.amazon.com/guardduty/) 以持续监控恶意活动和未经授权的行为，以保护存储在 Amazon S3 中的 AWS 账户、工作负载和数据
-考虑使用 [AWS Config] (https://aws.amazon.com/config/)，这是一项使您能够评估、审计和评估 AWS 资源配置的服务
-Darkbit 提供了一个 [AWS S3 数据丢失防护] (https://darkbit.io/blog/simple-dlp-for-amazon-s3) 的示例，可用于识别未经授权的对象复制。 还可以根据您的业务使用案例在模式中添加其他特定操作
-维护所有资源的完整资产库存，包括服务器、网络设备、网络/文件共享和开发人员机器

### 保护
-考虑 [启用复制]（http://docs.aws.amazon.com/AmazonS3/latest/dev/crr.html）-同一地区或跨区域。 跨区域复制通过在另一个区域维护第二个副本，从而防止应用程序缺陷和操作员错误
-考虑使用 AWS Config 创建规则来监控 AWS 服务的更改。 AWS Config 预先构建了几个用于监控 EBS 和 EC2 的托管规则，其中包括：
-[s3-account-level-public-access-blocks]（https://docs.aws.amazon.com/config/latest/developerguide/s3-account-level-public-access-blocks.html）
-[s3-account-level-public-access-blocks-periodic] (https://docs.aws.amazon.com/config/latest/developerguide/s3-account-level-public-access-blocks-periodic.html)
-[s3-bucket-blacklisted-actions-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-blacklisted-actions-prohibited.html)
-[s3-bucket-default-lock-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-default-lock-enabled.html)
-[s3-bucket-level-public-access-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-level-public-access-prohibited.html)
-[s3-bucket-logging-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-logging-enabled.html)
-[s3-bucket-policy-grantee-check] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-policy-grantee-check.html)
-[s3-bucket-policy-not-more-permissive] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-policy-not-more-permissive.html)
-[s3-bucket-public-read-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-public-read-prohibited.html)
-[s3-bucket-public-write-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-public-write-prohibited.html)
-[s3-bucket-replication-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-replication-enabled.html)
-[s3-bucket-server-side-encryption-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-server-side-encryption-enabled.html)
-[s3-bucket-ssl-requests-only] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-ssl-requests-only.html)
-[s3-bucket-versioning-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-versioning-enabled.html)
-[s3-default-encryption-kms] (https://docs.aws.amazon.com/config/latest/developerguide/s3-default-encryption-kms.html)
-考虑使用 [AWS Key Management Service (KMS)] (https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) 密钥加密所有对象并防止攻击者应用其加密密钥
-考虑使用 [AWS S3 阻止公共访问功能] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) 来减少对象的意外暴露
-考虑使用 [S3 Intelligent Tiering] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) 进行对象备份和成本优化
-考虑使用 [S3 Object Lock 定] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html) 这样你就可以使用 * 一次写入多读 * (WORM) 模型存储对象。 对象锁定可以帮助防止对象在固定时间内或无限期地被删除或覆盖。
-在对象锁定合规性模式 ** 中，** 受保护的对象版本不能被任何用户覆盖或删除，包括 AWS 账户中的根用户。 当对象处于合规性模式下锁定时，其保留模式不能更改，也不能缩短其保留期。 合规模式 *** 确保在保留期内不能覆盖或删除对象版本。
-启用包含敏感或关键信息的 [S3 存储桶和对象的 CloudTrail 事件日志记录] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html)。 默认情况下，CloudTrail 跟踪不记录数据事件，但是您可以配置跟踪记录指定的 S3 存储桶的数据事件，或者为 AWS 账户中的所有 Amazon S3 存储桶记录数据事件。
-启用 [S3 存储桶的 CloudTrail 服务器级别日志记录] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerLogs.html) 和包含敏感或关键信息的对象。 服务器访问日志记录提供对存储桶发出的请求的详细记录
-启用 [S3 Object Versioning] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) 以允许恢复修改过的对象
-要设置保留的版本数量，[设置生命周期策略] (http://docs.aws.amazon.com/AmazonS3/latest/dev/intro-lifecycle-rules.html) 应用于非当前版本
-启用 [S3 MFA delete] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/MultiFactorAuthenticationDelete.html) 以防止攻击者禁用版本控制和覆盖存储桶中的所有对象
-强制执行多因素身份验证 (MFA)
-强制执行密码复杂性要求并确定到期期
-实施 [CIS AWS Foundations] (https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html)，包括账户到期和强制性证书轮换
-运行 [IAM Credential Report] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) 以列出账户中的所有用户及其各种证书的状态，包括密码、访问密钥和 MFA 设备
-采取措施 [防止任何新凭据公开]（http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html）
-使用 [AWS IAM Access Analyzer] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) 识别组织和账户中的资源，例如与外部实体共享的 Amazon S3 存储桶或 IAM 角色。 这使您可以识别对资源和数据的意外访问权限，这是一种安全风险
-使用 IAM 角色管理权限
-实现最低权限且不允许 s3:\ * 权限
-使用并定期审核存储桶策略
-只公开需要的东西，并确保所有物品都受到私密保护
-虽然我们不能推荐特定的第三方解决方案，但也有我们的合作伙伴（包括 Veritas 和 CommVault）以及其他备份软件提供商的产品，它们具备将 S3 中的对象备份到 S3 内部或外部托管备份存储位置的本机功能

### 检测

-检查 CloudTrail 日志中是否存在未经批准的活动，例如创建未经授权的 IAM 用户、策略、角色或临时安全证书
-检查您的 CloudTrail 日志，查看您的 AWS 账户是否有任何未经授权的 AWS 使用情况，例如未经授权的 EC2 实例、Lambda 函数或 EC2 竞价出价。 您还可以通过登录 AWS 管理控制台并查看每个服务页面来检查使用情况。 也可以检查账单控制台中的 “账单\” 页面是否意外使用
-请记住，任何地区都可能发生未经授权的使用情况，并且您的主机一次只能向您显示一个区域。 要在区域之间切换，可以使用控制台屏幕右上角的下拉菜单
-赎金票据可以作为存储桶内的对象或通过电子邮件提供给客户
-S3 对象被删除或删除整个 S3 存储桶
-注意：对于数据销毁事件，可能会提供也可能不提供赎金票据。 另外，请确保您检查 CloudWatch 指标和 CloudTrail S3 事件，以验证是否发生了数据泄露，是否在赎金或数据销毁攻击之间划分
-S3 对象使用不属于客户所有的账户中的密钥进行加密

### 回应
-删除或轮换 [IAM 用户密钥] (https://console.aws.amazon.com/iam/home#users) 和 [Root 用户密钥] (https://console.aws.amazon.com/iam/home#security_credential)；如果您无法识别已公开的特定密钥或密钥，则可能希望轮换账户中的所有密钥
-删除未经授权的 [IAM 用户]（https://console.aws.amazon.com/iam/home#users。）
-删除未经授权的 [政策]（https://console.aws.amazon.com/iam/home#/policies）
-删除未经授权的 [角色]（https://console.aws.amazon.com/iam/home#/roles）
-如果您的应用程序使用公开的访问密钥，则需要更换密钥。 要替换密钥，首先创建第二个密钥（届时，两个密钥都将处于活动状态），然后修改应用程序以使用新密钥。 然后通过单击控制台中的 “设为非活动状态” 选项来禁用（但不要删除）公开的密钥。 如果你的应用程序有任何问题，你可以重新激活公开的密钥。 当您的应用程序使用新密钥完全正常运行时，请删除暴露的密钥
-[吊销临时证书]（https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time）。 也可以通过删除 IAM 用户来吊销临时证书。 * 注意：* 删除 IAM 用户可能会影响生产工作负载，应谨慎完成

### 恢复
-建议不要支付赎金
-支付赎金是一场赌博，罪犯在收到付款后是否会兑现交易
-如果没有数据备份，那么你应该进行成本效益分析，权衡数据/声誉损失的价值与向攻击者付款的价值
-如果你选择支付赎金，你可以直接让攻击者继续对你的公司或其他人进行操作
-在尝试恢复数据或对象之前，执行 Protect 下的过程
-删除版本控制对象的 [删除标记] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/RemDelMarker.html)
-重新创建删除的存储桶
-使用 [S3 Intelligent Tiering] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) 对象备份或复制区域存储桶还原对象
-* 注意：* 目前 S3 没有\ “取消删除\” 功能，并且 AWS 无法恢复已删除的数据。 在当前数据存储合规性和法规的时代，例如 [GDPR] (https://gdpr-info.eu/) 和 [CCPA] (https://oag.ca.gov/privacy/ccpa)，Amazon S3 无法继续存储从客户账户中明确删除的客户数据。 删除对象后，无论向 AWS 报告意外删除的速度如何，AWS 都无法再恢复对象

## 关系数据库服务 (RDS)

### 识别
-评估你的安全态势以识别和补救安全漏洞
-AWS 开发了一种新的开源 [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) 工具，该工具为客户提供了时间点评估，以获得对其 AWS 安全状况的宝贵见解账户。
-考虑使用 [AWS Config] (https://aws.amazon.com/config/)，这是一项使您能够评估、审计和评估 AWS 资源配置的服务
-考虑实施 [AWS GuardDuty] (https://aws.amazon.com/guardduty/) 以持续监控恶意活动和未经授权的行为，以保护存储在 Amazon RDS 中的 AWS 账户、工作负载和数据
-维护所有资源的完整资产库存，包括服务器、网络设备、网络/文件共享和开发人员机器

### 保护
-[Amazon RDS 中的安全性]（https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.html）中提供了其他参考和步骤
-考虑使用 AWS Config 创建规则来监控 AWS 服务的更改。 AWS Config 预先构建了几个用于监控 RDS 的托管规则，其中包括：
-[rds-automatic-minor-version-upgrade-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-automatic-minor-version-upgrade-enabled.html)
-[rds-cluster-deletion-protection-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-deletion-protection-enabled.html)
-[rds-cluster-iam-authentication-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-iam-authentication-enabled.html)
-[rds-cluster-multi-az-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-multi-az-enabled.html)
-[rds-enhanced-monitoring-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-enhanced-monitoring-enabled.html)
-[rds-instance-deletion-protection-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-deletion-protection-enabled.html)
-[rds-instance-iam-authentication-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-iam-authentication-enabled.html)
-[rds-instance-public-access-check] (https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-public-access-check.html)
-[rds-in-backup-plan]（https://docs.aws.amazon.com/config/latest/developerguide/rds-in-backup-plan.html）
-[rds-logging-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-logging-enabled.html)
-[rds-multi-az-support] (https://docs.aws.amazon.com/config/latest/developerguide/rds-multi-az-support.html)
-[rds-snapshots-public-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshots-public-prohibited.html)
-[rds-snapshot-encrypted] (https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshot-encrypted.html)
-[rds-storage-encrypted] (https://docs.aws.amazon.com/config/latest/developerguide/rds-storage-encrypted.html)
-拥有基于主机的第三方入侵检测系统 (HIDS) 解决方案（例如 OSSEC、Tripwire、Wazuh、[Amazon Inspector]（https://aws.amazon.com/inspector/））
-在基于 Amazon VPC 服务的虚拟私有云 (VPC) 中运行数据库实例，以实现最大限度的网络访问控制。 有关在 VPC 中创建数据库实例的更多信息，请参阅 [亚马逊虚拟私有云 VPC 和 Amazon RDS] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.html)。
-使用 AWS 身份和访问管理 (IAM) 策略分配权限，以确定谁可以管理 Amazon RDS 资源。 例如，您可以使用 IAM 来确定谁可以创建、描述、修改和删除数据库实例、标记资源或修改安全组。
-使用 Amazon RDS 加密来保护静态数据库实例和快照的安全。 Amazon RDS 加密使用行业标准 AES-256 加密算法对托管数据库实例的服务器上的数据进行加密。 有关更多信息，请参阅 [加密 Amazon RDS 资源] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Encryption.html)
-将安全套接字层 (SSL) 或传输层安全性 (TLS) 连接与运行 MySQL、MariaDB、PostgreSQL、Oracle 或 Microsoft SQL Server 数据库引擎的数据库实例结合使用。 有关将 SSL/TLS 与数据库实例结合使用的更多信息，请参阅 [使用 SSL/TLS 加密与数据库实例的连接] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html)
-使用安全组控制哪些 IP 地址或 Amazon EC2 实例可以连接到数据库实例上的数据库。 首次创建数据库实例时，其安全系统会阻止任何数据库访问，除非通过关联安全组指定的规则访问。
-对 Oracle 数据库实例使用网络加密和透明数据加密；有关更多信息，请参阅 [Oracle 本机网络加密] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.NetworkEncryption.html) 和 [Oracle 透明数据加密] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.AdvSecurity.html)
-使用数据库引擎的安全功能来控制谁可以登录数据库实例上的数据库。 这些功能就像数据库位于本地网络上一样工作。

### 检测
-AWS GuardDuty 机器学习可以检测可疑行为，包括作为数据泄露尝试的一部分生成亚马逊关系数据库服务 (Amazon RDS) 快照。 AWS 安全博客中提供了一个示例 [如何使用 Amazon GuardDuty 检测 AWS 账户中的可疑活动] (https://aws.amazon.com/blogs/security/how-you-can-use-amazon-guardduty-to-detect-suspicious-activity-within-your-aws-account/)
-查看 EC2 操作系统和应用程序日志是否存在不当登录、安装未知软件或是否存在无法识别的文件
-使用 vpcFlowLogs 识别来自外部 IP 地址的不当数据库访问
-你可以 [利用 Amazon 侦探调查 VPC 流程] (https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) 或 [Amazon Athena] (https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)
-[AWS Security Analytics Bootstrap] (https://github.com/awslabs/aws-security-analytics-bootstrap) 通过提供快速部署、可用且易于维护的 Amazon Athena 分析环境，使客户能够对 AWS 服务日志执行安全调查

### 回应
-删除或轮换 [IAM 用户密钥] (https://console.aws.amazon.com/iam/home#users) 和 [Root 用户密钥] (https://console.aws.amazon.com/iam/home#security_credential)；如果您无法识别已公开的特定密钥或密钥，则可能希望轮换账户中的所有密钥
-删除未经授权的 [IAM 用户]（https://console.aws.amazon.com/iam/home#users。）
-删除未经授权的 [政策]（https://console.aws.amazon.com/iam/home#/policies）
-删除未经授权的 [角色]（https://console.aws.amazon.com/iam/home#/roles）
-删除未识别或未经授权的公共快照或数据库
-确定有权访问数据库的 EC2 实例并调查这些实例
-如果您的应用程序使用公开的访问密钥，则需要更换密钥。 要替换密钥，首先创建第二个密钥（届时，两个密钥都将处于活动状态），然后修改应用程序以使用新密钥。 然后通过单击控制台中的 “设为非活动状态” 选项来禁用（但不要删除）公开的密钥。 如果你的应用程序有任何问题，你可以重新激活公开的密钥。 当您的应用程序使用新密钥完全正常运行时，请删除暴露的密钥
-[吊销临时证书]（https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time）。 也可以通过删除 IAM 用户来吊销临时证书。 * 注意：* 删除 IAM 用户可能会影响生产工作负载，应谨慎完成

### 恢复
-建议不要支付赎金
-支付赎金是一场赌博，罪犯在收到付款后是否会兑现交易
-如果没有数据备份，那么你应该进行成本效益分析，权衡数据/声誉损失的价值与向攻击者付款的价值
-如果你选择支付赎金，你可以直接让攻击者继续对你的公司或其他人进行操作
-删除受损的数据库并创建新的 RDS 数据库
-遵循监管要求或公司内部政策以确定是否需要 RDS 数据库的取证
-访问 < https://www.nomoreransom.org/ > 以确定是否可用于受感染的数据的恶意软件变体的解密
-使用 CloudEndure 灾难恢复在勒索软件攻击或数据损坏之前选择最新的恢复点，以恢复 AWS 上的工作负载
-如果使用备用数据备份策略，请验证备份没有被感染并从勒索软件事件发生之前的最后一个计划事件中恢复

### 关注事件

### 向当局报告赎金攻击
联邦调查局鼓励各组织向执法部门报告赎金事件。 互联网犯罪投诉中心 (IC3) 接受来自实际受害者或第三方向投诉人的在线互联网犯罪投诉，并将与他们合作确定未来的最佳行动方针。 准备分享：
-你认为支持投诉所必需的任何相关信息
-电子邮件标题
-金融交易信息（账户信息、交易日期和金额、收款人详细信息）
-受试者的姓名、地址、电话、电子邮件、网站和 IP 地址
-关于你是如何受害的具体细节
-受害者的姓名、地址、电话和电子邮件

### 执行根本原因分析
在大多数情况下，您现有的取证工具将在 AWS 环境中运行。 取证团队受益于跨 AWS 区域自动部署工具，以及使用构建业务关键型应用程序所基于的同样强大、可扩展的服务，以及在低摩擦的情况下快速收集大量数据的能力。

[Amazon 侦探] (https://aws.amazon.com/detective/%20) 可以轻松分析、调查和快速识别潜在安全问题或可疑活动的根本原因。 Amazon 侦探会自动从您的 AWS 资源收集 AWS 服务日志数据，并使用机器学习、统计分析和图表理论来构建一组链接的数据，从而使您能够更快、更高效地进行安全调查。

### 借助吸取的经验教训更新安全计划
记录模拟和实时事件期间吸取的经验教训并将其循环回到 “新常态” 流程和程序中，使组织能够更好地了解违反这些流程和程序的情况，例如他们在哪里易受攻击、自动化可能出现故障或缺乏可见性的地方 — 以及有机会加强他们的整体安全态势。

## 训练员工
通过有效的安全意识计划培训员工。 这需要有娱乐性和吸引力。 专注于报告网络钓鱼、通知程序以及如何举报 “坏” 事情，然后授予该行为可能非常有效。

## 结论
赎金攻击正在不断发展，但您的安全意识和准备情况也是如此。 世界各地的政府机构、非营利组织和企业都信任 AWS 来为其基础设施提供动力并确保系统和数据安全。 使用本行动手册中共享的 AWS 服务和最佳实践，您可以采取主动措施，降低 AWS 环境中赎金的可能性和影响。

## 附录 A：防止或检测赎金攻击的服务和工具
[Amazon CloudWatch 合成材料] (https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries.html)
[Amazon CloudWatch] (https://aws.amazon.com/cloudwatch)
[Amazon CodeGuru] (https://aws.amazon.com/codeguru/)
[Amazon EventBridge] (https://aws.amazon.com/eventbridge/)
[Amazon GuardDuty] (https://aws.amazon.com/guardduty/)
[Amazon 检查员网络可达性] (https://docs.aws.amazon.com/inspector/latest/userguide/inspector_network-reachability.html)
[Amazon 督察] (https://aws.amazon.com/inspector/)
[Amazon Macie] (https://aws.amazon.com/macie/)
[Amazon VPC 可达性分析器] (https://docs.aws.amazon.com/vpc/latest/reachability/what-is-reachability-analyzer.html)
[AWS CDK] (https://aws.amazon.com/cdk/)
[AWS Cloud9] (https://aws.amazon.com/cloud9/)
[AWS CloudFormation 守卫] (https://github.com/aws-cloudformation/cloudformation-guard)
[AWS CloudFormation] (https://aws.amazon.com/cloudformation/)
[AWS Code神器] (https://aws.amazon.com/codeartifact/)
[AWS CodeBuild] (https://aws.amazon.com/codebuild/)
[AWS CodeCommit] (https://aws.amazon.com/codecommit/)
[AWS CodeDeploy] (https://aws.amazon.com/codedeploy/)
[AWS CodePipeline] (https://aws.amazon.com/codepipeline/)
[AWS 配置规则] (https://aws.amazon.com/blogs/aws/aws-config-rules-dynamic-compliance-checking-for-cloud-resources/)
[AWS 控制塔] (https://aws.amazon.com/controltower/)
[AWS IAM 访问分析器] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html)
[AWS IAM] (https://aws.amazon.com/iam/)
[AWS KMS] (https://aws.amazon.com/kms/)
[AWS Lambda] (https://aws.amazon.com/lambda/)
[AWS 组织] (https://aws.amazon.com/organizations/)
[AWS 秘密管理器] (https://aws.amazon.com/secrets-manager/)
[AWS 安全中心] (https://aws.amazon.com/security-hub/)
[AWS 步骤函数] (https://aws.amazon.com/step-functions/)
[AWS 系统管理器] (https://aws.amazon.com/systems-manager/)
[AWS WAF] (https://aws.amazon.com/waf/)
[AWSPX] (https://github.com/FSecureLABS/awspx)
[CheckMarx] (https://checkmarx.com/)
[EC2 映像生成器] (https://aws.amazon.com/image-builder/)
[ECR 本机容器映像扫描] (https://aws.amazon.com/blogs/containers/amazon-ecr-native-container-image-scanning/)
[git-secrets]（https://github.com/awslabs/git-secrets）
[PMapper] (https://github.com/nccgroup/PMapper)
[Prowler] (https://github.com/toniblyx/prowler)
[SonarQUBE] (https://www.sonarqube.org/)