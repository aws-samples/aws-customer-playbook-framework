# 针对被盗的 AWS 账户证书的安全手册

## 简介

作为我们对客户的持续承诺的一部分，AWS 正在提供这个
安全事件响应手册，描述了执行以下操作所需的步骤
检测并响应您的 AWS 内被泄露的证书
账户。 本文档的目的是提供规范性
有关在您怀疑发生安全事件后应采取的措施的指南
发生了。

![Image](/images/nist_life_cycle.png)

*AWS 事件响应的各个方面*

## 准备

为了快速有效地执行事件响应
活动，至关重要的是要做好人员、流程和
组织内部的技术。 查看 [*AWS 安全事件
回应
指南*] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/preparation.html)，
并采取必要步骤，帮助确保所有人做好准备
三个域名。

## 如何联系 AWS 支持部门寻求帮助

一旦您怀疑存在泄露事件，就必须立即通知 AWS，这一点很重要
您的 AWS 账户或组织中的证书。 这里有
接触 AWS 支持的步骤：

### 提交 AWS 支持案例

-登录您的 AWS 账户：

-这是第一个受到安全影响的 AWS 账户
事件，以验证 AWS 账户所有权。

-注意：如果您无法访问账户，请使用 [this
表格] (https://support.aws.amazon.com/#/contacts/aws-account-support/)
提交支持请求。

-选择 “*服务*”、“*支持中心*”、“*创建案例*”。

-选择问题类型 “*账户和账单” *。

-选择受影响的服务和类别：

-示例：

-服务：账户

-类别：安全

-选择严重性：

-企业支持或入口客户：*关键业务风险
问题*。

-业务支持客户：*紧急业务风险问题*。

-描述您的问题或问题：

-详细描述您遇到的安全问题
经历、受影响的资源和业务影响。

-首次识别安全事件的时间。

-迄今为止的事件时间表摘要。

-您收集的工件（屏幕截图、日志文件）。

-您怀疑遭到泄露的 IAM 用户或角色的 ARN。

-受影响资源和业务影响的描述。

-您组织中的参与程度（例如：“这种安全性
该活动吸引了首席执行官和董事会的知名度
导演”）。

-可选：添加其他问题联系人。

-选择 “*联系我们*”。

-首选联系语言（可能视供应情况而定）

-首选联系方式：网络、电话或聊天（推荐）

-可选：其他案例联系人

<!---->

-*点击 “提交” *

-**升级**：请尽快通知您的 AWS 账户团队
可能，因此他们可以动用必要的资源并升级为
需要。

**注意：** 验证您的 “备用安全” 非常重要
“联系人” 是为每个 AWS 账户定义的。 欲了解更多详情，请参阅
到 [这个
文章] (https://aws.amazon.com/blogs/security/update-the-alternate-security-contact-across-your-aws-accounts-for-timely-security-notifications/)。

## 检测

有多种方法可以检测您内部的已泄露凭证
AWS 环境。 列出了一些检测潜在指标的方法
妥协（国际奥委会）。 有关国际奥委会的详细名单，请参阅 [附录
A.] (#appendix-a-reviewing-logs) **注意**：记录每篇文章的详细信息
怀疑国际奥委会需要进一步分析。

1。 评论亚马逊 [GuardDuty
调查结果] (https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_findings.html)。
以下发现与可疑或异常活动有关
由 IAM 实体提供。 查看 [调查结果
详情] (https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-iam.html)，
如果活动出乎意料，则可能表示受到了攻击
凭证。

1。 CredentialAccess:IAMUser/AnomalousBehavior

2。 DefenseEvasion:IAMUser/AnomalousBehavior

3。 Discovery:IAMUser/AnomalousBehavior

4。 Exfiltration:IAMUser/AnomalousBehavior

5。 Impact:IAMUser/AnomalousBehavior

6。 InitialAccess:IAMUser/AnomalousBehavior

7。 PenTest:IAMUser/KaliLinux

8。 PenTest:IAMUser/ParrotLinux

9。 PenTest:IAMUser/PentooLinux

10。 Persistence:IAMUser/AnomalousBehavior

11。 Policy:IAMUser/RootCredentialUsage

12。 PrivilegeEscalation:IAMUser/AnomalousBehavior

13。 Recon:IAMUser/MaliciousIPCaller

14。 Recon:IAMUser/MaliciousIPCaller.Custom

15。 Recon:IAMUser/TorIPCaller

16。 Stealth:IAMUser/CloudTrailLoggingDisabled

17。 Stealth:IAMUser/PasswordPolicyChange

18。 UnauthorizedAccess:IAMUser/ConsoleLoginSuccess.B

19。 UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration

20。 UnauthorizedAccess:IAMUser/MaliciousIPCaller

21。 UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom

22。 UnauthorizedAccess:IAMUser/TorIPCaller

2。 查看 AWS 账单，了解异常峰值，这可能是账户的迹象
并按照以下步骤泄露凭证：

1。 登录 AWS 管理控制台并打开 [账单]
控制台] (https://console.aws.amazon.com/billing/)。

2。 选择 “*账单” *以查看有关您当前费用的详细信息。

3。 选择 “*付款” *以查看您的历史付款交易。

4。 选择 “*AWS 成本和使用率报告” * 以查看中断的报告
降低您的成本。

3。 前往 IAM 控制台下载 IAM 证书报告，然后
在 “*访问权限” 下选择左侧的 “*凭证报告” *
报告” * 然后查看以下内容：

1. 通过查看创建日期来识别异常的 IAM 用户创建情况
以及 “上次使用/更改的密码” 列。

2。 检查是否有任何 IAM 用户拥有两个或两个以上的访问密钥。

3。 检查是否有 IAM 用户有
*awsexposedCredentialPolicy\ _DO\ _NOT\ _REMOVE* 已附上。 如果是这样，
轮换其访问密钥。

4。 查看 AWS 账户中的 IAM 角色，找出任何不熟悉的角色
已创建或访问的角色

1。 在 AWS 控制台中，选择 “*Service*s”、“*IAM*” 和
“*角色*”

2。 查看所有不熟悉的 “*角色名称*”。

3。 点击角色名称，然后查看列出的详细信息：

1。 角色创建日期

2。 ARN

3。 上次活动

4。 单击 “*权限*” 查看附加的 IAM 政策
到这个角色。

5。 单击 “*信任关系*” 查看可以假设的实体
角色。

6。 单击 “*Access Advisor*” 以显示已有哪些服务
由角色访问，以及 “*上次访问日期*”。

5。 检测您的 AWS 中任何无法识别或未经授权的资源
账户，例如：

1。 通过在 AWS CLI 中运行以下命令来实现 EC2 实例
终端 “*aws ec2 描述实例*” 并验证
结果。 使用查找每个实例的创建时间
*--查询 '预留\ [\] .Instances\ [\]。 {ip：公共 IP 地址，
tm: launchTime} '--filters 'name=tag: Name, Values=
myInstanceName' | jq 'sort\ _by (.tm) | reverse |.\ [0\] '* 过滤器。

2。 通过在 AWS CLI 中运行以下命令来实现 Lambda 的功能
终端 “*aws lambda 列表函数*” 并验证最后一个
使用 “| *grep “LastModified*” 过滤器修改时间。

6。 通过以下方式查看 AWS 发出的有关您账户的所有安全通知
登录 [AWS 支持]
那么 Center] (https://support.console.aws.amazon.com/support/home#/)
阅读并回复消息。

7。 按照以下步骤查看调查结果：

1。 打开 [IAM 控制台] (https://console.aws.amazon.com/iam/)。

2。 从 Access 下的左栏中选择 “*访问分析器” *
报告。

3。 在 “*活跃发现” *下，查看调查结果以确定资源
在您的组织中，例如 S3 存储桶、IAM 角色或 Lambda
与外部实体共享的函数。

## 分析

一旦您发现了任何可疑的资源或活动，这些资源或活动可能会发生
指出漏洞，在您的 SIEM 中进行进一步分析，或记录日志
分析工具。 AWS 有各种安全服务工具可以提供帮助
分析安全事件。 其中一些工具包括：

1。 **Amazon Guardduty**-亚马逊 GuardDuty 是一种威胁检测
持续监控恶意活动的服务
为保护您的 AWS 账户而采取的未经授权的行为，亚马逊 Elastic
计算云 (EC2) 工作负载、容器应用程序、亚马逊 Aurora
数据库以及存储在亚马逊简单存储服务 (S3) 中的数据。

2。 **AWS 安全中心**-AWS 安全中心是一种云安全态势
自动化、连续执行的管理 (CSPM) 服务
根据您的 AWS 资源进行安全最佳实践检查，以帮助您
识别错误配置并汇总您的安全警报
（即调查结果）采用标准化格式，这样您就可以更轻松地使用
充实、调查和补救它们。

3。 **亚马逊侦探**-亚马逊侦探使其更易于分析，
调查并快速找出潜在的根本原因
安全问题或可疑活动。

您还可以通过以下方式搜索您的 AWS CloudTrail 事件历史记录
以下是分析和收集证据的步骤:

1。 分析 AWS 云端追踪日志，了解以下内容：

1。 任何与登录相关的异常活动：

1。 前往 [CloudTrail]
仪表板] (https://console.aws.amazon.com/cloudtrail)。

2。 在左侧，选择 “事件历史记录”。

3。 在下拉菜单中，将 “只读” 更改为 “活动名称”。

4。 通过以下方式查看可用事件中是否存在任何可疑活动
通过搜索框搜索以下术语：
“*consoleLogin*”、“*AssumeRole*”、“*getFederationToken*”、
“*获取凭证报告*”，“*生成凭证报告*”。 还有
请务必注意 “*用户身份*” 应该会出现
作为 “*type*”：root 用户的 “*root*” 或 “*type*”：
“*iamUser*” 表示账户中的任何本地 IAM 用户。

<!---->

1。 找到用于启动的任何 IAM 访问密钥 ID 和用户名
可疑的亚马逊 EC2 实例：

1。 打开 AWS CloudTrail 控制台并选择 “*事件”
历史记录” * 来自导航窗格。

2。 选择 “*查找属性” * 下拉菜单，然后
选择 “*资源名称” *。

3。 在输入资源名称字段中，粘贴 EC2 实例 ID，
然后在你的设备上按回车键。

4。 展开*RunInstances* 的事件名称。

5。 复制 AWS 访问密钥，并记下用户名。

2。 查看 AWS CloudTrail 事件历史记录，了解活动情况
访问密钥泄露：

1。 打开 CloudTrail 控制台并选择 “*事件”
> 历史记录” * 来自导航窗格。

2。 选择 “*查找属性” * 下拉菜单，然后
> 选择 “*AWS 访问密钥” *。

3。 在 “*输入 AWS 访问密钥字段” * 中，输入
> 已泄露的 IAM 访问密钥 ID。

4。 展开 API 调用 *runInstances* 的事件名称。

3。 如果您通过第三方身份提供商访问 AWS，那么
一定要审核访问日志并遵循供应商的指导方针
关于响应事件并保护您的环境。

<!---->

1。 分析 IAM Access Advisor 泄露的凭证
通过以下信息查看它上次访问的服务是什么
这些步骤：

1。 前往 [IAM 控制台] (https://console.aws.amazon.com/iam)。

2。 前往用户或角色，点击受感染者的姓名
校长，选择 “*访问顾问” * 选项卡并查看哪个
上次访问的资源。

## 遏制

在分析并收集了有关受感染者的更多信息之后
凭证和任何其他受影响的资源，是时候控制和
抑制安全事件。

1。 禁用 IAM 用户，创建备份 IAM 访问密钥，然后
按照以下步骤禁用被泄露的访问密钥：

1。 打开 [IAM 控制台] (https://console.aws.amazon.com/iam/) 然后
将 IAM 访问密钥 ID 粘贴到搜索 IAM 栏中。

2。 选择用户名，然后选择 “*安全
凭证*” 选项卡。

3。 在控制台密码字段中，选择 “*管理*”。

4。 在 “控制台访问” 中，选择 “*禁用*”，然后选择 “应用”。

5。 对于泄露的 IAM 访问密钥，请选择 “*设为非活动*”。

2。 按照以下步骤轮换访问密钥：

1。 首先，前往 [IAM] 创建第二个密钥
控制台] (https://console.aws.amazon.com/iam/)。

2。 在导航窗格中，选择 “*用户” *。

3。 选择目标用户的姓名，然后选择
“*安全证书” * 选项卡。

4。 在 “*访问密钥” * 部分中，选择 “*创建访问密钥” *。 开启
“*访问密钥最佳实践*和替代方案” *页面，
选择 “*其他” *，然后选择 “*下一步” 。 *

5。 然后，修改您的应用程序以使用新密钥。

6。 禁用（但不要删除）第一个密钥。

7。 如果您的应用程序有任何问题，请重新激活
暂时密钥。 当您的应用程序功能齐全时，并且
第一个密钥被禁用，只有这样才可以安全地删除
第一把钥匙。 确保记录所有已删除的访问权限
用于继续在 AWS CloudTrail 日志中搜索它们的密钥。

3。 按照以下步骤撤消 IAM 角色的活动会话：

1。 打开 [IAM 控制台] (https://console.aws.amazon.com/iam/) 然后
转到角色并点击你要激活的 IAM 角色激活
的会话。

2。 单击 IAM 角色名称并转至 “*撤消会话” * 选项卡。

3。 单击 “*撤消活动会话*” 按钮并确认该步骤。

4。 按照以下步骤隔离受影响的资源：

1。 对于亚马逊 EC2 实例，请前往 EC2 实例控制台，选中
在要隔离的 EC2 实例旁边的方框中，单击
*操作*，点击*安全*，然后点击*更改安全性
团体*。 分离所有当前的安全组并附加
阻止入站和出站的隔离安全组
与 EC2 的通信。

2。 对于亚马逊 S3 存储桶，请使用 [存储桶
政策] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/add-bucket-policy.html)
阻止任何可疑 IP 地址访问 S3
水桶。

5。 撤消身份中心会话：

在 Identity Center 中，有两个会议需要关注
哪些是访问门户会话和角色/应用程序会话：

1。 访问门户会话：

1。 在 “身份中心” 中禁用用户：

1。 导航到 Identity Center 控制台并选择 “用户”

2。 选择被禁用的用户的用户名

3。 在用户的 “常规信息” 框中，单击 “禁用”
> 用户访问权限'

2。 撤消所有活跃的会话：

1。 在 Identity Center 的用户页面上，选择 “激活”
> “会话” 选项卡

2。 选择任何列出的会话，然后单击 “删除会话”

2。 撤消角色会话：

1。 确定用户正在使用的权限集。

1。 在 Identity Center 控制台中，单击 “权限集”

2。 选择权限集的名称。

3。 向下滚动到 “内联策略”，然后单击 “编辑” 按钮

4。 添加以下策略：

{

“版本”：“2012-10-17”，

“声明”:\ [

{

“效果”：“拒绝”，

“操作”：“\ *”，

“资源”：“\ *”，

“状况”：{

“StringEquals”: {

“IdentityStore: userID”: “示例”

}，

“DateLessThan”：{

“aws: tokenissueTime”：“2023-09-26T 15:00:00.000 Z”

}

}

}

\]

}

对于此政策，将 “示例” 更新为用户的身份中心用户 ID。
用户ID可以在用户的 “一般信息” 框中找到
用户页面。 aws: tokenissueTime 的值应等于中的时间
您应用本政策的内容。

1。 应用程序会话

应用程序会话是在用户访问第三方时创建的
应用程序或 AWS 服务直接来自访问门户。

要撤销申请会话，请查看其文档
已访问的应用程序。

1。 身份中心遭到入侵

如果您的身份提供商遭到入侵，则应阻止访问
来自被盗身份提供商（即外部）的身份中心
基于 SAML 的身份提供者（或活动目录），方法是更改
身份源到 “身份源目录”。

要更改您的身份来源，请执行以下操作：

6.1 导航到身份中心控制台

6.2 选择 “设置”

6.3 在 “设置” 页面上，单击 “身份来源” 选项卡

6.4 点击 “操作” 按钮，然后选择 “更改身份”
来源'

6.5 在 “选择身份来源” 上选择 “身份中心目录”
页

6.6 点击 “下一步” 按钮

6.7 阅读 “查看并确认” 框中包含的信息，

6.8 在相应的字段中输入 “接受”，然后单击 “更改”
“身份来源” 按钮

请注意，如果您通过第三方身份提供商访问 AWS，
请务必审核访问日志，并遵循供应商的指导方针
响应事件并保护您的环境。

另外，如果您要从 Active Directory 更改为身份中心
目录，则所有用户和组都将被删除。 权限集将
不会删除，但权限集分配将被删除。 如果你
正在从基于 SAML 的外部身份提供者改变，所有用户
群组将保留在 Identity Center 中
权限集分配

## 根除

控制完安全事件后，就该开始工作了
关于移除和清理安全事件的原因。

1。 移除由泄露的密钥创建的所有资源
在 “分析阶段步骤 1” 中检测到。 查看所有 AWS 区域，甚至
您不在其中启动 AWS 资源的地区。 如果你需要
保留调查资源，考虑对其进行备份。

2。 检查和
[删除] (https://repost.aws/knowledge-center/terminate-resources-account-closure)
您的账户中正在运行的任何无法识别的服务。 支付
特别注意以下资源:

1。 EC2 实例和 AMI，包括已停止的实例
州。

2。 EBS 卷和快照。

3。 AWS Lambd 函数和层。

3。 从记录 S3 存储桶或日志中移除不必要的权限
在 “检测阶段步骤 6” 中确定的聚合资源
可以用来避免被发现。 这可以让你识别出意外情况
访问您的资源和数据。

4。 移除操作不需要的所有暴露数据。

5。 对面向公众进行安全漏洞扫描
资源。 你可以使用 [亚马逊] 之类的工具
督察] (https://docs.aws.amazon.com/inspector/latest/user/scanning-resources.html)
扫描操作系统和<sup>第三方</sup>软件应用程序
漏洞。

## 恢复

一旦你完成了消除安全事件原因的工作。 是时候了
将受影响的资源恢复到众所周知的状态。

1。 从早于已知的干净备份中恢复必要的数据
事件:

1。 [从亚马逊 EBS 快照或
AMI] (https://docs.aws.amazon.com/prescriptive-guidance/latest/backup-recovery/restore.html)。

2。 [从数据库恢复
快照] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_RestoreFromSnapshot.html) 亚马逊
RDS。

3。 [恢复之前的
版本] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/RestoringPreviousVersions.html) 亚马逊
S3 对象版本。

2。 如有必要，从头开始重建系统，包括重新部署
使用自动化功能从可信来源获取，有时会存入新的 AWS 账户。

3。 如果丢失了对多因素身份验证 “MFA” 的访问权限，而您却没有
请为你的根账户注册另一台 MFA 设备，
请按照以下步骤操作：

1。 登录 [AWS 管理]
主机] (https://console.aws.amazon.com/) 作为账户所有者
选择 Root 用户并输入您的 AWS 账户邮箱
地址。 在下一页上，输入您的密码。

2。 在 “需要额外验证” 页面上，选择 MFA
进行身份验证的方法，然后选择 “下一步”。

3。 根据您使用的 MFA 类型，您应该会看到
页面不同，但是 “*MFA 疑难解答” * 选项起作用
同样的。 在 “*需要额外验证” *页面上
或 “*多因素身份验证” * 页面，选择 “*疑难解答
MFA” *。

4。 如果需要，请再次键入密码并选择*登录*。

5。 在 “*对身份验证设备进行故障排除” *页面上，在
“*使用替代因素登录
身份验证” * 部分，选择 “*使用其他方式登录
因素” *。

6。 在 “*使用替代因子登录” 上
身份验证” * 页面，通过验证来验证您的账户
电子邮件地址，选择 “*发送验证电子邮件” *

7。 请查看与您的 AWS 账户关联的电子邮件以获取
来自亚马逊云科技的消息
(recover-mfa-no-reply@verify.signin.aws)。 按照指示行事
在电子邮件中。

> 如果您的账户中没有看到该电子邮件，请查看您的垃圾邮件文件夹，或者
> 返回浏览器并选择 “*重新发送电子邮件*”。

1。 验证电子邮件地址后，您可以继续进行身份验证
你的账户。 要验证您的主要联系人电话号码，
选择 “立即给我打电话”。

2。 接听 AWS 的来电，出现提示时，输入 6 位数字
手机键盘上的 AWS 网站上的号码。

> 如果您没有接到 AWS 的来电，请选择 “登录” 登录
> 再次控制台然后重新开始。 或者参见 [多因子丢失或无法使用]
> 身份验证 (MFA)
> 设备] (https://support.aws.amazon.com/#/contacts/aws-mfa-support) 到
> 联系支持人员寻求帮助。

1。 验证电话号码后，您可以登录您的帐户
> 选择 “登录控制台”。

2。 根据您使用的 MFA 类型，下一步会有所不同：

1。 对于虚拟 MFA 设备，请将该账户从您的设备中移除。
> 然后前往 [AWS 安全]
> 凭证] (https://console.aws.amazon.com/iam/home？ #security_credential) 页面
> 并在创建之前删除旧的 MFA 虚拟设备实体
> 一个新的。

2。 要获取 FIDO 安全密钥，请前往 [AWS 安全
> 凭证] (https://console.aws.amazon.com/iam/home？ #security_credential) 页面
> 并在启用新的 FIDO 安全密钥之前停用旧的 FIDO 安全密钥
> 一个。

3。 如需硬件 TOTP 令牌，请联系第三方提供商获取
> 帮助修复或更换设备。 你可以继续签名
> 在你之前使用其他身份验证因素
> 接收您的新设备。 有了新硬件 MFA 之后
> 设备，前往 [AWS 安全]
> 凭证] (https://console.aws.amazon.com/iam/home？ #security_credential) 页面
> 然后在你之前删除旧的 MFA 硬件设备实体
> 创建一个新的。

<!---->

1。 确认 IAM 委托人拥有适当的访问权限和权限
在安全事件发生之后。

2。 使用 [AWS] 解决漏洞并安装必要的补丁
SSM 补丁
经理] (https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager.html)
或您使用的任何其他<sup>第三方</sup>工具。

3。 将所有受损/损坏的文件替换为来自的干净版本
备份。

## 事后活动

成功从安全事件中恢复过来后，您
应进行 [*AWS 安全事件] 中概述的练习
事后响应指南
activity*] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/establish-framework-for-learning.html)。
这将帮助您记录从事件中吸取的教训，衡量
并提高您的事件响应能力的有效性，
并实施额外的控制措施以防止类似的事件发生
反复出现。

## 结论

在本剧本中，我们描述了在以下情况下应采取的初始步骤
您的 AWS 账户内发生凭证安全泄露事件。
这包括接触 AWS 支持、检测漏洞、分析
您账户中的事件，包含安全事件，
消除威胁，将您的账户恢复到 “已知良好”
运营状态和事后活动，包括教训
学会了。 下一步，请查看以下 AWS 资源
帮助提高您的事件响应能力：

1。 [AWS 安全事件响应
指南] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/aws-security-incident-response-guide.html)

2。 [AWS 客户手册框架：针对受损的 IAM
凭证] (https://github.com/aws-samples/aws-customer-playbook-framework/blob/main/docs/Compromised_IAM_Credentials.md)

3。 [中的 AWS 安全最佳实践
IAM] (https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

4。 [AWS 架构完善的框架 — 安全
Pillar] (https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html)

## 附录 A — 查看日志

**CloudTrail 日志结构**

在搜索 CloudTrail 日志时，了解这一点很重要
CloudTrail 事件记录中包含的各个字段。 对于
完整列表，请参阅 [<u>官方 CloudTrail]
</u>文档] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-record-contents.html)。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>字段</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>EventTime</td>
<td>协调完成请求的日期和时间
世界时间 (UTC)</td>。
</tr>
<tr class="even">
<td>事件名称</td>
<td>请求的操作，这是 API 中的操作之一
那项服务</td>
</tr>
<tr class="odd">
<td>EventSource</td>
<td>向其发出请求的服务。 这个名字通常是
服务名称的缩写形式，不带空格加上.amazonaws.com。</td>
</tr>
<tr class="even">
<td>用户身份</td>
<td>有关发出请求的 IAM 身份的信息。 欲了解更多
信息，请参阅 <a
<u>href=” https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html “> CloudTrail
用户身份元素</u></a></td>。
</tr>
<tr class="odd">
<td>资源</td>
<td><p>活动中访问的资源列表。 该字段可以包含
以下信息：</p>
<ul>
<li><p>资源 ARN</p></li>
<li><p>资源所有者的账户 ID</p></li>
<li><p>资源类型标识符的格式为：
AWS:: aws-service-name::</p></li> 数据类型名称
</ul></td>
</tr>
<tr class="even">
<td>AWS 区域</td>
<td>向其发出请求的 AWS 区域，例如 us-east-2</td>
</tr>
<tr class="odd">
<td>源 IP 地址</td>
<td>发出请求的 IP 地址。 对于那样的动作
源自服务控制台，报告的地址用于
底层客户资源，而不是控制台 Web 服务器。</td>
</tr>
</tbody>
</table>

**活动**

威胁行为者在之后会采取许多行动
泄露账户凭证。 虽然列出所有内容是不切实际的
可能的操作，以下是一些模式可供您搜索
调查。

**注意：** 以下 API 操作不一定表示安全
事件已经发生。 评估上下文中所有记录的活动
你的调查。

**对 SAML/OIDC 身份提供商配置的更改**

威胁行为者可以创建或修改 SAML/OIDC 身份提供商
配置以避免被检测并在您的内部保持持久性
云环境。

<table>
<colgroup>
<col style="width: 32%" />
<col style="width: 67%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>API 操作</strong></th>
<th><strong>描述</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>我是：创建 SAMLProvider</td>
<td>创建描述身份提供商 (IdP) 的 IAM 资源
支持 SAML 2.0</td>
</tr>
<tr class="even">
<td>我是：删除 SAML 提供者</td>
<td>删除 IAM 中的 SAML 提供商资源。</td>
</tr>
<tr class="odd">
<td>我是：更新 samlProvider</td>
<td>更新现有 SAML 提供商的元数据文档</td>
</tr>
<tr class="even">
<td>我是：创建 OpenIDConnect Provider</td>
<td>创建一个 IAM 实体来描述身份提供商 (IdP)
支持 OpenID Connect (OIDC)</td>。
</tr>
<tr class="odd">
<td>我是：将 ClientID 添加到 OpenidConnect 提供商</td>
<td>向客户列表中添加新的客户端 ID（也称为受众）
已经为指定的 IAM OpenID Connect (OIDC) 注册了身份证
提供者资源</td>
</tr>
<tr class="even">
<td>我是：删除 OpenidConnectProvider</td>
<td>删除中的 OpenID Connect 身份提供商 (IdP) 资源对象
IAM</td>
</tr>
<tr class="odd">
<td>我是：从 OpenidConnect Provider 中移除客户端 ID</td>
<td>从中移除指定的客户端 ID（也称为受众）
为指定 IAM OpenID Connect 注册的客户端 ID 列表
(OIDC) 提供者资源对象</td>
</tr>
<tr class="even">
<td>我是：更新 OpenidConnect ProviderThumbprint</td>
<td>替换现有的服务器证书指纹列表
与 OpenID Connect (OIDC) 提供者资源对象相关联
新的指纹清单</td>
</tr>
</tbody>
</table>

**对 IAM 配置的更改**

威胁行为者可以创建或修改 IAM 委托人、证书或
避免被发现或在云中保持持久性的权限
环境。

<table>
<colgroup>
<col style="width: 28%" />
<col style="width: 71%" />
</colgroup>
<thead>
<tr class="header">
<th>API 操作</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>我是：更改密码</td>
<td>更改调用此命令的 IAM 用户的密码
</td>操作
</tr>
<tr class="even">
<td>我是:创建用户</td>
<td>为您的 AWS 账户创建一个新的 IAM 用户</td>
</tr>
<tr class="odd">
<td>我是:createRole</td>
<td>为您的 AWS 账户创建一个新角色</td>
</tr>
<tr class="even">
<td>我是：创建群组</td>
<td>创建新群组</td>
</tr>
<tr class="odd">
<td>我是：附上用户政策</td>
<td>将指定的托管策略附加到指定用户</td>
</tr>
<tr class="even">
<td>我是：附加角色政策</td>
<td>将指定的托管策略附加到指定的 IAM 角色</td>
</tr>
<tr class="odd">
<td>我是：附加群组策略</td>
<td>将指定的托管策略附加到指定的 IAM
</td>组
</tr>
<tr class="even">
<td>我是:列出访问密钥</td>
<td>返回与关联的访问密钥 ID 的相关信息
指定的 IAM 用户</td>
</tr>
<tr class="odd">
<td>我是：创建策略版本</td>
<td>创建指定托管策略的新版本</td>
</tr>
<tr class="even">
<td>我是：更新登录资料</td>
<td>更改指定 IAM 用户的密码</td>
</tr>
<tr class="odd">
<td>我是：创建访问密钥</td>
<td>创建新的 AWS 私有访问密钥和相应的 AWS 访问密钥
指定用户的 ID</td>
</tr>
<tr class="even">
<td>我是：更新访问密钥</td>
<td>将指定访问密钥的状态从 “活动” 更改为
处于非活动状态，反之亦然。</td>
</tr>
<tr class="odd">
<td>我是：停用 emfa 设备</td>
<td>停用指定的 MFA 设备并将其从关联中移除
使用最初启用它的用户名</td>
</tr>
</tbody>
</table>

**对日志和监控配置的更改**

威胁行为者可能会禁用资源监控或删除日志以避免
侦测或阻止事件调查。

<table>
<colgroup>
<col style="width: 46%" />
<col style="width: 53%" />
</colgroup>
<thead>
<tr class="header">
<th>CloudTrail：删除跟踪</th>
<th>删除跟踪 — 禁用向某人传送 CloudTrail 事件
亚马逊 S3 存储桶、CloudWatch Logs 或亚马逊 EventBridge</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>CloudTrail: 停止记录</td>
<td>暂停记录 AWS API 调用和日志文件传输
指定的轨迹</td>
</tr>
<tr class="even">
<td>CloudTrail：更新轨迹</td>
<td>更新指定日志文件传送的设置</td>
</tr>
<tr class="odd">
<td>CloudTrail：删除事件数据存储</td>
<td>删除 <a
<u>href=” https://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store.html “> CloudTrail
湖畔活动日期存储</u></a></td>
</tr>
<tr class="even">
<td>GuardDuty：删除探测器</td>
<td>删除亚马逊 GuardDuty 探测器并在其中禁用 GuardDuty
特定区域</td>
</tr>
<tr class="odd">
<td>GuardDuty：删除发布目的地</td>
<td>删除将结果导出到的发布目标
</td>到
</tr>
<tr class="even">
<td>Access Analyzer：删除分析器</td>
<td>删除指定的分析器，禁用 IAM 访问分析器
为该地区提供服务。</td>
</tr>
<tr class="odd">
<td>配置：删除配置规则</td>
<td>删除指定的 AWS 配置规则及其所有评估
</td>结果
</tr>
<tr class="even">
<td>配置：删除配置记录器</td>
<td>删除 AWS 配置配置记录器</td>
</tr>
<tr class="odd">
<td>配置：删除配送频道</td>
<td>删除 AWS 配置的交付渠道</td>
</tr>
<tr class="even">
<td>配置：停止配置记录器</td>
<td>停止记录的配置和配置更改
指定的录制组</td>
</tr>
</tbody>
</table>

**S3 存储桶配置更改**

威胁行为者可以修改或删除 S3 存储桶配置，以便
泄露数据。

<table>
<colgroup>
<col style="width: 43%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th>s3: listBuckets</th>
<th>返回经过身份验证的发送者拥有的所有存储桶的列表
该请求</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>s3: 创建存储桶</td>
<td>创建新的存储桶</td>
</tr>
<tr class="even">
<td>s3: 删除存储桶公共访问块</td>
<td>移除亚马逊 S3 的 PublicAccessBlock 配置
</td>桶
</tr>
<tr class="odd">
<td>s3: putbucketPolic</td>
<td>在存储桶上添加或更新策略</td>
</tr>
<tr class="even">
<td>s3: 删除存储桶策略</td>
<td>从存储桶中删除策略</td>
</tr>
<tr class="odd">
<td>s3: putObjectaCL</td>
<td>设置中对象的访问控制列表 (ACL) 权限
亚马逊 S3 存储桶</td>
</tr>
<tr class="even">
<td>s3: deleteObjectacl</td>
<td>删除对象的访问控制列表 (ACL)</td>
</tr>
<tr class="odd">
<td>s3: putBucketCors</td>
<td>为您的存储桶设置 CORS 配置</td>
</tr>
<tr class="even">
<td>s3: 删除 BucketCors</td>
<td>删除为存储桶设置的 CORS 配置信息</td>
</tr>
<tr class="odd">
<td>s3: putBucketenCrypti</td>
<td>为配置默认加密和亚马逊 S3 存储桶密钥
现有存储桶</td>
</tr>
<tr class="even">
<td>s3: 删除存储桶加密</td>
将@@ <td>存储桶的默认加密重置为服务器端
使用亚马逊 S3 托管密钥进行加密 (SSE-S3)</td>
</tr>
</tbody>
</table>

**其他操作**

以下是其他一些您应该在操作期间重点介绍的操作
调查

<table>
<colgroup>
<col style="width: 43%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th>kms: disableKey</th>
<th>将 KMS 密钥的状态设置为已禁用。 此更改是暂时性的
防止使用 KMS 密钥进行加密操作</th>。
</tr>
</thead>
<tbody>
<tr class="odd">
<td>kms: 计划密钥删除</td>
<td>计划删除 KMS 密钥。</td>
</tr>
<tr class="even">
<td>kms: putKeypolicy</td>
<td>将密钥策略附加到指定的 KMS 密钥。 关键政策是
控制 KMS 密钥访问权限的主要方法。</td>
</tr>
<tr class="odd">
<td>kms: createKey</td>
在您的 <td>AWS 账户中创建唯一的客户托管 KMS 密钥，然后
区域。</td>
</tr>
<tr class="even">
<td>ec2: 描述实例</td>
<td>描述您的账户中的 EC2 实例。</td>
</tr>
<tr class="odd">
<td>ec2: 运行实例</td>
<td>在您的账户中启动 EC2 实例。</td>
</tr>
<tr class="even">
<td>ec2: 创建 VPC</td>
<td>在您的账户中创建 VPC。</td>
</tr>
<tr class="odd">
<td>ec2: describeVPC</td>
<td>描述您账户中的 VPC。</td>
</tr>
<tr class="even">
<td>ec2: 创建安全组</td>
<td>创建安全组。 安全组充当虚拟组
您的实例的防火墙用于控制入站和出站流量。</td>
</tr>
<tr class="odd">
<td>ec2: 删除安全组</td>
<td>删除安全组。</td>
</tr>
<tr class="even">
<td>RDS: createdBinstance</td>
<td>创建 RDS 数据库实例。</td>
</tr>
<tr class="odd">
<td>RDS: deletedBinstance</td>
<td>删除 RDS 数据库实例。</td>
</tr>
</tbody>
</table>