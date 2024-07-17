# 事件响应手册：响应 Amazon Q 安全事件

本文档仅供参考。 它代表了截至本文档发布之日亚马逊网络服务 (AWS) 目前提供的产品和做法，如有更改，恕不另行通知。 客户有责任对本文档中的信息以及对 AWS 产品或服务的任何使用进行独立评估，每种产品或服务均按 “原样” 提供，不附带任何形式的明示或暗示担保。 本文档不设定 AWS、其关联公司、供应商或许可方的任何担保、陈述、合同承诺、条件或保证。 AWS 对其客户的责任和责任由 AWS 协议控制，本文档不是 AWS 与其客户之间任何协议的一部分，也不会对其进行修改。

© 2024 亚马逊网络服务公司或其附属公司。 保留所有权利。 本作品采用知识共享署名 4.0 国际许可协议进行许可。

本 AWS 内容的提供受到 http://aws.amazon.com/agreement 上的 AWS 客户协议条款或客户与亚马逊网络服务公司或亚马逊网络服务 EMEA SARL 或两者之间的其他书面协议的约束。

## 联系人

作者：`作者姓名`\
审批人：`审批人姓名`\
最后批准日期：

## 执行摘要
本手册概述了调查涉及 Amazon Q 的安全事件的示例流程和查询。

## 准备
本手册在可能的情况下引用并集成了 [Prowler] (https://github.com/toniblyx/prowler)，后者是一个命令行工具，可帮助您进行 AWS 安全评估、审计、强化和事件响应。

### 目标
在执行剧本的整个过程中，请专注于_***期望的结果***_，记下增强事件响应能力的笔记。

#### 确定：
* **漏洞被利用**
* **观察到的漏洞和工具**
***演员的意图**
* **演员的归因**
***对环境和企业造成的损害**

#### 恢复：
***恢复原始和强化配置**

#### 增强 CAF 安全透视组件：
[AWS 云采用框架 AWS Framework 安全视角] (https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* **指令**
* **侦探**
* **响应式**
* **预防性**

！ [图片] (/images/aws_caf.png)
* *

### 事件分类和处理
* **战术、技巧和程序**：Amazon Q
* **类别**：日志分析
* **资源**：Amazon Q
* **指标**：网络威胁情报、第三方通知
* **日志来源**：CloudTrail
* **团队**：安全运营中心 (SOC)、取证调查员、云工程

## 事件处理流程
### 事件响应过程分为以下几个阶段：
* 准备
* 检测与分析
* 遏制和根除
* 恢复
* 事后活动

### 响应步骤
1。 [**准备工作**]
2。 [**准备工作**]
3。 [**准备工作**]
4。 [**检测和分析**] 针对无法识别的 API 事件执行检测和分析 CloudTrail
5。 [**检测和分析**] 对 CloudWatch 执行检测并分析 CloudWatch 中是否存在无法识别的事件
6。 [**遏制和清除**] 删除或轮换 IAM 用户密钥
7。 [**遏制和根除**] 删除或轮换无法识别的资源

## 准备
* 评估您的安全态势，以识别和修复安全漏洞
* AWS 开发了一种新的开源 Selfersive Security Assement 工具，该工具为客户提供时间点评估，以获得有关其 AWS 账户安全状况的宝贵见解。
* 维护所有资源的完整资产清单，包括服务器、网络设备、网络/文件共享和开发者机器
* 考虑实施 AWS GuardDuty 来持续监控恶意活动和未经授权的行为，从而保护您的 AWS 账户、工作负载和存储在 Amazon SES 中的数据
* 实施 CIS AWS Foundations，包括账户到期和强制证书轮换
* 强制执行多因素身份验证 (MFA)
* 强制执行密码复杂性要求并确定到期时间
* 运行 IAM Credential Report 以列出您账户中的所有用户及其各种证书的状态，包括密码、访问密钥和 MFA 设备
* 使用 AWS IAM Access Analyzer 识别您的组织和账户中的资源，例如与外部实体共享的 IAM 角色。 这使您可以识别对您的资源和数据的意外访问，这是一种安全风险
* 考虑实施 AWS GuardDuty 来持续监控恶意活动和未经授权的行为，从而保护您的 AWS 账户和工作负载。

## 上报程序
-“我需要就何时进行 EC2 取证做出业务决定”
-`谁在监视日志/警报，接收它们并对每个日志/警报采取行动？ `
-`当发现警报时，谁会收到通知？ `
-`公共关系和法律部门何时参与这一进程？ `
-“你什么时候会联系 AWS 支持寻求帮助？ `

## Amazon Q Model

Amazon Q 的多个组件/服务遭到入侵：

* 聊天 — Amazon Q 用英语回答有关 AWS 的自然语言问题，包括有关 AWS 服务选择、AWS 命令行界面 (AWS CLI) 使用、文档和最佳实践的问题。 Amazon Q 以信息摘要或分步说明作为回应，并包含指向其信息源的链接。
* 记忆 — Amazon Q 使用您的对话情境为对话期间的未来回复提供信息。
* 代码改进和建议 — 在 IDE 中，Amazon Q 可以回答有关软件开发的问题，改进您的代码并生成新代码。
* 疑难解答和支持 — Amazon Q 可以帮助您理解 AWS 管理控制台中的错误，并允许访问实时的 AWS 支持代理来解决您的 AWS 问题和问题。
* 客户反馈 — Amazon Q 使用您通过反馈表提交的信息和反馈来提供支持或帮助解决技术问题。

## 检测和分析

### 注意事项和注意事项

* Q 并非在所有地区都可用（2024-05-22）。 有关当前区域的可用性，请参阅 [开发者指南] (https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/regions.html)
* 模型调用日志目前不是 Q 的功能。有计划在将来添加它。 这是区域级别的设置，不是默认设置
* 终端：
* Q 业务（开发人员）：qbusiness.amazonaws.com
* Q Chat：q.amazonaws.com

### 事件名称

下表中列出的事件名称是调查涉及 Q 的安全事件期间在 CloudTrail 中常见的事件类型的快速参考。在表中列出的事件中，在正常使用模型期间记录的最常见的事件名称是：

* `获取对话`
* `开始对话`
* `getIndex`
* `ListRetrievers`
* `getApplications`

| **活动名称** | **描述** |
|:------------------------------------------------------------------
| ***一般权限*** |---|
| `getConversation` | 授予获取与 Amazon Q 的特定对话相关的个人消息的权限 |
| `getTroubleShootingResults` | 授予通过 Amazon Q 获取疑难解答结果的权限 |
| `sendMessage` | 授予向 Amazon 发送消息的权限 Q |
| `startConversation` | 授予与 Amazon 开始对话的权限 Q |
| “startTroubleShootingAnalysis” | StartTroubleShootingAnalysis授予使用 Amazon Q 开始故障排除分析的权限
| `startTroubleShootingResolutingResolutingExplantion` | 授予开始向亚马逊解释疑难解答的权限 Q |
|---|---|
| ***创建应用程序*** |--------|
| `createApplication` | 授予创建应用程序的权限 |
| `deleteApplication` | 授予删除应用程序的权限 |
| `getApplication` | 授予获取应用程序的权限 |
| `ListApplications` | 授予列出应用程序的权限|
| `updateApplication` | 授予更新应用程序的权限 |
|---|---|
| ***创建索引*** |--------|
| `createIndex` | 授予为给定应用程序创建索引的权限 |
| `deleteIndex` | 授予删除索引的权限 |
| `getIndex` | 授予获取索引的权限|
| `listIndices` | 授予列出应用程序索引的权限 |
| `updateIndex` | 授予更新索引的权限 |
|---|---|
| ***创建检索器*** |--------|
| `createRetriever` | 授予为给定应用程序创建检索器的权限 |
| `deletereRetriever` | 授予删除检索器的权限 |
| `getRetriever` | 授予获取寻回犬的权限 |
| `listRetrievers` | 授予列出应用程序检索器的权限 |
| `updateRetriever` | 授予更新寻回犬的权限 |
|---|---|
| ***连接数据源*** |--------|
| `createDataSource` | 授予为给定应用程序和索引创建数据源的权限 |
| `deleteDataSource` | 授予删除数据源的权限 |
| `getDataSource` | 授予获取数据源的权限 |
| `listDataSources` | 授予列出应用程序和索引的数据源的权限 |
| `updateDataSource` | 授予更新数据源的权限 |
| `startDataSourceSyncJobs` | 授予启动数据源同步作业的权限 |
| `stopdataSourceSyncJobs` | 授予停止数据源同步作业的权限 |
| `listDataSourceSyncJobs` | 授予获取数据源同步任务历史记录的权限 |
|---|---|
| ***上传文件*** |--------|
| `batchputDocument` | 授予批量放置文档的权限 |
| `batchDeleteDocument` | 授予批量删除文档的权限 |
|---|---|
| ***聊天和对话管理*** |--------|
| `聊天` | 授予使用应用程序聊天的权限|
| `ChatSync` | 授予使用应用程序同步聊天的权限 |
| `DeleteConversation` | 授予删除对话的权限 |
| `ListConversations` | 授予列出应用程序所有对话的权限 |
| `ListMessages` | 授予列出所有消息的权限 |
|---|---|
| ***用户和群组管理*** |--------|
| `createUser` | 授予创建用户的权限 |
| `deleteUser` | 授予删除用户的权限 |
| `getUser` | 授予获取用户的权限 |
| `updateUser` | 授予更新用户的权限 |
| `putGroup` | 授予放置一组用户的权限 |
| `deleteGroup` | 授予删除群组的权限 |
| `getGroup` | 授予获取群组的权限 |
| `列表组` | 授予列出群组的权限 |
|---|---|
| ***插件*** |----------|
| `createPlugin` | 授予为给定应用程序创建插件的权限 |
| `deletePlugin` | 授予删除插件的权限 |
| `getPlugin` | 授予获取插件的权限 |
| `updatePlugin` | 授予更新插件的权限 |
|---|---|
| ***管理员控件和护栏*** |--------|
| `updateChatControlsConfiguration` | 授予更新应用程序聊天控件配置的权限 |
| `deleteChatControlsConfiguration` | 授予删除应用程序聊天控件配置的权限 |
| `getChatControlsConfiguration` | 授予获取应用程序聊天控件配置的权限 |

### CloudTrail 中的数据平面事件

默认情况下，CloudTrail 不记录数据事件。 下图显示了作为数据事件记录到 CloudTrail 的 Amazon Q API 操作。 数据事件类型（控制台）列显示了 CloudTrail 控制台中的相应选择。 Amazon Q 资源类型列显示您为记录资源的数据事件而指定的 resources.type 值。 更多信息可在 [Q 用户指南] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/logging-using-cloudtrail.html) 中找到。

**亚马逊 Q 企业应用程序**：AWS:: qBusiness:: 应用程序
* 列出 DatasourceSyncJobs
* 启动 DataSourceSyncJob
* 停止数据源同步作业
* 批处理文档
* 批量删除文档
* 推送反馈
* ChatSync
* 删除对话
* 列出对话
* 列出消息
* 列表组
* DeleteGroup
* getGroup
* putGroup
* 创建用户
* 删除用户
* getUser
* updateUser
* 列出文档

**亚马逊 Q 企业数据资源**：AWS:: qBusiness:: dataSource
* 列出 DatasourceSyncJobs
* 启动 DataSourceSyncJob
* 停止数据源同步作业

**亚马逊 Q 商业指数**: AWS:: qBusiness:: Index
* DeleteGroup
* getGroup
* putGroup
* 列表组
* 列出文档
* 批处理文档
* 批量删除文档

### 分析

如果发生事件，除了调查泄露指标、威胁行为者、时间范围等外，在确认这是与 Amazon Q 资源有关的事件后，还有一些其他问题需要考虑：

1。 创建和删除了哪些资源？
2。 使用了哪些插件？
3。 在安全事件发生期间，Q 可以访问哪些应用程序？
4。 Q 访问过这些应用程序吗？
5。 是否有任何类型的文件上传到 Q？
6。 Q 可以访问任何 IDE 环境吗？

## 遏制和根除

* [删除或轮换 IAM 用户密钥] (https://console.aws.amazon.com/iam/home#users) 和 [Root 用户密钥] (https://console.aws.amazon.com/iam/home#security_credential)；如果您无法识别已公开的特定密钥，则可能希望轮换账户中的所有密钥
* [删除未经授权的 IAM 用户] (https://console.aws.amazon.com/iam/home#users。)
* [删除未经授权的政策] (https://console.aws.amazon.com/iam/home#/policies)
* [删除未经授权的角色] (https://console.aws.amazon.com/iam/home#/roles)
* [撤销临时证书] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time)。 也可以通过删除 IAM 用户来撤销临时证书。
* 注意：删除 IAM 用户可能会影响生产工作负载，因此应谨慎行事
* [轮换 SMTP 凭证] (https://aws.amazon.com/premiumsupport/knowledge-center/ses-create-smtp-credentials/)
* [如何删除 Amazon Q 应用程序] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-app-actions.html)
* [如何删除 Amazon Q Retriever] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-native-retriever-actions.html)
* [如何删除 Amazon Q Kenra Retriever] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-kendra-retriever-actions.html)
* [如何删除上传的文档] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/delete-doc-upload.html)
* [如何删除 Amazon Q 数据源] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-datasource-actions.html)
* [如何删除亚马逊网络体验] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-exp-actions.html)

## 恢复

* 使用最低权限访问策略创建新 IAM 用户


## 数据保护和 IAM 政策

### 数据保护：

* 切勿将机密或敏感信息（例如客户的电子邮件地址）放入标签或自由格式的文本字段（例如 “姓名” 字段）中。 这包括您使用控制台、API、AWS CLI 或 AWS 软件开发工具包使用 Amazon Q 或其他 AWS 服务时。 您在标签或用于名称的自由格式文本字段中输入的任何数据都可用于计费或诊断日志。
* Amazon Q 将您的问题、答案和其他上下文（例如控制台元数据和代码）存储在您的 IDE 中，以生成对问题的回复。
* Amazon Q，数据被发送到并存储在美国地区。 与 Amazon Q 对话期间收集的数据存储在美国东部（弗吉尼亚北部）区域。 故障排除控制台错误会话期间收集的数据存储在美国西部（俄勒冈）区域。

### IAM 权限：

* “AmazonqFullAccess” 托管策略提供完全访问权限，允许与 Amazon Q 进行互动。默认情况下，所有管理员和高级用户都有访问权限，但对于其他用户和角色，客户需要附加 AWS Q 托管策略。 可以基于 Amazon Q 操作来限制权限。
* 要在 AWS 控制台中提问 Amazon Q 问题，IAM 身份需要权限才能执行以下操作：
* 开始对话 [示例：“Q: 开始对话”]
* 发送消息

* 要解决 Amazon Q 的控制台错误，IAM 身份需要权限才能执行以下操作：
* 开始疑难解答分析
* 获取疑难解答结果
* 开始疑难解答分析

* 如果附加的策略未明确允许其中一项操作，则在您尝试使用 Amazon Q 时会返回 IAM 权限错误。


## 吸取的教训
“这里是添加贵公司特有的项目的地方，这些项目不一定需要 “修复”，但在执行本手册时要根据运营和业务要求了解这些内容。 `

## 已解决的待办事项列表

## 当前待办事项列表