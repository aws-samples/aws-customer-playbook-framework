# 响应 Amazon Bedrock 安全事件的安全手册
本文档仅供参考。 它代表了截至本文档发布之日亚马逊网络服务 (AWS) 目前提供的产品和做法，如有更改，恕不另行通知。 客户有责任对本文档中的信息以及对 AWS 产品或服务的任何使用进行独立评估，每种产品或服务均按 “原样” 提供，不提供任何形式的明示或暗示担保。 本文档不设定 AWS、其关联公司、供应商或许可方的任何担保、陈述、合同承诺、条件或保证。 AWS 对其客户的责任和责任由 AWS 协议控制，本文档不是 AWS 与其客户之间任何协议的一部分，也不会对其进行修改。

© 2024 亚马逊网络服务公司或其附属公司。 保留所有权利。 本作品采用知识共享署名 4.0 国际许可协议进行许可。

本 AWS 内容的提供受到 http://aws.amazon.com/agreement 上的 AWS 客户协议条款或客户与亚马逊网络服务公司或亚马逊网络服务 EMEA SARL 或两者之间的其他书面协议的约束。

## 联系人

作者：`作者姓名`\
审批人：`审批人姓名`\
最后批准日期：

## 执行摘要

作为我们对客户的持续承诺的一部分，AWS 正在提供这个
安全事件响应手册，描述了所需的步骤
调查 Amazon Bedrock 成为您的 AWS 账户内未经授权使用的来源或目标的安全事件。 本文档的目的是就您怀疑发生安全事件后应采取的措施提供规范性指导。

！ [图片] (/images/nist_life_cycle.png)

*AWS 事件响应的各个方面*

## 准备

为了快速有效地执行事件响应活动，让组织中的人员、流程和技术做好准备至关重要。 查看 [*AWS 安全事件响应指南*] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/preparation.html)，并实施必要的步骤来帮助确保所有三个域做好准备。

## 如何联系 AWS 支持部门寻求帮助

如果您怀疑自己的 AWS 账户或组织中的凭证遭到泄露，请务必立即通知 AWS。 以下是与 AWS Support 合作的步骤：

### 提交 AWS 支持案例

-登录您的 AWS 账户：
-这是第一个受到安全事件影响的用于验证 AWS 账户所有权的 AWS 账户。
-注意：如果您无法访问账户，请使用 [此表格] (https://support.aws.amazon.com/#/contacts/aws-account-support/) 提交支持请求。
-选择 “*服务*”、“*支持中心*”、“*创建案例*”。
-选择问题类型 “*账户和账单” *。
-选择受影响的服务和类别：
-示例：
-服务：账户
-类别：安全
-选择严重性：
-企业支持或入口客户：*关键业务风险问题*。
-业务支持客户：*紧急业务风险问题*。
-描述您的问题或问题：
-详细描述您遇到的安全问题、受影响的资源以及业务影响。
-首次识别安全事件的时间。
-迄今为止的事件时间表摘要。
-您收集的工件（屏幕截图、日志文件）。
-您怀疑遭到泄露的 IAM 用户或角色的 ARN。
-受影响资源和业务影响的描述。
-贵组织中的参与程度（例如：“此安全事件具有首席执行官和董事会的知名度”）。
-可选：添加其他问题联系人。
-选择 “*联系我们*”。
-首选联系语言（可能视供应情况而定）
-首选联系方式：网络、电话或聊天（推荐）
-可选：其他案例联系人
-*点击 “提交” *

-**升级**：请尽快通知您的 AWS 账户团队，以便他们动用必要的资源并根据需要进行上报。

**注意：** 请务必确认为每个 AWS 账户定义了 “备用安全联系人”。 欲了解更多详情，请参阅 [this
文章] (https://aws.amazon.com/blogs/security/update-the-alternate-security-contact-across-your-aws-accounts-for-timely-security-notifications/)。


## Amazon Bedrock

Amazon Bedrock 受到多个组件/服务的威胁：

* 模型-可以是领先的 AI 初创公司的基础模型，也可以是定制模型
* 推理-根据提供给模型的输入生成输出的过程
* 知识库-允许您将数据源积累到信息存储库中
* 代理-帮助您的最终用户根据组织数据和用户输入完成操作

当 AWS 账户（授权或未授权）中的用户访问 Bedrock 时，他们最终会通过模型、知识库或代理进行访问。 发出的任何提示及其响应都将在模型调用日志中发布，前提是该日志是之前设置的 (https://docs.aws.amazon.com/bedrock/latest/userguide/model-invocation-logging.html)。 欲了解更多信息，请在此处查看 Amazon Bedrock 的用户指南：https://docs.aws.amazon.com/bedrock/

下图描绘了提示、[代理] (https://docs.aws.amazon.com/bedrock/latest/userguide/agents-how.html) 和知识库之间的关系，可能是可视化数据流的好方法（摘自 [用户指南] (https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html)）

！ [构建时 API 操作期间的代理构建] (/images/bedrock-01.png)


## 检测

### 注意事项和注意事项

* 基岩并非在所有地区都可用（2024-06-04）。 有关当前地区的可用性，请参阅 [链接] (https://docs.aws.amazon.com/bedrock/latest/userguide/bedrock-regions.html)
* 需要模型调用日志才能查看发送给模型的提示和响应。 这是区域级别的设置，不是默认设置
* 对于 CloudTrail，EventSource = bedrock.amazonaws.com
* ***invokeModel*** API 是一个**只读**事件

### 事件名称

下表中列出的事件名称是调查涉及 Bedrock 的安全事件期间通常在 CloudTrail 中发现的事件类型的快速参考。 在表中列出的事件名称中，在正常使用模型期间记录的最常见的事件名称是：

* `获取基础模型可用性`
* `invokeModel`
* `使用响应流调用模型`

| **活动名称** | **只读** | **描述** |
|:----------------------------------------------------------------------------------
| ***与模特相关的事件*** |---|---|
| `getFoundationModelaVailability` | TRUE | 检索基础模型的可用性 |
| `getuseCaseForModelAccess` | TRUE | 检索指定模型的用例 |
| `invokeModel` | TRUE | 使用请求正文中提供的输入调用指定的 Bedrock 模型来运行推理 |
| `invokeModelWithResponseStream` | TRUE | 使用请求正文中提供的输入使用分块调用指定的 Bedrock 模型来运行推理 |
| `listCustomModels` | TRUE | 列出已创建的 Bedrock 自定义模型 |
| `listFoundationModels` | TRUE | 列出可以使用的基础模型 |
| `listProvisionedModelThroughputs` | TRUE | 列出模型的预配置容量 |
|---|---|---|
| ***与模型调用日志相关的事件*** |---|---|
| `deleteModelinvocationLoggingConfiguration` | FALSE | 删除现有的调用日志配置 |
| `getModelinvocationLoggingConfiguration` | TRUE | 检索现有的调用日志配置 |
| `putModelinvocationLoggingConfiguration` | FALSE | 创建现有的调用日志配置 |
|---|---|---|
| ***与知识库相关的事件*** |---|---|
| `AssociateAgentKnowledgebase` | FALSE | 此操作记录代理创建期间或创建代理之后知识库的关联 |
| `createDataSource` | FALSE | 创建数据源 |
| `CreateKnowledgebase` | FALSE | 创建知识库 |
| `deleteKnowledgebase` | FALSE | 删除知识库 |
| `getDataSource` | TRUE | 获取有关数据源的信息 |
| `getKnowledgebase` | TRUE | 检索现有的知识库 |
| `ListDataSources` | TRUE | 列出可用的数据源 |
| `ListKnowledgebases` | TRUE | 列出现有知识库 |
|---|---|---|
| ***与代理相关的事件*** |---|---|
| `createAgent` | FALSE | 创建一个新的代理和一个指向草稿代理版本的测试代理别名 |
| `createAgentactionGroup` | FALSE | 为代理创建操作组。 操作组表示代理可以通过定义代理可以调用的 API 和调用它们的逻辑来为客户执行的操作 |
| `createAgentalias` | FALSE | 为代理创建新别名 |
| `deleteAgent` | FALSE | 删除之前创建的代理 |
| `getAgent` | TRUE | 检索现有代理 |
| `getAgentalias` | TRUE | 检索现有的别名 |
| `invokeAgent` | FALSE | 发送提示代理处理和回复 |
| `listagentActionGroups` | TRUE | 列出代理的操作组以及有关每个操作组的信息 |
| `listagentaliases` | TRUE | 列出代理的别名以及每个代理的相关信息 |
| `listagentKnowledgebases` | TRUE | 列出与代理相关的知识库以及有关每个代理的信息 |
| `ListAgents` | TRUE | 列出现有代理 |
| `listagentVersions` | TRUE | 列出代理的版本以及有关每个版本的信息 |
| `PrepareAgent` | FALSE | 准备现有的 Amazon Bedrock Agent 以接收运行时请求 |
|---|---|---|
| ***与招聘相关的事件*** |---|---|
| `getingestionJob` | TRUE | 获取有关摄取作业的信息，其中将数据源添加到知识库中 |
| `listingestionJobs` | TRUE | 列出数据源的摄取任务以及每个采集任务的相关信息 |
| `startingestionJob` | FALSE | 开始摄取作业，将数据源添加到知识库中 |
|---|---|---|
| ***与 RAG 相关的事件*** |---|---|
| `Retrieve` | TRUE | 从知识库中检索提取的数据（被视为数据事件，不会显示在 CloudTrail 中）|
| `RetrieveAndGenerate` | FALSE | 发送用户输入以执行检索和生成（被视为数据事件，不会显示在 CloudTrail 中）|

### 基岩使用截图

下面的屏幕截图可以为事件响应者提供视觉帮助，帮助他们解释调查期间发现的事件。 下面的每张图片都表示所采取的与记录的事件名称相匹配的操作

---

**获取基础模型可用性**
<details>
<summary>展开屏幕截图</summary>
-
此事件是在浏览到 Amazon Bedrock 主页面时首次记录的

！ [getFoundationModelaVailability] (/images/bedrock-02.png)

</details>

---

**列出基础模型**和**列出自定义模型**
<details>
<summary>展开屏幕截图</summary>
-
浏览到 “基础模型” 和 “自定义模型” 页面时会记录这些事件

！ [列表基础模型和列表自定义模型] (/images/bedrock-03.png)

</details>

---

**InvokeModel**
<details>
<summary>展开屏幕截图</summary>
-
调用模型时会记录此事件。

！ [模型调用示例] (/images/bedrock-04.png)

模型调用的 CloudTrail 事件记录示例：

！ [适用于 InvokeModel 的 CloudTrail] (/images/bedrock-05.png)

</details>

---

**invokeModel**-其他示例
<details>
<summary>展开屏幕截图</summary>
-
下图提供了有关如何记录 `invokeModel` 事件名称的其他示例。
第一张图片显示了各种模型调用方法之间的区别：
(a) 使用知识库调用模型
(b) 直接调用模型
(c) 使用代理调用模型

请注意，显示的*用户名*会有所不同，具体取决于模型的调用方式

！ [代理和知识库模型调用] (/images/bedrock-06.png)

使用 CloudTrail 中 “InvokeModel” 事件记录的两张并排屏幕截图突出显示了这种差异
左图 = 直接调用模型
右图 = 使用知识库调用模型

！ [适用于 InvokeModel 的 CloudTrail] (/images/bedrock-07.png)


</details>

---

**invokeModel**或**invokeModelwithResponseStream**-提示和响应
<details>
<summary>展开屏幕截图</summary>
-
当 “invokeModel” 和 “invokeModelWithResponseStream” 事件显示在 CloudTrail 中时，模型调用日志（配置后）还会显示与每个 “invokeModel” 事件相对应的提示和响应。 下图取自已发送到 S3 的模型调用日志，其中包含使用的提示以及模型的输出或响应（下方有 Athena DDL）：

！ [S3 模型调用示例] (/images/bedrock-09.png)

</details>

---

**putModelIncationLogging配置**
<details>
<summary>展开屏幕截图</summary>
-
在 AWS 账户中配置模型调用日志时，会记录此事件

！ [配置模型调用日志] (/images/bedrock-10.png)

</details>

---

**创建知识库**
<details>
<summary>展开屏幕截图</summary>
-
创建知识库时会记录以下 CloudTrail 事件记录。 事件记录包括发出请求的身份和知识库的名称。 可以在使用 `invokeModel` 捕获模型调用的后续日志中引用知识库的名称

！ [创建知识库] (/images/bedrock-11.png)

</details>

---

**CreateAgent**
<details>
<summary>展开屏幕截图</summary>
-
下图描绘了使用控制台创建代理的过程。 记录的结果事件名称是 `createAgent`

！ [创建代理] (/images/bedrock-12.png)

</details>

---

**检索**和**检索并生成**
<details>
<summary>展开屏幕截图</summary>
-
测试知识库时，下图中显示了 “检索” 和 “检索并生成” 事件名称的示例。 请注意，“检索” 仅测试从知识库中检索数据，而 “RetrieveAndGenerate” 测试从知识库中检索数据并生成响应：

！ [检索和检索并生成] (/images/bedrock-13.png)

</details>

---

### Athena DDL

使用下面的 DDL 代码，根据存储在 Amazon S3 存储桶中的模型调用日志在 Athena 中创建表。 确保使用正确的 S3 URI 作为代码片段末尾的存储段位置：

```
创建外部表 bedrock_model_invocation_metadata_unfilter (
架构类型字符串，
架构版本字符串，
时间戳字符串，
账户 ID 字符串，
<arn: string>身份结构，
区域字符串，
requestId 字符串，
操作字符串，
modelID 字符串，
错误代码字符串，
输入结构 <inputBodyJSON：字符串，
inputBodys3Path：字符串，
InputContentType：字符串，
inputTokenCount：int>，
输出结构 <outputBodyJSON：字符串，
outputBodys3Path：字符串，
outputContentType：字符串，
outputTokenCount：int，
outputImageBucketedStepsize：int，
outputImageHeight：int，
outputImageWidth：int
>
)
行格式 SERDE 'org.openx.data.jsonserde.jsonSerde'
<insert_prefix>位置 's3://<insert_S3_bucket>//'
```

## 已解决的待办事项列表
-作为事件响应者，我需要能够监控所有关键的 Bedrock 事件
-作为一名事件响应者，我需要一本关于大规模查询 Bedrock Cloudtrail 事件的剧本

## 当前待办事项列表