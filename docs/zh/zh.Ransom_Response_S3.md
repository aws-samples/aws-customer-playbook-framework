# 事件响应行动手册：S3 的赎金响应
本文档仅供参考。 它代表了截至本文档发布之日亚马逊 Web Services (AWS) 提供的当前产品和实践，这些产品和做法如有更改，恕不另行通知。 客户有责任对本文档中的信息以及对 AWS 产品或服务的任何使用情况进行自己的独立评估，每种产品或服务都是 “按原样” 提供的，无论是明示还是暗示的担保。 本文档不创建 AWS、其附属公司、供应商或许可方的任何担保、陈述、合同承诺、条件或保证。 AWS 对客户的责任和责任受 AWS 协议的控制，本文档既不是 AWS 与其客户之间的任何协议的一部分，也不修改。

© 2021 亚马逊网络服务公司或其附属公司。 保留所有权利。 本作品根据知识共享署名 4.0 国际许可协议进行许可。

提供此 AWS 内容须遵守 http://aws.amazon.com/agreement 提供的 AWS 客户协议的条款或客户与亚马逊 Web Services, Inc. 或亚马逊网络服务 EMEA SARL 或两者之间的其他书面协议。

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
[AWS 云采用框架安全角度] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* ** 指令 **
* ** 侦探 **
* ** 响应 **
* ** 预防性 **

! [图片] (/images/aws_caf.png)
* * *

### 响应步骤
1. [** 准备 **] 使用 AWS Config 查看配置合规性
2. [** 准备 **] 识别、记录和测试上报程序
3. [** 检测和分析 **] 执行检测和分析 CloudTrail 以查找无法识别的 API
4. [** RECOVERY**] 酌情执行恢复程序

*** 响应步骤遵循 [NIST 特别出版物 800-61r2 计算机安全事件处理指南] 中的事件响应生命周期（https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf）

! [图片] (/images/nist_life_cycle.png) ***

### 事件分类和处理
* ** 策略、技术和程序 **：赎金和数据销毁
* ** 类别 **：赎金攻击
* ** 资源 **：S3
* ** 指标 **：网络威胁情报、第三方通知、Cloudwatch 指标
* ** 日志源 **：S3 服务器日志、S3 访问日志、CloudTrail、CloudWatch、AWS Config
* ** 团队 **：安全运营中心 (SOC)、法医调查员、云工程

## 事件处理流程
### 事件响应流程有以下几个阶段：
* 准备
* 检测和分析
* 遏制和根除
* 恢复
* 事件后活动

## 执行摘要
本行动手册概述了针对 AWS 简单存储服务 (S3) 的赎金攻击的响应流程。

有关更多信息，请查看 [AWS 安全事件响应指南] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

## 准备
* 评估你的安全态势以识别和补救安全漏洞
* AWS 开发了一种新的开源 [自助安全评估] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) 工具，该工具为客户提供了时间点评估，以获得对其 AWS 安全状况的宝贵见解账户。
* 维护所有资源的完整资产清单，包括服务器、网络设备、网络/文件共享和开发人员机器
* 考虑使用 [AWS Config] (https://aws.amazon.com/config/)，这是一项使您能够评估、审计和评估 AWS 资源配置的服务
* 考虑实施 [AWS GuardDuty] (https://aws.amazon.com/guardduty/) 以持续监控恶意活动和未经授权的行为，以保护存储在 Amazon S3 中的 AWS 账户、工作负载和数据
* Darkbit 提供了一个 [AWS S3 数据丢失防护] (https://darkbit.io/blog/simple-dlp-for-amazon-s3) 的示例，可用于识别未经授权的对象复制。 还可以根据您的业务使用案例在模式中添加其他特定操作
* 使用 IAM 角色管理权限
* 实现最低权限且不允许 s3：* 权限
* 实施 [CIS AWS 基金会] (https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html)，包括账户到期和强制性证书轮换
* 强制执行多因素身份验证 (MFA)
* 强制执行密码复杂性要求并确定到期期
* 运行 [IAM 凭证报告] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) 以列出账户中的所有用户及其各种证书的状态，包括密码、访问密钥和 MFA 设备
* 使用 [AWS IAM 访问分析器] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) 识别组织和账户中的资源，例如与外部实体共享的 Amazon S3 存储桶或 IAM 角色。 这使您可以识别对资源和数据的意外访问权限，这是一种安全风险
* [启用 S3 对象版本控制] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) 以允许恢复修改过的对象
* 要设置保留的版本数，请设置生命周期策略 (http://docs.aws.amazon.com/AmazonS3/latest/dev/intro-lifecycle-rules.html) 以应用于非当前版本
* [启用 S3 MFA 删除] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/MultiFactorAuthenticationDelete.html) 以防止攻击者禁用版本控制和覆盖存储桶中的所有对象
* 考虑使用 [S3 对象锁定] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html)，以便您可以使用一次写入多读 (WORM) 模型存储对象。 对象锁定可以帮助防止对象在固定时间内或无限期地被删除或覆盖。
* 在对象锁定合规性模式 * 中，* 受保护的对象版本不能被任何用户覆盖或删除，包括 AWS 账户中的根用户。 当对象处于合规性模式下锁定时，其保留模式不能更改，也不能缩短其保留期。 合规模式 * 确保在保留期内不能覆盖或删除对象版本。
* 考虑使用 [S3 智能分层] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) 进行对象备份和成本优化
* 使用并定期审核存储桶策略
* 只公开需要的东西，并确保所有对象都通过私有的方式受到保护
* 考虑使用 [AWS 密钥管理服务 (KMS)] (https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) 密钥加密所有对象并防止攻击者应用其加密密钥
* 考虑使用 [AWS S3 阻止公共访问功能] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) 来减少对象的意外暴露
* 启用包含敏感或关键信息的 [S3 存储桶和对象的 CloudTrail 事件日志记录] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html)。 默认情况下，CloudTrail 跟踪不记录数据事件，但是您可以配置跟踪记录指定的 S3 存储桶的数据事件，或者为 AWS 账户中的所有 Amazon S3 存储桶记录数据事件。
* 启用 [S3 存储桶的 CloudTrail 服务器级别日志记录] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerLogs.html) 和包含敏感或关键信息的对象。 服务器访问日志记录提供对存储桶发出的请求的详细记录
* 对于关键任务数据上传，请考虑 [启用复制] (http://docs.aws.amazon.com/AmazonS3/latest/dev/crr.html)-同一区域或跨区域。 跨区域复制通过在另一个区域维护第二个副本，从而防止应用程序缺陷和操作员错误
* 采取措施 [防止任何新凭据公开]（http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html）
* 虽然我们不能推荐特定的第三方解决方案，但也有我们的合作伙伴（包括 Veritas 和 CommVault）以及其他备份软件提供商的产品，它们具有将 S3 中的对象备份到 S3 内外的托管备份存储位置备份的本机能力

### 使用 AWS Config 查看配置合规性：
1. 登录 AWS 管理控制台并在 https://console.aws.amazon.com/config/ 打开 AWS Config 控制台
1. 在 AWS 管理控制台菜单中，验证区域选择器是否设置为支持 AWS Config 规则的区域。 有关受支持区域的列表，请参阅 Amazon Web 服务通用参考中的 AWS Config 区域和终端节点
1. 在导航窗格中，选择资源。 在资源库存页面上，您可以按资源类别、资源类型和合规性状态进行筛选。 如果适当，选择包括已删除的资 该表显示了资源类型的资源标识符以及该资源的资源合规性状态。 资源标识符可能是资源 ID 或资源名称
1. 从资源标识符列中选择资源
1. 选择资源时间轴按钮。 您可以按配置事件、合规性事件或 CloudTrail 事件进行筛选
1. 特别关注以下活动：
* s3 账户级别的公共访问阻止
* s3 账户级别的公共访问区块-定期
* s3 存储桶黑名单-禁止操作
* s3 存储桶默认锁定启用
* s3 存储桶级别的公共访问禁止
* 启用 s3 存储桶日志记录
* s3 存储桶-政策-格兰特-检查
* s3 存储桶-政策-不是更宽松
* s3 存储桶-公共阅读禁止
* s3 存储桶-公共写入禁止
* 启用 s3 存储桶复制
* 启用 s3 存储桶服务器端加密
* 仅限 s3 存储桶-SSL 请求
* 启用 s3 存储桶版本控制
* s3 默认加密-kms

## 上报程序
-“我需要就什么时候进行 EC2 取证作出业务决定 `
-“谁在监控日志/警报，接收日志/警报并对其采取行动？ `
-“发现警报后谁会收到通知？ `
-“什么时候公共关系和法律参与这一过程？ `
-“你什么时候联系 AWS Support 寻求帮助？ `

## 检测和分析
* S3 对象被删除或删除整个 S3 存储桶
* 注意：对于数据销毁事件，可能会提供赎金通知书，也可能不会提供。 另外，请确保您检查 CloudWatch 指标和 CloudTrail S3 事件，以验证是否发生了数据泄露，是否在赎金或数据销毁攻击之间划分
* S3 对象使用不属于客户所有的账户中的密钥进行加密
* 赎金票据可以作为存储桶内的对象或通过电子邮件提供给客户
* 检查 CloudTrail 日志中是否存在未经批准的活动，例如创建未经授权的 IAM 用户、策略、角色或临时安全证书
* 查看 CloudTrail 是否有无法识别的 API 调用。 具体来说，查找以下事件：
* 删除存储桶
* 删除 BucketCORS
* 删除存储桶加密
* 删除存储桶生命周期
* 删除存储桶策略
* 删除存储桶复制
* 删除 Buckett标记
* 删除存储桶公共访问阻止
* 如果启用了 S3 服务器访问日志，则从同一远程 IP 和请求者查找高顺序的 `REST.COPY.OBJECT_GET`。
* 查看您的 CloudTrail 日志，查看您的 AWS 账户是否有任何未经授权的 AWS 使用情况，例如未经授权的 EC2 实例、Lambda 函数或 EC2 竞价出价。 您还可以通过登录 AWS 管理控制台并查看每个服务页面来检查使用情况。 也可以检查账单控制台中的 “账单” 页面是否意外使用
* 请记住，任何地区都可能发生未经授权的使用情况，并且您的主机一次只能向您显示一个区域。 要在区域之间切换，您可以使用控制台屏幕右上角的下拉菜单

## 恢复
* 建议不要支付赎金
* 支付赎金是一场赌博，罪犯在收到付款后是否会兑现交易
* 如果不存在数据备份，那么您应该进行成本效益分析，并权衡数据/声誉损失与向攻击者付款的价值
* 如果你选择支付赎金，你可以直接让攻击者继续对你的公司或其他人进行操作
* 访问 https://www.nomoreransom.org/ 以确定是否有解密器可用于恶意软件变体（受感染的数据）
* [删除或轮换 IAM 用户密钥] (https://console.aws.amazon.com/iam/home#users) 和 [root 用户密钥] (https://console.aws.amazon.com/iam/home#security_credential)；如果您无法识别已公开的特定密钥或密钥，则可能希望轮换账户中的所有密钥
* [删除未经授权的 IAM 用户] (https://console.aws.amazon.com/iam/home#users。)
* [删除未经授权的政策] (https://console.aws.amazon.com/iam/home#/policies)
* [删除未经授权的角色] (https://console.aws.amazon.com/iam/home#/roles)
* [吊销临时证书] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time)。 也可以通过删除 IAM 用户来吊销临时证书。 注意：删除 IAM 用户可能会影响生产工作负载，应谨慎完成 * 在勒索软件攻击或数据损坏之前，使用 CloudEnDure 灾难恢复选择最新的恢复点，以恢复 AWS 上的工作负载
* 如果使用备用数据备份策略，请验证备份没有被感染并从勒索软件事件发生之前的最后一个计划事件中恢复
* 在尝试恢复数据或对象之前，执行 Protect 下的过程
* [删除删除标记] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/RemDelMarker.html) 用于版本控制对象
* 重新创建删除的存储桶
* [使用 S3 智能分层还原对象] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) 对象备份或复制区域存储桶
* 注意：S3 目前没有 “取消删除” 功能，并且 AWS 无法恢复已删除的数据。 在当前数据存储合规性和法规的时代，例如 GDPR (https://gdpr-info.eu/) 和 CCPA (https://oag.ca.gov/privacy/ccpa)，Amazon S3 无法继续存储已从客户账户中明确删除的客户数据。 删除对象后，无论向 AWS 报告意外删除的速度如何，AWS 都无法再恢复对象

## 吸取的教训
“这里是一个添加特定于贵公司的物品的地方，这些物品不一定需要 “修复”，但在与运营和业务要求同时执行本行动手册时也很重要。 `

## 已解决积压项目
-作为事件响应者，我需要一本运行手册来进行 EC2 取证
-作为事件响应者，我需要就应该何时进行 EC2 取证作出业务决策
-作为事件响应者，我需要在所有已启用的区域启用日志记录，无论使用意图如何
-作为事件响应者，我需要能够检测我现有 EC2 实例上的加密挖矿

## 当前积压项目