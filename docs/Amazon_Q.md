# Incident Response Playbook: Responding to Amazon Q Security Events

This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2024 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

## Points of Contact

Author: `Author Name` \
Approver: `Approver Name` \
Last Date Approved:   

## Executive Summary
This playbook outlines example processes and queries to investigate security events involving Amazon Q. 

## Preparation
This playbook references and integrates, where possible, with [Prowler](https://github.com/toniblyx/prowler) which is a command line tool that helps you with AWS security assessment, auditing, hardening and incident response.

### Objectives
Throughout the execution of the playbook, focus on the _***desired outcomes***_, taking notes for enhancement of incident response capabilities.

#### Determine: 
* **Vulnerabilities exploited**
* **Exploits and tools observed**
* **Actor's intent**
* **Actor's attribution**
* **Damage inflicted to the environment and business**

#### Recover:
* **Return to original and hardened configuration**

#### Enhance CAF Security Perspective components:
[AWS Cloud Adoption Framework Security Perspective](https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* **Directive**
* **Detective**
* **Responsive**
* **Preventative**

![Image](/images/aws_caf.png)
* * *

### Incident Classification & Handling
* **Tactics, techniques, and procedures**: Amazon Q
* **Category**: Log Analysis
* **Resource**: Amazon Q
* **Indicators**: Cyber Threat Intelligence, Third Party Notice
* **Log Sources**: CloudTrail
* **Teams**: Security Operations Center (SOC), Forensic Investigators, Cloud Engineering

## Incident Handling Process
### The incident response process has the following stages:
* Preparation
* Detection & Analysis
* Containment & Eradication
* Recovery
* Post-Incident Activity

### Response Steps
1. [**PREPARATION**] 
2. [**PREPARATION**] 
3. [**PREPARATION**] 
4. [**DETECTION AND ANALYSIS**] Perform detection and analyze CloudTrail for unrecognized API events
5. [**DETECTION AND ANALYSIS**] Perform detection and analyze CloudWatch for unrecognized events
6. [**CONTAINMENT & ERADICATION**] Delete or rotate IAM User Keys
7. [**CONTAINMENT & ERADICATION**] Delete or rotate unrecognized resources

## Preparation
* Assess your security posture to identify and remediate security gaps
* AWS developed a new open source Self-Service Security Assessment tool that provides customers with a point-in-time assessment to gain valuable insights into the security posture of their AWS account.
* Maintain a complete asset inventory of all resources including servers, networking devices, network/file shares and developer machines
* Consider implementing AWS GuardDuty to continuously monitor for malicious activity and unauthorized behavior to protect your AWS accounts, workloads, and data stored in Amazon SES
* Implement CIS AWS Foundations including expiration of accounts and mandatory credential rotations
* Enforce multi-factor authentication (MFA)
* Enforce password complexity requirements and establish expiration periods
* Run an IAM Credential Report to list all users in your account and the status of their various credentials, including passwords, access keys, and MFA devices
* Use AWS IAM Access Analyzer to identify the resources in your organization and accounts, such as IAM roles that are shared with an external entity. This lets you identify unintended access to your resources and data, which is a security risk
* Consider implementing AWS GuardDuty to continuously monitor for malicious activity and unauthorized behavior to protect your AWS accounts, and workloads.

## Escalation Procedures
- `I need a business decision on when EC2 forensics should be conducted`
- `Who is monitoring the logs/alerts, receiving them and acting upon each?`
- `Who gets notified when an alert is discovered?`
- `When do public relations and legal get involved in the process?`
- `When would you reach out to AWS Support for help?`

## Amazon Q Model

Amazon Q is compromised of multiple components/services:

* Chat – Amazon Q answers natural language questions in English about AWS, including questions about AWS service selection, AWS Command Line Interface (AWS CLI) usage, documentation, and best practices. Amazon Q responds with information summaries or step-by-step instructions, and includes links to its information sources.
* Memory – Amazon Q uses the context of your conversation to inform future responses for the duration of your conversation.
* Code improvements and advice – Within IDEs, Amazon Q can answer questions about software development, improve your code, and generate new code.
* Troubleshoot and support – Amazon Q can help you understand errors in the AWS Management Console and provides access to live AWS Support agents to address your AWS questions and issues.
* Customer feedback – Amazon Q uses the information and feedback you submit through feedback forms to provide support or help fix technical issues.

## Detection and Analysis

### Notes and Considerations

* Q is not available in all regions (2024-05-22).  See [the developer guide](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/regions.html) for current region availability 
* Model Invocation Logging is currently not a feature of Q. There are plans to add it in the future.  This is a region level setting and is not default
* Endpoints:
    * Q Business (Developers): qbusiness.amazonaws.com
    * Q Chat: q.amazonaws.com

### Event Names

The event names listed in the table below are a quick reference for types of events typically found in CloudTrail during the investigation of a security event involving Q.  Of those listed in the table, the most common event names that are logged during normal model usage are:

* `GetConversation`
* `StartConversation`
* `GetIndex`
* `ListRetrievers`
* `GetApplications`

| **Event Names** | **Description** | 
| :-----------------| :----------------------------------------------- |
| ***General Permissions*** | --- |
| `GetConversation` | Grants permission to get individual messages associated with a specific conversation with Amazon Q | 
| `GetTroubleshootingResults` | Grants permission to get troubleshooting results with Amazon Q | 
| `SendMessage` | Grants permission to send a message to Amazon Q | 
| `StartConversation` | Grants permission to start a conversation with Amazon Q | 
| `StartTroubleshootingAnalysis` | StartTroubleshootingAnalysis	Grants permission to start a troubleshooting analysis with Amazon Q
| `StartTroubleshootingResolutionExplanation` | Grants permission to start a troubleshooting resolution explanation with Amazon Q| 
| --- | --- |
| ***Creating an application*** | -------- |
| `CreateApplication` | Grants permission to create an application | 
| `DeleteApplication` | Grants permission to delete an application | 
| `GetApplication` | Grants permission to get an application | 
| `ListApplications` | Grants permission to list the applications| 
| `UpdateApplication` | Grants permission to update an Application | 
| --- | --- |
| ***Creating an index*** | -------- |
| `CreateIndex` | Grants permission to create an index for a given application | 
| `DeleteIndex` | Grants permission to delete an index | 
| `GetIndex` | Grants permission to get an index| 
| `ListIndices` | Grants permission to list the indices of an application | 
| `UpdateIndex` | Grants permission to update an index | 
| --- | --- |
| ***Creating a retriever*** | -------- |
| `CreateRetriever` | Grants permission to create a retriever for a given application | 
| `DeleteRetriever` | Grants permission to delete a retriever | 
| `GetRetriever` | Grants permission to get a retriever | 
| `ListRetrievers` | Grants permission to list the retrievers of an application | 
| `UpdateRetriever` | Grants permission to update a Retriever | 
| --- | --- |
| ***Connecting data sources*** | -------- |
| `CreateDataSource` | Grants permission to create a data source for a given application and index | 
| `DeleteDataSource` | Grants permission to delete a DataSource | 
| `GetDataSource` | Grants permission to get a data source |
| `ListDataSources` | Grants permission to list the data sources of an application and an index | 
| `UpdateDataSource` | Grants permission to update a DataSource | 
| `StartDataSourceSyncJobs` | Grants permission to start Data Source sync job | 
| `StopDataSourceSyncJobs` | Grants permission to stop Data Source sync job | 
| `ListDataSourceSyncJobs` | Grants permission to get Data Source sync job history | 
| --- | --- |
| ***Uploading documents*** | -------- |
| `BatchPutDocument` | Grants permission to batch put document | 
| `BatchDeleteDocument` | Grants permission to batch delete document | 
| --- | --- |
| ***Chat and conversation management*** | -------- |
| `Chat` | Grants permission to chat using an application| 
| `ChatSync` | Grants permission to chat synchronously using an application | 
| `DeleteConversation` | Grants permission to delete a conversation |
| `ListConversations` | Grants permission to list all conversations for an application | 
| `ListMessages` | Grants permission to list all messages | 
| --- | --- |
| ***User and group management*** | -------- |
| `CreateUser` | Grants permission to create a user | 
| `DeleteUser` | Grants permission to delete a user | 
| `GetUser` | Grants permission to get a user |
| `UpdateUser` | Grants permission to update a user | 
| `PutGroup` | Grants permission to put a group of users | 
| `DeleteGroup` | Grants permission to delete a group | 
| `GetGroup` | Grants permission to get a group | 
| `ListGroups` | Grants permission to list groups | 
| --- | --- |
| ***Plugins*** | -------- |
| `CreatePlugin` | Grants permission to create a plugin for a given application | 
| `DeletePlugin` | Grants permission to delete a plugin | 
| `GetPlugin` | Grants permission to get a plugin |
| `UpdatePlugin` | Grants permission to update a plugin | 
| --- | --- |
| ***Admin controls and guardrails *** | -------- |
| `UpdateChatControlsConfiguration` | Grants permission to update chat controls configuration for an application | 
| `DeleteChatControlsConfiguration` | Grants permission to delete chat controls configuration for an application | 
| `GetChatControlsConfiguration` | Grants permission to get chat controls configuration for an application |

### Data plane events in CloudTrail

By default, CloudTrail doesn't log data events. The following shows the Amazon Q API operations logged to CloudTrail as data events. The Data event type (console) column shows the appropriate selection in the CloudTrail console. The Amazon Q resource types column shows the resources.type value that you would specify to log data events for the resource. More information is available in the [Q User Guide](https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/logging-using-cloudtrail.html).

**Amazon Q Business Applications**: AWS::QBusiness::Application
* ListDataSourceSyncJobs
* StartDataSourceSyncJob
* StopDataSourceSyncJob
* BatchPutDocument
* BatchDeleteDocument
* PutFeedback
* ChatSync
* DeleteConversation
* ListConversations
* ListMessages
* ListGroups
* DeleteGroup
* GetGroup
* PutGroup
* CreateUser
* DeleteUser
* GetUser
* UpdateUser
* ListDocuments

**Amazon Q Business Data Resource**: AWS::QBusiness::DataSource
* ListDataSourceSyncJobs
* StartDataSourceSyncJob
* StopDataSourceSyncJob

**Amazon Q Business Index**: AWS::QBusiness::Index
* DeleteGroup
* GetGroup
* PutGroup
* ListGroups
* ListDocuments
* BatchPutDocument
* BatchDeleteDocument

### Analysis

In the event of an incident, in addition to investigating the indicators of compromise, threat actor, timeframe, etc., here are some additional questions to consider once it has been confirmed that this is an incident relating to Amazon Q resources:

1. What resources were created and deleted?
2. What PlugIns were utilized?
3. What applications did Q have access to during the security incident? 
4. Were these applications accessed by Q?
5. Was any type of file uploaded to Q?
6. Does Q have access to any IDE environment?

## Containment and Eradication

* [Delete or rotate IAM User Keys](https://console.aws.amazon.com/iam/home#users) and [Root User Keys](https://console.aws.amazon.com/iam/home#security_credential); you may wish to rotate all keys in your account if you cannot identify a specific key or keys that has been exposed
* [Delete unauthorized IAM Users](https://console.aws.amazon.com/iam/home#users.)
* [Delete unauthorized policies](https://console.aws.amazon.com/iam/home#/policies)
* [Delete unauthorized roles](https://console.aws.amazon.com/iam/home#/roles)
* [Revoke temporary credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Temporary credentials can also be revoked by deleting the IAM User.
    * NOTE: Deleting IAM Users may impact production workloads and should be done with care
* [Rotate SMTP Credentials](https://aws.amazon.com/premiumsupport/knowledge-center/ses-create-smtp-credentials/)
* [How to Delete Amazon Q Application](https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-app-actions.html)
* [How to Delete Amazon Q Retriever](https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-native-retriever-actions.html)
* [How to Delete Amazon Q Kenra Retriever](https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-kendra-retriever-actions.html)
* [How to Delete Uploaded Documents](https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/delete-doc-upload.html)
* [How to Delete Amazon Q Data Source](https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-datasource-actions.html)
* [How to Delete Amazon Web Experience](https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-exp-actions.html)

## Recovery

* Create new IAM users with least-privilege access policies


## Data Protection and IAM Policies

### Data Protection:

* NEVER put confidential or sensitive information, such as your customers' email addresses, into tags or free-form text fields such as a Name field. This includes when you work with Amazon Q or other AWS services using the console, API, AWS CLI, or AWS SDKs. Any data that you enter into tags or free-form text fields used for names may be used for billing or diagnostic logs.
* Amazon Q stores your questions, its responses, and additional context, such as console metadata and code in your IDE, to generate responses to your questions. 
* Amazon Q, data is sent to and stored in a US region. Data collected during conversations with Amazon Q is stored in the US East (N. Virginia) region. Data collected during troubleshooting console error sessions is stored in the US West (Oregon) region.

### IAM Permissions:

* The `AmazonQFullAccess` managed policy provides full access to enable interactions with Amazon Q. All Admins and power users have access by default but for other users and roles, customer will need to attach the AWS Q managed policy. Permissions can be restricted based on Amazon Q actions. 
* To ask Amazon Q questions in the AWS Console, an IAM identity needs permissions for the following actions:
  * StartConversation [Example: "q:StartConversation"]
  * SendMessage

* To troubleshoot console errors with Amazon Q, an IAM identity needs permissions for the following actions:
  * StartTroubleshootingAnalysis
  * GetTroubleshootingResults
  * StartTroubleshootingAnalysis

* If one of these actions isn't explicitly allowed by an attached policy, an IAM permissions error is returned when you try to use Amazon Q.


## Lessons Learned
`This is a place to add items specific to your company that do not necessarilly need "fixing", but are important to know when executing this playbook in tandem with operational and business requirements.`

## Addressed Backlog Items

## Current Backlog Items