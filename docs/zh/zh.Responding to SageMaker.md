# 事件响应手册：响应 SageMaker 安全事件
本文档仅供参考。 它代表了截至本文档发布之日亚马逊网络服务 (AWS) 目前提供的产品和做法，如有更改，恕不另行通知。 客户有责任对本文档中的信息以及对 AWS 产品或服务的任何使用进行独立评估，每种产品或服务均按 “原样” 提供，不提供任何形式的明示或暗示担保。 本文档不设定 AWS、其关联公司、供应商或许可方的任何担保、陈述、合同承诺、条件或保证。 AWS 对其客户的责任和责任由 AWS 协议控制，本文档不是 AWS 与其客户之间任何协议的一部分，也不会对其进行修改。

© 2024 亚马逊网络服务公司或其附属公司。 保留所有权利。 本作品采用知识共享署名 4.0 国际许可协议进行许可。

本 AWS 内容的提供受到 http://aws.amazon.com/agreement 上的 AWS 客户协议条款或客户与亚马逊网络服务公司或亚马逊网络服务 EMEA SARL 或两者之间的其他书面协议的约束。


## 联系人

作者：`作者姓名`\
审批人：`审批人姓名`\
最后批准日期：

## 执行摘要
作为我们对客户的持续承诺的一部分，AWS 提供了这本安全事件响应手册，其中描述了在 Amazon SageMaker 成为您的 AWS 账户中未经授权使用的来源或目标的安全事件进行调查所需的步骤。 本文档的目的是就您怀疑发生安全事件后应采取的措施提供规范性指导。

！ [图片] (sagemaker_images/nist_life_cycle.png)

*AWS 事件响应的各个方面*


### 亚马逊 SageMaker

Amazon SageMaker 是一项完全托管的机器学习 (ML) 服务。 借助 SageMaker，数据科学家和开发人员可以快速、自信地构建、训练机器学习模型，并将其部署到可用于生产的托管环境中。 它为运行机器学习工作流程提供了用户界面体验，使 SageMaker 机器学习工具可在多个集成开发环境 (IDE) 中使用。

借助 SageMaker，您无需构建和管理自己的服务器即可存储和共享数据。 凭借对自带算法和框架的内置支持，SageMaker 提供了灵活的分布式训练选项，可根据特定工作流程进行调整。 欲了解更多信息，请在此处查看亚马逊 SageMaker 的开发者指南：https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html



## 准备
通过实施预防控制（[服务控制策略] (https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)）和侦测控制（有关配置规则，请参阅 “检测” 部分），主动做好环境准备。


**预防性 (SCP) **

VPC
* 例如，除非指定了 VPC，否则以下 SCP 将阻止用户启动任何笔记本、培训或处理作业。

```
{
“版本”：“2012-10-17”，
“声明”：[
{
“Sid”：“vpc 部署”，
“效果”：“拒绝”，
“动作”：[
“SageMaker：创建超参数调整作业”，
“SageMaker：CreateModel”，
“SageMaker：创建笔记本实例”，
“SageMaker：创建处理任务”，
“SageMaker：CreateTrainingJob”
]，
“资源”：[
“*”
]，
“状况”：{
“空”：{
“SageMaker: vpcSecurityGroupID”：“没错”，
“SageMaker: vpcSubnets”：“没错”
}
}
}
]
}
```


* 强制执行任务加密

```
{
“版本”：“2012-10-17”，
“声明”：[
{
“Sid”：“denyunEncryptedVolumes”，
“效果”：“拒绝”，
“动作”：[
“SageMaker：创建超参数调整作业”，
“SageMaker：CreateTrainingJob”，
“SageMaker: createEndpointConfig”
“SageMaker：CreateTransformJo
]，
“资源”：[
“*”
]，
“状况”：{
“空”：{
“SageMaker: volumekmsKey”: [
“真的”
]
}
}
}
]
}
```
* 强制执行容器间流量加密
```
{
“版本”：“2012-10-17”，
“声明”：[
{
“Sid”：“denyunEncryptedTraffic”，
“效果”：“拒绝”，
“动作”：[
“SageMaker：CreateTrainingJob”，
“SageMaker：创建超参数调优作业”
]，
“资源”：[
“*”
]，
“状况”：{
“Bool”：{
“SageMaker：容器间流量加密”：“false”
}
}
}
]
}
```

* 强制网络隔离
```
{
“版本”：“2012-10-17”，
“声明”：[
{
“Sid”：“denynotiSolated”，
“效果”：“拒绝”，
“动作”：[
“SageMaker：CreateTrainingJob”，
“SageMaker：创建超参数调整作业”，
“SageMaker：CreateModel”
]，
“资源”：“*”，
“状况”：{
“Bool”：{
“SageMaker: NetworkIsolation”：“false”
}
}
}
]
}
```
* 将笔记本预签名 URL 限制为 IP
```
{
“版本”：“2012-10-17”，
“声明”：[
{
“Sid”：“restricturlToIP”，
“效果”：“拒绝”，
“Action”：“SageMaker：createpresignedNotebookInstanceURL”，
“资源”：“*”，
“状况”：{
“对于所有值：notipAddress”：{
“aws: sourceIP”: [
“[输入_PUBLIC_IP_ADDRESS]”
]
}
}
}
]
}
```
* 禁用互联网接入
```
{
“版本”：“2012-10-17”，
“声明”：[
{
“Sid”：“denyDirectInternet”，
“效果”：“拒绝”，
“动作”：“SageMaker：CreateNotebookInstance”，
“资源”：“*”，
“状况”：{
“StringEquals”: {
“SageMaker：直接互联网接入”：[
“已启用”
]
}
}
}
]
}
```

* 在 SageMaker 笔记本中禁用 Root 访问权限
```
{
“版本”：“2012-10-17”，
“声明”：[
{
“Sid”：“SageMakerdenyRootAccess”，
“效果”：“拒绝”，
“动作”：[
“SageMaker：创建笔记本实例”，
“SageMaker：UpdateNotebookInstance”
]，
“资源”：“*”，
“状况”：{
“StringEquals”: {
“SageMaker: rootAccess”: [
“已启用”
]
}
}
}
]
}
```
* 限制用户可以启动的实例类型
```
{
“版本”：“2012-10-17”，
“声明”：[
{
“Sid”：“SageMakerLimitInstanceTypes”，
“效果”：“拒绝”，
“动作”：“SageMaker：CreateNotebookInstance”，
“资源”：“*”，
“状况”：{
“foranyValue: stringnotLike”: {
“SageMaker: InstanceTypes”: [
“[示例_实例_类型]”，
“ml.c5.xlarge”，
“ml.m5.xlarge”，
“ml.t3.medium”
]
}
}
}
]
}
```

* 同样，对于 Studio，请参阅以下示例政策。 请注意，管理员需要允许默认 Jupyter Server 应用程序使用系统实例。
```
{
“版本”：“2012-10-17”，
“声明”：[
{
“Sid”：“SageMaker允许的实例类型”，
“效果”：“拒绝”，
“动作”：[
“SageMaker: CreateApp”
]，
“资源”：“*”，
“状况”：{
“foranyValue: stringnotLike”: {
“SageMaker: InstanceTypes”: [
“ml.c5.large”,
“ml.m5.large”,
“ml.t3.medium”，
“系统”
]
}
}
}
]
}
```


## 检测

### SageMaker 检查

### AWS Config

AWS Config 有几个 [用于评估 SageMaker 的托管规则] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
* sagemaker端点配置kms密钥配置
* sagemaker-endpoint-config-prod-sance-
* sagemaker-notebook-instance-inside-vpc
* sagemaker-notebook-instance-key-key 配置了
rdsessick
* sagemaker-notebook 无法直接访问互联网

### CloudTrail 事件

如果您的环境遭到未经授权的访问，请参阅下文，了解与相关 SageMaker API 调用相关的可能场景，作为快速参考（请注意，这不是 SageMaker API 调用的完整列表）：


**数据/模型泄露：**
-从 SageMaker 数据存储或模型工件中复制或下载敏感数据。
-提取模型参数或训练数据，可能泄露知识产权或个人信息。

<ins>API 调用示例：</ins>
-“describeModelPackage”，用于检索有关模型包的信息。
-“describeTrainingJob” 访问训练作业的详细信息及其输出数据。
-“getModelPackageMetrics” 用于检索模型指标和潜在的敏感数据。



**数据中毒：**
* 通过注入恶意数据或对抗性示例来修改或毒化训练过的模型。
* 将受感染的模型部署到 SageMaker 端点，从而导致错误的预测或恶意输出。

<ins>API 调用示例：</ins>
-“createModelPackage” 或 “updateModelPackage” 来部署受损的模型包。
-`createTransformJob` 使用中毒的模型执行转换作业。
-`createEndpointConfig`和`createEndPoint`用于将恶意模型部署到端点。
-“CreateTrainingJob” 或 “updateTrainingJob”，用于将恶意数据注入训练作业。



**资源滥用：**
* 启动未经授权的 SageMaker 笔记本电脑或实例进行加密挖矿或其他恶意活动。
* 使用 SageMaker 资源作为在 AWS 环境中横向移动的入口点或枢轴点。

<ins>API 调用示例：</ins>
-“createNotebookInstance” 或 “updateNotebookInstance” 以启动未经授权的笔记本实例。
-“CreateTrainingJob” 或 “createHyperParameterTuningJob” 来启动过多的训练作业。
-“createEndpoint” 或 “updateEndPoint”，用于为未经授权的目的创建或修改端点。

**拒绝服务 (DoS): **
* 启动过多的训练任务或端点，从而耗尽 SageMaker 资源（例如计算实例、存储）。
* 请求量大，SageMaker API 或服务不堪重负，导致服务中断。

<ins>API 调用示例：</ins>
-“CreateTrainingJob” 或 “createHyperParameterTuningJob”，启动大量训练任务并耗尽资源。
-“createEndpoint” 或 “updateEndPoint” 来创建多个端点并消耗过多的计算资源。

**配置更改：**
* 修改 SageMaker 角色、策略或权限以提升权限或授予未经授权的访问权限。
* 更改 SageMaker VPC 配置、安全组或网络设置以绕过安全控制。

<ins>API 调用示例：</ins>
-“CreateRole” 或 “updateRole” 来修改 SageMaker 角色和权限。
-“创建 NotebookInstanceLifeCycleConfig” 或 “updateNotebookInstanceLifeCycleConfig” 来更改笔记本实例配置。
-“createEndpointConfig” 或 “updateEndpointConfig” 来更改端点配置或安全设置

**篡改日志：**
* 修改或删除 SageMaker 日志或审计跟踪，以掩盖轨迹并阻碍事件调查。
* 注入虚假日志条目以误导安全分析师或隐藏恶意活动。

<ins>API 调用示例：</ins>
-“putModelPackageModelMetrics” 在日志中注入虚假的模型指标。
-“stopTrainingJob” 或 “stopTransformJob” 可能会修改或删除日志数据。

**恶意软件部署：**
* 在 SageMaker 笔记本电脑或实例中部署恶意软件或后门程序，以实现持续访问或数据盗窃。
* 使用 SageMaker 资源分发恶意软件或对其他系统或网络发起攻击。

<ins>API 调用示例：</ins>
-“createNotebookInstance” 或 “updateNotebookInstance” 以启动带有恶意软件的笔记本实例。
-“createModelPackage” 或 “updateModelPackage”，用于部署包含恶意代码的模型包。

**凭证盗窃：**
* 窃取存储在笔记本或实例中的 AWS 凭证或 SageMaker API 密钥。
* 使用被盗的凭证进一步未经授权访问其他 AWS 资源或服务。

<ins>API 调用示例：</ins>
-“describeNotebookInstance” 或 “describeTrainingJob” 可能会访问存储的凭证或 API 密钥。
-“getModelPackageModelMetrics” 或 “describeModelPackage” 以检索敏感信息或证书。

**加密劫持：**
* 劫持 SageMaker 计算资源（例如实例、端点），用于未经授权的加密货币挖矿活动。
* 消耗过多的计算资源，并可能导致服务中断或成本增加。

<ins>API 调用示例：</ins>
-“createNotebookInstance” 或 “updateNotebookInstance”，用于启动用于加密货币挖矿的实例。
-“CreateTrainingJob” 或 “createHyperParameterTuningJob”，用于启动用于挖矿目的的计算密集型作业。


**注意：请务必注意，这些 API 调用也可以用于合法目的，但在未经授权的访问中，它们可能会被滥用来执行危险的操作。 实施强大的访问控制、监控和审计机制对于检测和防止此类滥用 SageMaker API 和资源至关重要。 **

### 理解 SageMaker 日志条目

以下屏幕截图为事件响应者提供了视觉帮助，以帮助解释调查期间发现的事件。 下面的每张图片都表示所采取的与记录的事件名称相匹配的操作

---

**创建域名**
<details>
<summary>展开屏幕截图</summary>
-
创建域。 域由关联的 Amazon Elastic 文件系统卷、授权用户列表以及各种安全、应用程序、策略和亚马逊虚拟私有云 (VPC) 配置组成。 网域内的用户可以相互共享笔记本文件和其他工件。

！ [CreateDomain] (/sagemaker_images/sagemaker-01.png)
</details>

---

**SageMaker 域名详情**
<details>
<summary>展开屏幕截图</summary>

亚马逊 SageMaker 域支持 SageMaker 机器学习 (ML) 环境。 SageMaker 域由以下实体组成：域、用户配置文件、共享空间、应用程序


！ [domainDetails] (/sagemaker_images/sagemaker-02.png)
！ [domainDetails2] (/sagemaker_images/sagemaker-03.png)

</details>

---


**CreateDomain的云端追踪事件**

<details>
<summary>展开屏幕截图</summary>

请注意，Cloudtrail 中的 “CreateDomain” 事件包含以下所有信息：VPC、子网、执行角色、应用程序等。

！ [带有关联 VPC、执行角色、应用程序等的 SageMaker 域] (/sagemaker_images/sagemaker-04.png)


</details>

---

**CreateEndPoint 的 CloudTra

<details>
<summary>展开屏幕截图</summary>

请注意，CloudTrail 中 SageMaker 的 “CreateEndPoint” 事件由 “sagemaker-executionRole” 服务角色调用

！ [创建端点示例] (/sagemaker_images/sagemaker-05.png)

</details>

---



## 分析

如果发生事件，除了调查泄露指标、威胁行为者、时间范围等外，在确认这是与 SageMaker 资源有关的事件后，还有一些其他问题需要考虑：

1。 哪些 SageMaker 资源是在未经授权的情况下被访问的？ （例如，笔记本电脑、模型、端点、数据存储）
2。 未经授权的访问是如何获得的？ （例如，凭证泄露、权限配置错误、漏洞被利用）
3。 在未经授权的访问期间，对受影响的 SageMaker 资源执行了哪些操作？
4。 是否有任何模型或数据被泄露或篡改？
5。 如果访问了模型，是否存在模型中毒或对抗攻击的风险？
6。 在未经授权的访问期间，是否创建或修改了任何新资源（例如笔记本电脑、端点）？
7。 在未经授权的访问期间是否使用了任何 SageMaker API 或 SDK，以及通过它们执行了哪些操作？
8。 为了掩盖轨迹，是否修改或删除了任何 SageMaker 日志或审计记录？
9。 事件期间是否修改或滥用了任何 SageMaker 角色或 IAM 策略？
10。 有没有更改 SageMaker VPC 配置或网络设置？
11。 是否有任何 SageMaker 笔记本电脑或实例用作入口点或枢轴点，以便进一步进行未经授权的访问？
12。 是否有任何 SageMaker 资源用于对其他 AWS 资源或外部系统发起攻击或恶意活动？
13。 未经授权的访问会对 SageMaker 资源和相关数据的机密性、完整性和可用性产生什么潜在影响？
14。 如何安全隔离、备份受影响的 SageMaker 资源，并有可能恢复或重建？
15。 没有遵循哪些具体的 SageMaker 安全最佳实践或配置，从而导致未经授权的访问？

## 遏制和根除

如果任何资源是由未经授权的用户创建的，或者资源未被授权创建，请按照以下说明删除/修改已创建的资源或权限：

-[如何删除 SageMaker 域名（控制台）] (https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-delete-domain.html#gs-studio-delete-domain-studio)
-[如何删除 SageMaker 域名 (CLI)] (https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-delete-domain.html#gs-studio-delete-domain-cli)
-[如何删除 SageMaker Endpoint] (https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-endpoint)
-[如何删除 SageMaker 端点配置] (https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-endpoint-config)
-[如何删除 SageMaker 模型] (https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-model)
-[从 SageMaker 笔记本中删除 Root 访问权限] (https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-root-access.html)（转到所需的笔记本实例/停止实例/完成后，单击 “编辑”/“权限” 下，选择 “禁用/更新笔记本实例/运行实例”）


## 已解决的待办事项列表
-作为事件响应者，我需要能够监控所有关键的 SageMaker 事件
-作为一名事件响应者，我需要一本关于大规模查询 SageMaker Cloudtrail 事件的剧本

## 当前待办事项列表

## 附录-最佳实践

**强烈推荐**

-[在隔离的 VPC 中部署] (https://docs.aws.amazon.com/sagemaker/latest/dg/infrastructure-connect-to-resources.html)
-使用 VPC 终端节点访问资源
-[Sagemaker 笔记本电脑应限制从 VPC 内部访问连接] (https://docs.aws.amazon.com/sagemaker/latest/dg/infrastructure-connect-to-resources.html)
-使用安全组和 NACL 来控制进出环境的流量
-[用于具有多个计算实例的训练作业的容器间流量加密] (https://docs.aws.amazon.com/sagemaker/latest/dg/train-encrypt.html)
-[使用 KMS 启用静态加密] (https://docs.aws.amazon.com/sagemaker/latest/dg/encryption-at-rest.html)
-[如果不需要，请禁用笔记本电脑的 root 访问权限] (https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-root-access.html)
-[生命周期配置最佳实践] (https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-lifecycle-config-install.html)
-创建允许的软件包列表，团队可以使用该列表来降低恶意代码运行的风险
-[使用 IAM 角色和基于资源的策略（例如用于访问 S3 存储桶数据的存储桶策略）的最低权限，利用机器学习治理] (https://docs.aws.amazon.com/sagemaker/latest/dg/governance.html)
-[使用 IAM 身份中心] (https://docs.aws.amazon.com/sagemaker/latest/dg/domain-user-profile-add-remove.html)
-[在 Secrets Manager 中存储和轮换凭证] (https://docs.aws.amazon.com/secretsmanager/latest/userguide/integrating-sagemaker.html)
-[使用 SageMaker 模型监视器监控模型输入和输出] (https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-faqs.html)
-[启用 CloudTrail S3 数据事件记录以进行 S3 数据和模型构件审计] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html)
-[启用 SageMaker 实验并利用模型工件的版本控制] (https://docs.aws.amazon.com/sagemaker/latest/dg/experiments.html)
-[启用 VPC Flow Logs 以监控您的 VPC 中的网络流量] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)
-[使用 CodeArtifact 从互联网上下载所需的库/软件包] (https://aws.amazon.com/blogs/machine-learning/secure-aws-codeartifact-access-for-isolated-amazon-sagemaker-notebook-instances/)
-[CloudWatch 也可以用来监控 SageMaker] (https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html)

**鼓励**

-[使用 Amazon Macie 保护敏感的 S3 数据] (https://docs.aws.amazon.com/macie/latest/user/data-classification.html)
-[使用服务目录出售经过预先审查的 SageMaker 资源，例如笔记本电脑，以限制实例大小并强制使用安全配置] (https://aws.amazon.com/blogs/machine-learning/automate-a-centralized-deployment-of-amazon-sagemaker-studio-with-aws-service-catalog/)