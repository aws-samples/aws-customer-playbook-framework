This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2021 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

## Points of Contact

Author: `Author Name`  
Approver: `Approver Name`  
Last Date Approved:   

## Executive Summary
This playbook outlines the process to identify owners of code resources, determine how they were exposed, who may have accessed the exposed code, determine impact of exposure, required remediation actions, and determine root cause of code exposure.

## Potential Indicators of Compromise
- DLP Alerts
- Known code publication or exposure
- Suspicious CloudTrail logs

## Potential AWS GuardDuty Findings
- Behavior:EC2/NetworkPortUnusual
- Behavior:EC2/TrafficVolumeUnusual
- CredentialAccess:IAMUser/AnomalousBehavior
- DefenseEvasion:IAMUser/AnomalousBehavior
- Discovery:IAMUser/AnomalousBehavior
- Exfiltration:IAMUser/AnomalousBehavior
- Exfiltration:S3/MaliciousIPCaller
- Exfiltration:S3/ObjectRead.Unusual
- Impact:IAMUser/AnomalousBehavior
- InitialAccess:IAMUser/AnomalousBehavior
- Persistence:IAMUser/AnomalousBehavior
- PrivilegeEscalation:IAMUser/AnomalousBehavior
- UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.InsideAWS
- UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.OutsideAWS

### Objectives
Throughout the execution of the playbook, focus on the _***desired outcomes***_, taking notes for enhancement of incident response capabilities.

#### Determine: 
* **Code copies, transfers, and publication**
* **Credential exposure**
* **Vulnerabilities exposed**
* **Enviormental intelligence exposed**
	* **Tools used**
	* **Configuration information**
* **Actor's intent**
* **Actor's attribution**
* **Other damage inflicted to the environment and business**
* **Risk created for environment and business**

#### Recover:
* **Expire or reset any exposed authentication credentials**
* **Enumerate risks for potential attack vectors based on exposed environmental intelligence**
* **Minimize or eliminate risks of exposed code**

#### Enhance CAF Security Perspective components:
[AWS Cloud Adoption Framework Security Perspective](https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* **Directive**
* **Detective**
* **Responsive**
* **Preventative**

![Image](/images/aws_caf.png)
* * *

### Response Steps
1. [**PREPARATION**] Create an account asset inventory
2. [**PREPARATION**] Create a repository inventory
3. [**PREPARATION**] Enable logging as appropriate
4. [**PREPARATION**] Identify what type of data is in each repository
5. [**PREPARATION**] Identify, document, and test escalation procedures
6. [**PREPARATION**] Implement training to address exfiltration attacks
7. [**DETECTION AND ANALYSIS**] Perform exfiltration and DLP checks
8. [**DETECTION AND ANALYSIS**] Review respository (CodeCommit) read and write actions
9. [**DETECTION AND ANALYSIS**] Review DNS logs
10. [**DETECTION AND ANALYSIS**] Review VPC flow logs
11. [**DETECTION AND ANALYSIS**] Review endpoint / host based logs
12. [**CONTAINMENT**] Block access for affected accounts
13. [**ERADICATION**] Remove unrecognized and unauthorized objects in respositories
14. [**RECOVERY**] Perform recovery procedures as appropriate

***The response steps follow the Incident Response Life Cycle from [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)  

![Image](/images/nist_life_cycle.png)***

### Incident Classification & Handling
* **Tactics, techniques, and procedures**: Data Exfiltration, AWS Service Abuse
* **Category**: Data Loss
* **Resource**: CodeCommit, S3, EC2
* **Indicators**: Cyber Threat Intelligence, Third Party Notice, CloudWatch Metrics
* **Log Sources**: DNS Logs, VPC Flow Logs, CloudTrail, CloudWatch
* **Teams**: Security Operations Center (SOC), Forensic Investigators, Cloud Engineering

## Incident Handling Process
### The incident response process has the following stages:
* Preparation
* Detection & Analysis
* Containment & Eradication
* Recovery
* Post-Incident Activity

## Preparation

This playbook references and integrates, where possible, with [Prowler](https://github.com/toniblyx/prowler) which is a command line tool that helps you with AWS security assessment, auditing, hardening and incident response.

It follows guidelines of the CIS Amazon Web Services Foundations Benchmark (49 checks) and has more than 100 additional checks including related to GDPR, HIPAA, PCI-DSS, ISO-27001, FFIEC, SOC2 and others. 

This tool provides a rapid snapshot of the current state of security within a customer environment. Additionally, [AWS Security Hub](https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) provides for automated compliance scanning and can [integrate with Prowler](https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Asset Inventory
Identify all existing resources and have an updated asset inventory list coupled with who owns each resource.  Source code may be stored in one of the following assets:
- (CodeCommit)[https://aws.amazon.com/codecommit/] repositories
- (S3)[https://aws.amazon.com/s3/] backed code storage
- (EC2)[https://aws.amazon.com/ec2/] self-hosted code storage mechanisms

#### Identify what data has been exfiltrated from each asset
- CloudWatch and CloudTrail logs stored in S3 can be queried using (Amazon Athena)[https://aws.amazon.com/athena/] to identify undesired actions
- (Amazon GuardDuty)[https://aws.amazon.com/guardduty/] may automatically detect anomalous traffic.  This can be accessed with (AWS Security Hub)[https://aws.amazon.com/security-hub/] or (Amazon Detective)[https://aws.amazon.com/detective/]
- Application and instance logs stored in S3 can also be queried using Athena.

### Training
- `What training is in place for analysts within the company to become familiar with AWS API (command-line environment), Code Commit, S3, RDS, and other AWS services?`
>>>
Opportunities here for Threat Detection and incident response include: \
[AWS RE:INFORCE](https://reinforce.awsevents.com/faq/) \
[Self-Service Security Assessment](https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

- `Which roles are able to make changes to services within your account?`
- `Which users have those roles assigned to them? Is least privilege being followed, or do super admin users exist?`
- `Has a security assessment been performed against your environment? Do you have a known baseline to detect "new" or "suspicious" things?`

### Communication Technology
- `What technology is used within the team/company to communicate issues? Is there anything automated?`
>>>
Telephone \
E-mail \
SMS \
AWS SES \
AWS SNS \
Slack \
Chime \
Teams \
Other? 
>>>

## Detection

### Logging and S3 Bucket Checks
- Ensure CloudTrail is enabled in all regions: `./prowler -c check21`
- Ensure CloudTrail log file validation is enabled: `./prowler -c check22`
- Ensure S3 bucket access logging is enabled on the CloudTrail S3 bucket: `./prowler -c check26`
- Ensure there are no S3 buckets open to Everyone or Any AWS user: `./prowler -c extra73`
- Identify the resources in your organization and accounts; such as Amazon S3 buckets or IAM roles; that are shared with an external entity: `./prowler -c extra769`
- Find resources exposed to the internet: `./prowler -g group17`

## Escalation Procedures
- `Who is monitoring the logs/alerts, receiving them and acting upon each?`
- `Who gets notified when an alert is discovered?`
- `When do public relations and legal get involved in the process?`
- `When would you reach out to AWS Support for help?`

## Analysis
It is highly recommended to export logs to a security incident event management (SIEM) solution (such as Splunk, ELK stack, etc.) to aid in viewing and analyzing a variety of logs for a more complete attack timeline analysis.

### CloudTrail: Public S3 Bucket
By default, CloudTrail logs API calls that were made in the last 90 days, but not log requests made to objects. You can see bucket-level events on the CloudTrail console. However, you can't view data events (Amazon S3 object-level calls) there—you must parse or query CloudTrail logs for them. 

1. Navigate to your [CloudTrail Dashboard](https://console.aws.amazon.com/cloudtrail)
1. In the left-hand margin select `Event History`
1. In the drop-down change from `Read-Only` to `Event Name`
1. Review CloudTrail logs for the eventnames `GetPublicAccessBlock` and `DeletePublicAccessBlock`

### CloudTrail: Public S3 Object
You can also get CloudTrail logs for object-level Amazon S3 actions. To do this, enable data events for your S3 bucket or all buckets in your account. When an object-level action occurs in your account, CloudTrail evaluates your trail settings. If the event matches the object that you specified in a trail, the event is logged. 

1. Navigate to your [CloudTrail Dashboard](https://console.aws.amazon.com/cloudtrail)
1. In the left-hand margin select `Event History`
1. In the drop-down change from `Read-Only` to `Event Name`
1. Review CloudTrail logs for the eventnames `GetObjectAcl` and `PutObjectAcl`

### VPC Flow Logs
VPC Flow Logs is a feature that enables you to capture information about the IP traffic going to and from network interfaces in your VPC. This can be useful for IP addresses discovered within CloudTrail to determine the types of external connections to any public resources. 

For further information and steps, including querying with Athena, please refer to the [AWS Documentation for VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html). It is recommended that Athena analysis be included in a separate playbook and linked to other relevant items. 

### Endpoint / Host Based
1. Navigate to your [CloudTrail Dashboard](https://console.aws.amazon.com/cloudtrail)
1. In the left-hand margin select `Event History`
1. In the drop-down change from `Read-Only` to `Event Name`
1. Review CloudTrail for `PutObject` and  `DeleteObject` requests from public IP addresses

- Review EC2 operating system and application logs for inappropriate logins, installation of unknown software, or the presence of unrecognized files. 

- It is highly recommended to have a third-party host-based intrusion detection system (HIDS) solution (such as OSSEC, Tripwire, Wazuh, [Amazon Inspector](https://aws.amazon.com/inspector/), other) 

## Containment

### S3 Block Public Access
aws s3api put-public-access-block --bucket bucket-name-here --public-access-block-configuration "BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true"

You can also review [Blocking public access to your Amazon S3 storage](https://aws.amazon.com/s3/features/block-public-access/) for additional details on blocking public S3 access across your account.

## Eradication

### S3 Remove Unrecognized / Unauthorized Objects
Remove any unrecognized objects from buckets

1. Sign in to the AWS Management Console and open the Amazon S3 console at https://console.aws.amazon.com/s3/
1. In the Bucket name list, choose the name of the bucket that you want to delete an object from.
1. Choose the name of the object that you want to delete.
1. To delete the current version of the object, choose Latest version, and choose the trash can icon.
1. To delete a previous version of the object, choose Latest version, and choose the trash can icon beside the version that you want to delete.

## Recovery
Same procedures as those listed for Eradication

## Preventative Actions

### Authentication Prowler Checks
- Ensure multi-factor authentication (MFA)  is enabled:
 `./prowler -c check12`

### S3 Prowler Checks
**Encryption**
- Check if S3 buckets have default encryption (SSE) enabled: `./prowler -c extra734`

**Disaster Recovery**
- Check if S3 buckets have object versioning enabled: `./prowler -c extra763`

### S3 Service Actions
[Prevent Users from Modifying S3 Block Public Access Settings](https://asecure.cloud/a/scp_s3_block_public_access/)

Regularly review bucket access and policies on a monthly basis and utilize [CloudWatch Events](https://docs.aws.amazon.com/codepipeline/latest/userguide/create-cloudtrail-S3-source-console.html) or Security Hub for automated detections

[Using versioning in S3 buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) to mitigate accidental or intentional deletion of top level objects

[Managing access with ACLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/acls.html) to limit unauthorized access to resources on a bucket and object level

### Amazon Macie
[Amazon Macie](https://aws.amazon.com/macie/) can detect stored credentials, private keys, and other access data by [using managed data identifiers](https://docs.aws.amazon.com/macie/latest/user/managed-data-identifiers.html) 

### AWS Config
[AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) has multiple automated rules to manage code exposure including [codebuild-project-envvar-awscred-check](https://docs.aws.amazon.com/config/latest/developerguide/codebuild-project-envvar-awscred-check.html) to check if credentials are stored in code.

### Overall Security Posture
Execute a [Self-Service Security Assessment](https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) against the environment to further identify other risks and potentially other public exposure not identified throughout this playbook.

## Lessons Learned
`This is a place to add items specific to your company that do not need "fixing," but are important to know when executing this playbook in tandem with operational and business requirements.`

## Addressed Backlog Items
- As an incident responder I need a runbook on how to detect code exposure
- As an incident responder I need a runbook on how to detect code exfiltration
- As an incident responder I need to be able to detect public resources (AMIs, EBS Volumes, ECR Repos, etc)
- As an incident responder I need to know which roles are capable of making critical changes within AWS
- As an incident responder I need a playbook on mitigating a code exposure and required escalation points
- As an incident responder I need documentation on logs required for different data classifications

## Current Backlog Items