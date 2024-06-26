# 响应 Amazon Bedrock 安全事件的安全手册
本文档仅供参考。 它代表了截至本文档发布之日亚马逊网络服务 (AWS) 目前提供的产品和做法，如有更改，恕不另行通知。 客户有责任对本文档中的信息以及对 AWS 产品或服务的任何使用进行独立评估，每种产品或服务均按 “原样” 提供，不提供任何形式的明示或暗示担保。 本文档不设定 AWS、其关联公司、供应商或许可方的任何担保、陈述、合同承诺、条件或保证。 AWS 对其客户的责任和责任由 AWS 协议控制，本文档不是 AWS 与其客户之间任何协议的一部分，也不会对其进行修改。

© 2024 亚马逊网络服务公司或其附属公司。 版权所有。 本作品采用知识共享署名 4.0 国际许可协议进行许可。

本 AWS 内容的提供受到 http://aws.amazon.com/agreement 上的 AWS 客户协议条款或客户与亚马逊网络服务公司或亚马逊网络服务 EMEA SARL 或两者之间的其他书面协议的约束。

## 联系人

作者：`作者姓名`\
审批人：`审批人姓名`\
最后批准日期：

## 执行摘要

作为我们对客户的持续承诺的一部分，AWS 正在提供这个
安全事件响应手册，描述了向 AWS Support 请求安全事件帮助所需的步骤。

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
-基本和开发者支持客户：*一般问题*。
-要了解更多信息，您可以 [比较 AWS 支持计划] (https://aws.amazon.com/premiumsupport/plans/)。
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