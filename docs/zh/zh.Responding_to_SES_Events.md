# 事件响应行动手册：应对简单的电子邮件服务事件

## 联系点

作者：`作者姓名`
批准者：`批准者姓名`
最后批准日期：

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
[AWS 云采用框架安全角度] (https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* ** 指令 **
* ** 侦探 **
* ** 响应 **
* ** 预防性 **

### 响应步骤
1. [** 准备 **] 对 IAM 使用 AWS GuardDuty 检测
2. [** 准备 **] 识别、记录和测试上报程序
3. [** 检测和分析 **] 执行检测和分析 CloudTrail 以查找无法识别的 API 事件
4. [** 检测和分析 **] 执行检测和分析 CloudWatch 是否有无法识别的事件
3. [** 遏制和消除 **] 删除或轮换 IAM 用户密钥
4. [** 遏制和消除 **] 删除或轮换无法识别的资源
5. [** RECOVERY**] 酌情执行恢复程序

*** 响应步骤遵循 [NIST 特别出版物 800-61r2 计算机安全事件处理指南] 中的事件响应生命周期（https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf）

### 事件分类和处理
* ** 策略、技巧和程序 **：
* ** 类别 **:
* ** 资源 **：SES
* ** 指标 **:
* ** 日志来源 **：
* ** 团队 **：安全运营中心 (SOC)、法医调查员、云工程

## 事件处理流程
### 事件响应流程有以下几个阶段：
* 准备
* 检测和分析
* 遏制和根除
* 恢复
* 事件后活动

## 执行摘要
本行动手册概述了应对针对 AWS 简单电子邮件服务 (SES) 的攻击的流程。 结合本指南，请查看 [Amazon SES 发送审核流程常见问题解答] (https://docs.aws.amazon.com/ses/latest/DeveloperGuide/faqs-enforcement.html)，了解执法行动的答案和对不良 SES 使用的回应。

有关更多信息，请查看 [AWS 安全事件响应指南] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

## 准备-一般
* 评估你的安全态势以识别和补救安全漏洞
* AWS 开发了一种新的开源 [自助安全评估] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) 工具，该工具为客户提供了时间点评估，以获得对其 AWS 安全状况的宝贵见解账户。
* 维护所有资源的完整资产清单，包括服务器、网络设备、网络/文件共享和开发人员机器
* 考虑实施 [AWS GuardDuty] (https://aws.amazon.com/guardduty/) 以持续监控恶意活动和未经授权的行为，以保护存储在 Amazon SES 中的 AWS 账户、工作负载和数据
* 实施 [CIS AWS 基金会] (https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html)，包括账户到期和强制性证书轮换
* ** 强制执行多因素身份验证 (MFA) **
* 强制执行密码复杂性要求并确定到期期
* 运行 [IAM 凭证报告] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) 以列出账户中的所有用户及其各种证书的状态，包括密码、访问密钥和 MFA 设备
* 使用 [AWS IAM 访问分析器] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) 识别组织和账户中的资源，例如与外部实体共享的 IAM 角色。 这使您可以识别对资源和数据的意外访问权限，这是一种安全风险

## 准备-SES 特定
* 使用发送授权策略控制谁可以发送电子邮件以及从哪里发送
* https://docs.aws.amazon.com/ses/latest/dg/sending-authorization.html
* 使用配置集来控制允许发送消息的 IP 地址和身份
* https://docs.aws.amazon.com/ses/latest/dg/using-configuration-sets.html
* 考虑为 Amazon SES 使用专用 IP 地址
* https://docs.aws.amazon.com/ses/latest/dg/dedicated-ip.html
* 在 Amazon SES 中管理您自己的邮件和订阅列表以及电子邮件禁止
* https://docs.aws.amazon.com/ses/latest/dg/lists-and-subscriptions.html
* 为实时通知设置 Amazon SES 事件发布
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-using-event-publishing-setup.html
* 在 Amazon SES 中使用最低权限的身份和访问管理
* https://docs.aws.amazon.com/ses/latest/dg/control-user-access.html
* 查看 SES 最佳实践，重点关注安全性和访问权限
* https://docs.aws.amazon.com/ses/latest/DeveloperGuide/best-practices.html（经典）
* https://docs.aws.amazon.com/ses/latest/DeveloperGuide/control-user-access.html（经典）
* 为自己的域设置 SPF、DKIM 和 DMARC，以帮助防止网络钓鱼和欺骗
* https://docs.aws.amazon.com/ses/latest/dg/email-authentication-methods.html

### 潜在的 AWS GuardDuty 检测
以下调查结果特定于 IAM 实体和访问密钥，并且始终具有 AccessKey 的资源类型。 调查结果的严重性和详细信息因查找结果类型而异。 有关每种查找类型的更多信息，请参阅 [GuardDuty IAM 查找类型] (https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-iam.html) 网页。

* 凭据访问权限：iAMERS/ 异常行为
* Defenseevasion: iAMERS/ 异常行为
* 发现：iAMERS/ 异常行为
* 渗透：iAMERS/ 异常行为
* 影响：iAMERS/ 异常行为
* 初始访问：iAMERS/ 异常行为
* 五旬节：iAMER/Kalilinux
* 五旬节：iAMER/ParrotLinux
* 五旬节：iAMER/pentoolLinux
* 持久性：iAMERS/ 异常行为
* 政策：iAMER/root 凭据使用
* 特权升级：iAMERS/ 异常行为
* RECOS: iAMERS/ 恶意拨打呼叫者
* RECOS: iAMER/恶意ipCaller.自定义
* RECOS: iAMERS/ ToripPaller
* 隐身：iAMER/CloudTrail 日志记录已禁用
* 隐身：iAMER/密码政策更改
* 未经授权的访问：iAMERS/ 控制台登录成功。B
* 未经授权的访问权限：iAMER/实例证书Explover.外部AWS
* 未经授权访问：iAMERS/ 恶意拨打呼叫者
* 未经授权的访问：iAMER/ 恶意 IP 呼叫者。自定义
* 未经授权的访问：iAMER/ToripPaller

## 上报程序
-“我需要就什么时候进行 EC2 取证作出业务决定 `
-“谁在监控日志/警报，接收日志/警报并对其采取行动？ `
-“发现警报后谁会收到通知？ `
-“什么时候公共关系和法律参与这一过程？ `
-“你什么时候联系 AWS Support 寻求帮助？ `

## 检测和分析
### CloudTrail
Amazon SES 与 AWS CloudTrail 集成，该服务提供了 Amazon SES 中的用户、角色或 AWS 服务所采取的操作的记录。 CloudTrail 将 Amazon SES 的 API 调用作为事件捕获。 捕获的调用包括来自 Amazon SES 控制台的调用以及对 Amazon SES API 操作的代码调用。

以下是特定的 CloudTrail `eventName` 事件，用于查找与 SES 相关的账户变更：

* 删除身份
* 删除身份策略
* 删除收据过滤器
* 删除收据规则
* 删除收据规则集
* 删除已验证的电子邮件地址
* 获取 SendPquota
* Put身份政策
* 更新收据规则
* 验证 ydomaindKim
* 验证域身份
* 验证电子邮件地址
* 验证电子邮件身份

[使用 CloudTrail 事件历史记录查看活动] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)

### 其他侦探控件
* 记录和监控 SES 事件（例如发送、退回、投诉等）
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-activity.html
* https://aws.amazon.com/blogs/messaging-and-targeting/handling-bounces-and-complaints/
* 监控发件人信誉指标
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sender-reputation.html
* 为 SES 指标设置适当的 Cloudwatch 警报或仪表板
* https://docs.aws.amazon.com/ses/latest/dg/security-monitoring-overview.html

## 遏制和根除
* [删除或轮换 IAM 用户密钥] (https://console.aws.amazon.com/iam/home#users) 和 [root 用户密钥] (https://console.aws.amazon.com/iam/home#security_credential)；如果您无法识别已公开的特定密钥或密钥，则可能希望轮换账户中的所有密钥
* [删除未经授权的 IAM 用户] (https://console.aws.amazon.com/iam/home#users。)
* [删除未经授权的政策] (https://console.aws.amazon.com/iam/home#/policies)
* [删除未经授权的角色] (https://console.aws.amazon.com/iam/home#/roles)
* [吊销临时证书] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time)。 也可以通过删除 IAM 用户来吊销临时证书。
* 注意：删除 IAM 用户可能会影响生产工作负载，应谨慎完成

## 恢复
* 使用最低权限访问策略创建新的 IAM 用户
* 实施 “准备-SES 特定” 部分中的步骤和资源
* 记录和监控 SES 事件（例如发送、退回、投诉等）
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-activity.html
* https://aws.amazon.com/blogs/messaging-and-targeting/handling-bounces-and-complaints/
* 监控发件人信誉指标
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sender-reputation.html
* 为 SES 指标设置适当的 Cloudwatch 警报或仪表板
* https://docs.aws.amazon.com/ses/latest/dg/security-monitoring-overview.html

## 吸取的教训
“这里是一个添加特定于贵公司的物品的地方，这些物品不一定需要 “修复”，但在与运营和业务要求同时执行本行动手册时也很重要。 `

## 已解决积压项目

## 当前积压项目