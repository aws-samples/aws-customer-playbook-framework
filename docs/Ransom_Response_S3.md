# Incident Response Playbook: Ransom Response for S3
This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2021 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

## Points of Contact

Author: `Author Name`   
Approver: `Approver Name`  
Last Date Approved:   

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

### Response Steps
1. [**PREPARATION**] Use AWS Config to view configuration compliance
2. [**PREPARATION**] Identify, document, and test escalation Procedures
3. [**DETECTION AND ANALYSIS**] Perform detection and analyze CloudTrail for unrecognized API 
4. [**RECOVERY**] Execute recovery procedures as appropriate

***The response steps follow the Incident Response Life Cycle from [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)  

![Image](/images/nist_life_cycle.png)***

### Incident Classification & Handling
* **Tactics, techniques, and procedures**: Ransom & Data Destruction
* **Category**: Ransom Attack
* **Resource**: S3
* **Indicators**: Cyber Threat Intelligence, Third Party Notice, Cloudwatch Metrics
* **Log Sources**: S3 Server Logs, S3 Access Logs, CloudTrail, CloudWatch, AWS Config
* **Teams**: Security Operations Center (SOC), Forensic Investigators, Cloud Engineering

## Incident Handling Process
### The incident response process has the following stages:
* Preparation
* Detection & Analysis
* Containment & Eradication
* Recovery
* Post-Incident Activity

## Executive Summary
This playbook outlines the process for responses to Ransom attacks against AWS Simple Storage Service (S3). 

For additional information, please review the [AWS Security Incident Response Guide](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

## Preparation
* Assess your security posture to identify and remediate security gaps 
    *  AWS developed a new open source [Self-Service Security Assessment](https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) tool that provides customers with a point-in-time assessment to gain valuable insights into the security posture of their AWS account.
* Maintain a complete asset inventory of all resources including servers, networking devices, network/file shares and developer machines
* Consider using [AWS Config](https://aws.amazon.com/config/), which is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources
* Consider implementing [AWS GuardDuty](https://aws.amazon.com/guardduty/) to continuously monitor for malicious activity and unauthorized behavior to protect your AWS accounts, workloads, and data stored in Amazon S3 
* Darkbit provides an example of [AWS S3 Data Loss Prevention](https://darkbit.io/blog/simple-dlp-for-amazon-s3) that can be used to identify unauthorized copying of objects. Other specific operations could be added in the pattern as well based upon your business use case(s)
* Use IAM roles to manage permissions
    * Implement least-privilege and do not allow s3:* permissions
* Implement [CIS AWS Foundations](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html) including expiration of accounts and mandatory credential rotations
* Enforce multi-factor authentication (MFA) 
* Enforce password complexity requirements and establish expiration periods
* Run an [IAM Credential Report](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) to list all users in your account and the status of their various credentials, including passwords, access keys, and MFA devices
* Use [AWS IAM Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) to identify the resources in your organization and accounts, such as Amazon S3 buckets or IAM roles that are shared with an external entity. This lets you identify unintended access to your resources and data, which is a security risk
* [Enable S3 Object Versioning](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) to allow restoral of modified objects
    *  To set the number of versions kept, set a lifecycle policy (http://docs.aws.amazon.com/AmazonS3/latest/dev/intro-lifecycle-rules.html) to apply to noncurrent versions
* [Enable S3 MFA delete](https://docs.aws.amazon.com/AmazonS3/latest/userguide/MultiFactorAuthenticationDelete.html) to prevent an attacker from disabling versioning and overwriting all objects within a bucket
* Consider using [S3 Object Lock](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html) so you can store objects using a write-once-read-many (WORM) model. Object Lock can help prevent objects from being deleted or overwritten for a fixed amount of time or indefinitely. 
    * In Object Lock Compliance Mode*,* a protected object version can't be overwritten or deleted by any user, including the root user in your AWS account. When an object is locked in compliance mode, its retention mode can't be changed, and its retention period can't be shortened. Compliance mode *ensures* that an object version can't be overwritten or deleted for the retention period's duration. 
* Consider using [S3 Intelligent Tiering](https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) for object backups and cost optimization
* Use and routinely audit bucket policies
    * Only make public what needs to be, and ensure all of objects are protected by being private
* Consider using [AWS Key Management Service (KMS)](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) keys to encrypt all objects and to prevent an attacker from applying their encryption key
* Consider using the [AWS S3 Block Public Access Feature](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) to mitigate unintentional exposure of objects
* Enable [CloudTrail event logging for S3 buckets and objects](https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html) containing sensitive or critical information. By default, CloudTrail trails don't log data events, but you can configure trails to log data events for S3 buckets that you specify, or to log data events for all the Amazon S3 buckets in your AWS account. 
* Enable [CloudTrail server level logging for S3 buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerLogs.html) and objects containing sensitive or critical information. Server access logging provides detailed records for the requests that are made to a bucket
*  For mission critical data uploads, consider [enabling replication](http://docs.aws.amazon.com/AmazonS3/latest/dev/crr.html) - same region or cross region. Cross region replication protects data against application defects and operator errors by maintaining a second copy in another region
* Take steps to [prevent any new credentials from being publicly exposed](http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)
*  While we cannot recommend specific third-party solutions, there are also products from our Partners, including Veritas and CommVault, and other backup software providers that have the native capability to backup objects in S3 to their managed backup storage locations inside or outside of S3 

### Use AWS Config to view configuration compliance:
1. Sign in to the AWS Management Console and open the AWS Config console at https://console.aws.amazon.com/config/
1. In the AWS Management Console menu, verify that the region selector is set to a region that supports AWS Config rules. For the list of supported regions, see AWS Config Regions and Endpoints in the Amazon Web Services General Reference
1. In the navigation pane, choose Resources. On the Resource inventory page, you can filter by resource category, resource type, and compliance status. Choose Include deleted resources if appropriate. The table displays the resource identifier for the resource type and the resource compliance status for that resource. The resource identifier might be a resource ID or a resource name
1. Choose a resource from the resource identifier column
1. Choose the Resource Timeline button. You can filter by Configuration events, Compliance events, or CloudTrail Events
1. Specifically focus on the following events:
    * s3-account-level-public-access-blocks
    * s3-account-level-public-access-blocks-periodic
    * s3-bucket-blacklisted-actions-prohibited
    * s3-bucket-default-lock-enabled
    * s3-bucket-level-public-access-prohibited
    * s3-bucket-logging-enabled
    * s3-bucket-policy-grantee-check
    * s3-bucket-policy-not-more-permissive
    * s3-bucket-public-read-prohibited
    * s3-bucket-public-write-prohibited
    * s3-bucket-replication-enabled
    * s3-bucket-server-side-encryption-enabled
    * s3-bucket-ssl-requests-only
    * s3-bucket-versioning-enabled
    * s3-default-encryption-kms

## Escalation Procedures
- `I need a business decision on when EC2 forensics should be conducted`
- `Who is monitoring the logs/alerts, receiving them and acting upon each?`
- `Who gets notified when an alert is discovered?`
- `When do public relations and legal get involved in the process?`
- `When would you reach out to AWS Support for help?`

## Detection and Analysis
* S3 Objects are deleted or entire S3 buckets are deleted
    * Note: With data destruction events, a ransom note may or may not be provided. Also ensure you check CloudWatch metrics and CloudTrail S3 events to verify if data exfiltration occurred or not to delineate between a ransom or data destruction attack
* S3 Objects are encrypted using a key from an account not owned by the customer
* Ransom note provided either as an object within the bucket or via e-mail to the customer
* Check your CloudTrail log for unsanctioned activity such as creation of unauthorized IAM users, policies, roles or temporary security credentials
* Review CloudTrail for unrecognized API calls. Specifically, look for the following events: 
    * DeleteBucket
    * DeleteBucketCors
    * DeleteBucketEncryption
    * DeleteBucketLifecycle
    * DeleteBucketPolicy
    * DeleteBucketReplication
    * DeleteBucketTagging
    * DeleteBucketPublicAccessBlock
* If S3 Server Access Logs are enabled, then look for high, sequential `REST.COPY.OBJECT_GET` from the same remote IP and requester.
* Check your CloudTrail log to review your AWS account for any unauthorized AWS usage, such as unauthorized EC2 instances, Lambda functions, or EC2 Spot bids. You can also check usage by logging into your AWS Management Console and reviewing each service page. The "Bills" page in the Billing console can also be checked for unexpected usage 
    *  Please keep in mind that unauthorized usage can occur in any region and that your console may show you only one region at a time. To switch between regions, you can use the dropdown in the top-right corner of the console screen

## Recovery
* It is recommended not pay the ransom
    * Paying the ransom is a gamble as to whether the criminal will honor the transaction after receiving payment
    * If no data backups exist, then you should do a cost benefit analysis and weigh the value of the data/reputational compromise against the payment to the attacker
    * You directly enable the attacker to continue their operations against your company or others if you choose to pay the ransom
* Visit https://www.nomoreransom.org/ to identify if a decryptor is available for the malware variant the infected your data
* [Delete or rotate IAM User Keys](https://console.aws.amazon.com/iam/home#users) and [Root User Keys](https://console.aws.amazon.com/iam/home#security_credential); you may wish to rotate all keys in your account if you cannot identify a specific key or keys that has been exposed
* [Delete unauthorized IAM Users](https://console.aws.amazon.com/iam/home#users.)
* [Delete unauthorized policies](https://console.aws.amazon.com/iam/home#/policies)
* [Delete unauthorized roles](https://console.aws.amazon.com/iam/home#/roles)
* [Revoke temporary credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Temporary credentials can also be revoked by deleting the IAM User. NOTE: Deleting IAM Users may impact production workloads and should be done with care * Use CloudEndure Disaster Recovery to select the latest recovery point before the ransomware attack or data corruption to restore your workloads on AWS
    * If using an alternate data backup strategy, validate the backups have not been infected and restore from the last scheduled event prior to the ransomware event
* Implement procedures under Protect before attempting to restore data or objects
* [Remove delete markers](https://docs.aws.amazon.com/AmazonS3/latest/userguide/RemDelMarker.html) for versioned objects
* Recreate deleted buckets
* [Restore objects from using S3 Intelligent Tiering](https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) object backups or replicated region bucket
* Note:  There is currently no "undelete" capability for S3, and AWS does not have the ability to recover data that has been deleted. In the current era of data storage compliance and regulations such as GDPR (https://gdpr-info.eu/) and CCPA (https://oag.ca.gov/privacy/ccpa), Amazon S3 cannot continue to store customer data that has been explicitly deleted from the customer’s account. Once an object is deleted, it can no longer be recovered by AWS regardless of how quickly the unintended deletion is reported to AWS

## Lessons Learned
`This is a place to add items specific to your company that do not necessarilly need "fixing", but are important to know when executing this playbook in tandem with operational and business requirements.`

## Addressed Backlog Items
- As an Incident Responder I need a runbook to conduct EC2 Forensics
- As an Incident Responder I need a business decision on when EC2 forensics should be conducted
- As an Incident Responder I need to have logging enabled in all regions that are enabled regardless of intention of use
- As an Incident Responder I need to be able to detect crypto mining on my existing EC2 instances

## Current Backlog Items
