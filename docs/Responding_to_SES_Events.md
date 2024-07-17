# Incident Response Playbook: Responding to Simple Email Service Events

## Points of Contact

Author:  `Author's Name`  
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
[AWS Cloud Adoption Framework Security Perspective](https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* **Directive**
* **Detective**
* **Responsive**
* **Preventative**

![Image](/images/aws_caf.png)
* * *

### Response Steps
1. [**PREPARATION**] Use AWS GuardDuty detections for IAM
2. [**PREPARATION**] Identify, document, and test escalation Procedures
3. [**DETECTION AND ANALYSIS**] Perform detection and analyze CloudTrail for unrecognized API events
4. [**DETECTION AND ANALYSIS**] Perform detection and analyze CloudWatch for unrecognized events
3. [**CONTAINMENT & ERADICATION**] Delete or rotate IAM User Keys
4. [**CONTAINMENT & ERADICATION**] Delete or rotate unrecognized resources
5. [**CONTAINMENT & ERADICATION**] Rotate SMTP Credentials
6. [**RECOVERY**] Execute recovery procedures as appropriate

***The response steps follow the Incident Response Life Cycle from [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)  
### Incident Classification & Handling
* **Tactics, techniques, and procedures**: Tool: AWS Management Console
* **Category**: Log Analysis
* **Resource**: SES
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

## Executive Summary
This playbook outlines the process for responses to attacks against AWS Simple Email Service (SES). In combination with this guide, please review the [Amazon SES Sending review process FAQs](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/faqs-enforcement.html) for answers to enforcement actions and responses to adverse SES usage.

For additional information, please review the [AWS Security Incident Response Guide](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

## Preparation - General
* Assess your security posture to identify and remediate security gaps
    *  AWS developed a new open source [Self-Service Security Assessment](https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) tool that provides customers with a point-in-time assessment to gain valuable insights into the security posture of their AWS account.
* Maintain a complete asset inventory of all resources including servers, networking devices, network/file shares and developer machines
* Consider implementing [AWS GuardDuty](https://aws.amazon.com/guardduty/) to continuously monitor for malicious activity and unauthorized behavior to protect your AWS accounts, workloads, and data stored in Amazon SES
* Implement [CIS AWS Foundations](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html) including expiration of accounts and mandatory credential rotations
* **Enforce multi-factor authentication (MFA)**
* Enforce password complexity requirements and establish expiration periods
* Run an [IAM Credential Report](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) to list all users in your account and the status of their various credentials, including passwords, access keys, and MFA devices
* Use [AWS IAM Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) to identify the resources in your organization and accounts, such as IAM roles that are shared with an external entity. This lets you identify unintended access to your resources and data, which is a security risk

## Preparation - SES Specific
* Use Sending Authorization Policies to control who can send email and from where
    * https://docs.aws.amazon.com/ses/latest/dg/sending-authorization.html
* Use configuration sets to create didicated IP address pools permitted to send various types of messages
    * https://docs.aws.amazon.com/ses/latest/dg/using-configuration-sets.html
* Consider using Dedicated IP addresses for Amazon SES
    * https://docs.aws.amazon.com/ses/latest/dg/dedicated-ip.html
* Manage your own lists for mailing and subscriptions as well as for email suppression in Amazon SES
    * https://docs.aws.amazon.com/ses/latest/dg/lists-and-subscriptions.html
* Setup Amazon SES event publishing for real-time notifications
    * https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-using-event-publishing-setup.html
* Use least-privilege Identity and Access Management in Amazon SES
    * https://docs.aws.amazon.com/ses/latest/dg/control-user-access.html
* Review SES best practices, focusing on Security and Access
    * https://docs.aws.amazon.com/ses/latest/DeveloperGuide/best-practices.html (Classic)
    * https://docs.aws.amazon.com/ses/latest/DeveloperGuide/control-user-access.html (Classic)
* Setup SPF, DKIM, and DMARC for your own domain to help prevent phishing and spoofing
    * https://docs.aws.amazon.com/ses/latest/dg/email-authentication-methods.html

### Potential AWS GuardDuty Detections
The following findings are specific to IAM entities and access keys and always have a Resource Type of AccessKey. The severity and details of the findings differ based on the finding type. For more information on each finding type please reference the [GuardDuty IAM finding types](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-iam.html) webpage.  

* CredentialAccess:IAMUser/AnomalousBehavior
* DefenseEvasion:IAMUser/AnomalousBehavior
* Discovery:IAMUser/AnomalousBehavior
* Exfiltration:IAMUser/AnomalousBehavior
* Impact:IAMUser/AnomalousBehavior
* InitialAccess:IAMUser/AnomalousBehavior
* PenTest:IAMUser/KaliLinux
* PenTest:IAMUser/ParrotLinux
* PenTest:IAMUser/PentooLinux
* Persistence:IAMUser/AnomalousBehavior
* Policy:IAMUser/RootCredentialUsage
* PrivilegeEscalation:IAMUser/AnomalousBehavior
* Recon:IAMUser/MaliciousIPCaller
* Recon:IAMUser/MaliciousIPCaller.Custom
* Recon:IAMUser/TorIPCaller
* Stealth:IAMUser/CloudTrailLoggingDisabled
* Stealth:IAMUser/PasswordPolicyChange
* UnauthorizedAccess:IAMUser/ConsoleLoginSuccess.B
* UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.OutsideAWS
* UnauthorizedAccess:IAMUser/MaliciousIPCaller
* UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom
* UnauthorizedAccess:IAMUser/TorIPCaller

## Escalation Procedures
- `I need a business decision on when forensics should be conducted`
- `Who is monitoring the logs/alerts, receiving them and acting upon each?`
- `Who gets notified when an alert is discovered?`
- `When do public relations and legal get involved in the process?`
- `When would you reach out to AWS Support for help?`

## Detection and Analysis
### CloudTrail
Amazon SES is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in Amazon SES. CloudTrail captures API calls for Amazon SES as events. The calls captured include calls from the Amazon SES console and code calls to the Amazon SES API operations.  

Following are specific CloudTrail `eventName` events to look for changes in your account related to SES:

* DeleteIdentity
* DeleteIdentityPolicy
* DeleteReceiptFilter
* DeleteReceiptRule
* DeleteReceiptRuleSet
* DeleteVerifiedEmailAddress
* GetSendQuota
* PutIdentityPolicy
* UpdateReceiptRule
* VerifyDomainDkim
* VerifyDomainIdentity
* VerifyEmailAddress
* VerifyEmailIdentity  

[Viewing events with CloudTrail Event history](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)  

### Other Detective Controls
* Log and monitor SES events (e.g. sending, bounces, complaints, etc)
    * https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-activity.html
    * https://aws.amazon.com/blogs/messaging-and-targeting/handling-bounces-and-complaints/
* Monitor sender reputation metrics
    * https://docs.aws.amazon.com/ses/latest/dg/monitor-sender-reputation.html
* Setup appropriate Cloudwatch Alarms or dashboard for SES metrics
    * https://docs.aws.amazon.com/ses/latest/dg/security-monitoring-overview.html

## Containment and Eradication
* [Delete or rotate IAM User Keys](https://console.aws.amazon.com/iam/home#users) and [Root User Keys](https://console.aws.amazon.com/iam/home#security_credential); you may wish to rotate all keys in your account if you cannot identify a specific key or keys that has been exposed
* [Delete unauthorized IAM Users](https://console.aws.amazon.com/iam/home#users.)
* [Delete unauthorized policies](https://console.aws.amazon.com/iam/home#/policies)
* [Delete unauthorized roles](https://console.aws.amazon.com/iam/home#/roles)
* [Revoke temporary credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Temporary credentials can also be revoked by deleting the IAM User.
    * NOTE: Deleting IAM Users may impact production workloads and should be done with care
* [Rotate SMTP Credentials](https://aws.amazon.com/premiumsupport/knowledge-center/ses-create-smtp-credentials/)

## Recovery
* Create new IAM users with least-privilege access policies
* Implement steps and resources found in section `Preparation - SES Specific`
* Log and monitor SES events (e.g. sending, bounces, complaints, etc)
    * https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-activity.html
    * https://aws.amazon.com/blogs/messaging-and-targeting/handling-bounces-and-complaints/
* Monitor sender reputation metrics
    * https://docs.aws.amazon.com/ses/latest/dg/monitor-sender-reputation.html
* Setup appropriate Cloudwatch Alarms or dashboard for SES metrics
    * https://docs.aws.amazon.com/ses/latest/dg/security-monitoring-overview.html

## Lessons Learned
`This is a place to add items specific to your company that do not necessarilly need "fixing", but are important to know when executing this playbook in tandem with operational and business requirements.`

## Addressed Backlog Items

## Current Backlog Items
