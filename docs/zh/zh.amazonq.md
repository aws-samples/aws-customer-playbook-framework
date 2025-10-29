# 加快事件响应：使用 Amazon Q 构建智能安全手册助手

## 概述

本指南提供了部署专为安全事件响应和行动手册管理而设计的经过安全强化的 Amazon Q Business 应用程序的分步说明。 该解决方案创建了一个由 AI 驱动的搜索和聊天界面，允许安全团队使用自然语言查询快速查找安全手册并与之交互。

## 安全增强

与标准实施相比，此安全版本包括以下安全改进：

### 🔒 **增强安全功能**
-**KMS 加密**：所有 S3 存储桶都使用 AWS KMS 加密而不是基本的 AES256
-**访问日志**：使用专用日志存储桶进行全面的 S3 访问日志记录
-**最低权限 IAM **：适用于特定应用程序资源的 IAM 策略（无通配符）
-**安全用户管理**：移除了过于宽松的 IAM 群组，仅使用 Identity Center
-**增强监控**：具有 90 天自动保留政策的详细访问日志
-**数据保护**：在所有数据桶上启用 S3 版本控制

### 🛡️ **安全合规性**
-IAM 策略中没有通配符权限
-所有资源限定为特定的应用程序 ID
-所有 S3 存储桶均禁止公开访问
-静态和传输中的加密数据
-全面的审核日志

## 架构

该解决方案部署了：
-**集成了 IAM 身份中心的亚马逊 Q 企业应用程序**
-适用于 AWS 的**两个加密的 S3 存储桶**和自定义安全手册
-**具有生命周期管理功能的专用访问日志存储桶**
-带有可搜索文档属性的**Q 业务索引**
-**网络体验**用于自然语言交互
-**具有最低权限的 IAM 角色**

## 先决条件

-现有的 AWS IAM 身份中心实例
-在身份中心创建的用户
-AWS CLI 配置了适当的权限
-CloudFormation 部署权限

## 步骤 1：部署安全 CloudFormation 模板

1。 **下载安全模板**：`q-security Playbooks-secure.yaml`

2。 **获取您的身份中心实例 ARN **：
``bash
aws sso-admin 列表实例
```

3。 **部署模板**：
``bash
aws 云信息部署\
--template-file q-securityPlaybooks-security.yaml\
--stack-name q-security playbooks-secure\
--参数覆盖\
应用程序名称=安全手册\
awsplaybooksBucketname=aws-security-play
cxplaybooksBucketname=custom-security-p
identityCenterInstancearn=arn: aws: SSO::: instance/ssoins-xxxxxxxxxxx\
--能力能力_IAM\
--区域 us-east-1
```

4。 **等待部署完成**（通常为 5-10 分钟）

## 步骤 2：上传 AWS 安全行动手册

1。 **克隆 AWS 客户行动手册框架**：
``bash
vcursive https://github.com/aws-samples/aws-customer-playbook-framework.git
cd aws-客户剧本框架
```

2。 **上传到您的 AWS 剧本存储桶**：
``bash
aws s3 sync。s3://aws-security-playbooks-YOUR_ACCOUNT_ID/ —排除 “.git/*”
```

3。 **在 AWS 控制台中验证上传**或通过 CLI：
``bash
aws s3 ls recursive ls s3://aws-security-playbooks-YOUR_ACCOUNT_ID/
```

## 步骤 3：上传自定义安全手册

1。 **在本地目录中准备您的自定义剧本**

2。 **将自定义 playbook 上传到专用 S3 存储桶**：
``bash
aws s3 cp 你的自定义剧本/ s3://custom-security-playbooks-YOUR_ACCOUNT_ID/--recursive
```

3。 **通过查看 AWS 控制台中的存储桶内容来确认上传**

## 步骤 4：配置用户访问权限（仅限身份中心）

⚠️ **重要**：此安全版本删除了 IAM 用户组。 所有用户管理都必须通过 Identity Center 完成。

1。 **导航到 IAM 身份中心控制台**
2。 **确保在身份中心（不是 IAM）中创建用户**
3。 **前往 Amazon Q Business 控制台**
4。 **选择您的应用程序**
5。 **点击 “访问管理” **
6。 **仅从身份中心添加用户和群组**
7。 **根据用户角色分配适当的权限**

## 步骤 5：将 Playbook 与 Amazon Q 同步

1。 **导航至 Amazon Q 企业版控制台**
2。 **选择您的应用程序**
3。 **点击 “数据源” 选项卡**
4。 **对于每个数据源**（AWS 行动手册和自定义行动手册）：
-在列表中找到数据源
-点击 “立即同步”
-监控同步进度（可能需要几分钟）
5。 **验证两个数据源的同步成功**

## 步骤 6：设置 AWS Playbook 自动更新（增强安全性）

为自动更新创建具有增强安全性的 Lambda 函数：

1。 **导航到 AWS Lambda 控制台**
2。 **点击 “创建函数” **
3。 **选择名为 “updateawsPlaybooks-Secure” 的 “从头开始作者” **
4。 **选择 Python 3.11**（或最新版本）

5。 **使用此增强型安全代码**：
``python
导入 boto3
导入子进程
导入操作系统
vpcfl
从键入导入 Dict，Any
导入 json

vpcflows
logger = logging.getLogger ()
Logger.setLevel（logging.info

def lambda_handler（事件：Dict [str，Any]，上下文：任意）-> Dict [str，Any]：
“"”
安全 AWS Lambda 功能可将 AWS 客户手册框架同步到 S3。
通过更好的错误处理和安全日志进行增强。
“"”
试试：
# 清理所有现有文件
如果 os.path.exists ('/tmp/playbooks')：
subprocess.run (['rm'、'-rf '、' /tmp/playbooks ']、check=True)

# 通过安全验证从 GitHub 克隆最新版本
logger.info（“使用安全验证克隆存储库...”）
结果 = subprocess.run (
['git'、'clone'、'--depth'、'1'、'--single-branch'、
'https://github.com/aws-samples/aws-customer-playbook-framework.git'，
'/tmp/playbooks']，
capture_output=True，
text=true
check=True，
timeout=300 # 5 分钟超时
)

# 使用增强的安全性初始化 S3 客户端
s3 = boto3.client ('s3')
bucket_name = os.environ ['BUCKET_NAME']

# 验证存储桶是否存在并且我们有访问权限
试试：
s3.head_bucket (bucket=bucket_name)
除了异常，因为 e:
引发异常 (f “无法访问存储桶 {bucket_name}: {str (e)}”）

# 将包含元数据的文件上传到 S3
logger.info (“将文件上传到 S3 存储桶：{bucket_name}”)
文件已上传 = 0

对于 os.walk ('/tmp/playbooks') 中的 root、_、文件：
对于文件中的文件：
如果不是 file.startswith ('.git') 而不是 file.startswith (' 。 '):
local_path = os.path.join（根目录，文件）
s3_key = os.path.relpath（local_path，'/tmp/playbooks'）

# 添加用于跟踪的元数据
s3.upload_file (
文件名=本地路径，
bucket=bucket_name，
key=s3_key，
extraArgs= {
'元数据'：{
'source'：'自动同步'，
'sync-timestamp'：str（context.aws_request_id）
}，
'serversideEncryption': 'aws: kms
}
)
文件已上传 += 1

# 记录成功完成
logger.info (f “成功将 {files_uploaded} 文件上传到 S3”)

# 触发 Q Business 数据源同步
qbusiness = boto3.client ('qbusiness')
app_id = os.environ.get ('QBUSINESS_APP_ID')
data_source_id = os.environ.get ('QBUSINESS_DATA_SOURCE_ID')
index_id = os.environ.get ('QBUSINESS_INDEX_ID')

如果全部（[app_id、data_source_id、index_id]）：
试试：
qbusiness.start_data_source_sync_job
applicationid=app_id，
datasourceid=data_source_ID，
indexid=index_id
)
logger.info（“触发 Q Business 数据源同步”）
除了异常，因为 e:
logger.warning (f “无法触发 Q Business 同步：{str (e)}”）

返回 {
'状态码'：200，
'body'：json.dumps ({
'message'：f'成功将 {files_uploaded} 文件上传到 S3 '，
'bucket'：存储桶名称，
'files_uploaded'：文件已上传
})
}

subprocess.calledProcessError 除外 e:
error_message = f “Git 克隆失败：{e.stderr}”
logger.error（error_message）
引发异常（错误消息）

除了异常，因为 e:
error_message = F “出现错误：{str (e)}”
logger.error（error_message）
引发异常（错误消息）

最后：
# 安全清理
如果 os.path.exists ('/tmp/playbooks')：
subprocess.run (['rm'、'-rf '、' /tmp/playbooks ']、check=True)
```

6。 **配置环境变量**：
-“BUCKET_NAME”：你的 AWS 剧本 S3 存储桶名称
-`QBUSINESS_APP_ID`：你的 Q Business 应用程序 ID（来自 CloudFormation 的输出）
-`QBUSINESS_DATA_SOURCE_ID`：你的数据源 ID
-`QBUSINESS_INDEX_ID`：你的索引 ID

7。 **将执行超时**设置为 5 分钟
8。 **使用最低要求的权限配置增强型 IAM 角色**
9。 **部署功能**

## 步骤 7：通过增强安全性安排自动更新

1。 **前往亚马逊 EventBridge 控制台**
2。 **点击 “创建规则” **
3。 **配置规则**：
-名称：“SecurePlaybookupdate”
-描述：“安全的自动剧本更新”
-规则类型：“日程安排”
4。 **设置日程安排模式**（例如，世界标准时间每天凌晨 2 点）
5。 **添加目标**：
-服务：AWS Lambda
-功能：您的安全 Lambda 函数
6。 **添加输入转换器**以增强日志记录
7。 **查看并创建**

## 步骤 8：监控和测试安全解决方案

### 测试
1。 **通过身份中心门户网站打开您的 Amazon Q 应用程序**
2。 **测试与安全相关的查询**：
-“如何缓解勒索软件攻击？”
-“向我展示数据泄露事件响应程序”
-“解决 AWS Config 合规性问题的步骤是什么？”
3。 **验证回复**包括相关的剧本信息

### 监控
1。 **在您的专用日志存储桶中查看 S3 访问日志**
2。 **监控 CloudTrail **，查看 Q Business API 调用
3。 **查看 Lambda 执行日志**以了解同步操作
4。 **针对同步失败或访问异常情况设置 CloudWatch 警报**

## 安全监控与合规性

### 访问日志
-**S3 访问日志**：存储在专用存储桶中，保留期为 90 天
-**CloudTrail 集成**：记录并监控所有 API 调用
-**Lambda 执行日志**：详细的同步操作日志

### 安全最佳实践
-**常规访问权限审核**：每季度查看用户访问权限
-**Playbook 内容审计**：确保敏感信息得到正确分类
-**成本监控**：设置 AWS 预算以控制成本
-**安全扫描**：定期扫描上传的内容

## 成本估算（增强安全版）

安全版本包括可能会影响成本的其他组件：

| 组件 | 配置 | 每月成本 (美元) |
|-----------------------------------------------------------
| 亚马逊 Q Business Lite | 4,500 名用户 × 每位用户 3 美元 | 13,500 美元 |
| 亚马逊 Q Business Pro | 500 个用户 × 20 美元/用户 | 10,000 美元 |
| 企业指数 | 50 个指数单位 × 每小时 0.264 美元 | 9,504 美元 |
| AWS Lambda | 每月 100 万个请求 | 46.91 美元 |
| Amazon API Gateway | 每月 100 万个请求 | 5 美元 |
| **S3 访问日志** | **额外存储成本** | **~50-100美元** |
| **KMS 加密** | **其他密钥使用量** | **~10-20 美元** |
| **增强监控** | **CloudWatch 日志/指标** | **~25-50美元** |
| **估计总成本** | | **33,140-33,225美元** |

### 安全版本的成本优化
-监控访问日志存储并根据需要调整保留期
-使用 S3 Intelligent Tiering 进行日志存储
-优化 Lambda 执行频率
-定期清理未使用的资源
-设置详细的成本分配标签

## 清理（安全版）

为避免持续收费并确保彻底清理，请执行以下操作：

1。 **删除 CloudFormation 堆栈**：
``bash
aws cloudformation delete-stack-name q-security-playb
```

2。 **验证 S3 存储桶清理**：
-检查是否所有三个存储桶都已删除（数据 + 日志）
-如果删除失败，则手动清空存储桶

3。 **清理 Lambda 和 EventBridge **：
-删除 Lambda 函数
-移除赛事桥赛事规则

4。 **验证身份中心清理**：
-移除应用程序分配
-清理所有测试用户（如果已创建）

5。 **检查 KMS 密钥使用情况**：
-如果使用自定义密钥，请查看 KMS 密钥策略

## 安全注意事项

### 数据分类
-确保剧本不包含敏感凭据
-实施正确的数据分类标签
-定期对内容进行合规性审计

### 访问控制
-使用身份中心群组进行基于角色的访问
-实施最低权限原则
-定期访问审查和清理

### 监控和警报
-设置异常访问模式警报
-监控失败的身份验证尝试
-跟踪数据源同步失败

## 疑难解答

### 常见安全相关问题
1。 **拒绝访问错误**：验证身份中心用户分配
2。 **同步失败**：检查 IAM 角色权限的范围是否正确
3。 **缺少日志**：验证访问日志存储桶权限
4。 **KMS 错误**：确保正确的 KMS 密钥策略

### 支持资源
-AWS 支持 Q 业务问题
-用于模板疑难解答的 CloudFormation 文档
-用于用户管理的身份中心文档

## 后续步骤

1。 **根据您的组织需要实施其他安全控制**
2。 **设置对剧本内容的自动安全扫描**
3。 **与 SIEM 系统集成**以增强监控
4。 **针对您的环境开发自定义剧本**
5。 **培训安全团队**了解如何有效使用 Q Business
6。 **为剧本管理建立治理流程**

## 通知

本安全实施指南为 AWS Q Business 部署提供了增强的安全控制。 客户负责：
-确保遵守组织安全政策
-定期进行安全审查和更新
-正确的数据分类和处理
-监测和事件响应程序

AWS 产品和服务 “按原样” 提供，不提供担保。 本指南代表当前的最佳实践，可能会发生变化。 请务必查阅针对您的特定用例的最新 AWS 文档和安全最佳实践。

[q-security Playbooks-secure.json] (https://github.com/user-attachments/files/21760628/Q-SecurityPlaybooks-Secure.json)