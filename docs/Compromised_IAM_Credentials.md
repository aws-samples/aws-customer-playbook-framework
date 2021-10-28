# Incident Response Playbook: Compromised IAM Credentials
This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2021 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

## Points of Contact

Author: `Author Name` \
Approver: `Approver Name` \
Last Date Approved:   

## Executive Summary
This playbook outlines the process to respond when you observe unauthorized activity within your AWS account, or you believe that an unauthorized party accessed your account.

## Potential Indicators of Compromise
- New or unrecognized IAM users
- Unrecognized or unauthorized resources (e.g. EC2, Lambda)
- Unusual billing increases 
- Notification from security researcher
- Notification that my AWS resources or account might be compromised

## Potential AWS GuardDuty Findings
- CredentialAccess:IAMUser/AnomalousBehavior
- DefenseEvasion:IAMUser/AnomalousBehavior
- Discovery:IAMUser/AnomalousBehavior
- Exfiltration:IAMUser/AnomalousBehavior
- Impact:IAMUser/AnomalousBehavior
- InitialAccess:IAMUser/AnomalousBehavior
- PenTest:IAMUser/KaliLinux
- PenTest:IAMUser/ParrotLinux
- PenTest:IAMUser/PentooLinux
- Persistence:IAMUser/AnomalousBehavior
- Policy:IAMUser/RootCredentialUsage
- PrivilegeEscalation:IAMUser/AnomalousBehavior
- Recon:IAMUser/MaliciousIPCaller
- Recon:IAMUser/MaliciousIPCaller.Custom
- Recon:IAMUser/TorIPCaller
- Stealth:IAMUser/CloudTrailLoggingDisabled
- Stealth:IAMUser/PasswordPolicyChange
- UnauthorizedAccess:IAMUser/ConsoleLoginSuccess.B
- UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration
- UnauthorizedAccess:IAMUser/MaliciousIPCaller
- UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom
- UnauthorizedAccess:IAMUser/TorIPCaller

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
1. [**PREPARATION**] Perform an Asset Inventory
2. [**PREPARATION**] Implement a training plan to identify and respond to exposed IAM credentials
3. [**PREPARATION**] Implement a communication strategy for incident response
4. [**DETECTION**] Identify Root Account Access (authorized and not)
5. [**DETECTION**] Identify New or unrecognized IAM users
6. [**DETECTION**] Identify Unrecognized or unauthorized resources (e.g. EC2, Lambda)
7. [**DETECTION**] Identify and find exposed secrets
8. [**DETECTION**] Identify Unusual billing increases
9. [**DETECTION**] Respoond to notification from AWS or a third party that my AWS resources or account might be compromised
10. [**DETECTION**] Identify any potentially unauthorized IAM user credentials
11. [**PREPARATION**] Identify Escalation Procedures
12. [**DETECTION AND ANALYSIS**] Review CloudTrail Logs
13. [**DETECTION AND ANALYSIS**] Review VPC Flow Logs
14. [**DETECTION AND ANALYSIS**] Review Endpoint / Host Based Logs
15. [**CONTAINMENT**] Perform appropriate containment actions
16. [**ERADICATION**] Review the findings from Review CloudTrail event history for activity by the compromised access key
17. [**ERADICATION**] Review the Avoiding unexpected charges
18. [**RECOVERY**] Perform appropriate recovery actions
19. [**PREPARATION**] Perform a Prowler IAM Scan
20. [**PREPARATION**] Enable MFA
21. [**PREPARATION**] Verify your account information
22. [**PREPARATION**] Use AWS Git projects to scan for evidence of unauthorized use
23. [**PREPARATION**] Avoid using the root user for day-to-day operations
24. [**PREPARATION**] Evaluate your Overall Security Posture

***The response steps follow the Incident Response Life Cycle from NIST Special Publication 800-61r2
[NIST Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)
![Image](/images/nist_life_cycle.png)***

### Incident Classification & Handling
* **Tactics, techniques, and procedures**: Exposure of credentials
* **Category**: IAM credential exposure
* **Resource**: IAM
* **Indicators**: Cyber Threat Intelligence, Third Party Notice
* **Log Sources**: AWS CloudTrail, AWS Config, VPC Flow Logs, Amazon GuardDuty
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

This tool provides a rapid snapshot of the current state of security within a customer environment. As an alternative, [AWS Security Hub](https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) provides for automated complaince scanning and can [integrate with Prowler](https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Asset Inventory
Identify all existing users and have an updated list of the purpose for each account

1. Navigate to [AWS Console](https://console.aws.amazon.com/)
1. Go to `Services` and select `IAM`
1. In the left-hand menu, select `Credential report`
1. Select `Download Report`
1. Pay special attention to when accounts were created, password last used, and password last changed columns. 
    * **NOTE** if a user has multiple access keys, validate the purpose for each

### Training
- `What training is in place for analysts within the company to become familiar with AWS API (command-line environment), S3, RDS, and other AWS services?`
>>>
Opportunities here for Threat Detection and incident response include: \
[AWS RE:INFORCE](https://reinforce.awsevents.com/faq/) \
[Self-Service Security Assessment](https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

- `Which roles are able to make changes to services within your account?`
- `Which users have those roles assigned to them? Is least privilege being followed, or do super admin users exist?`
- `Has a Security Assessment been performed against your environment, do you have a known baseline to detect "new" or "suspicious" things?`

### Communication Technology
- `What technology is used within the team/company to communicate issues? Is there anything automated?`
>>>
Telephone \
E-mail \
AWS SES \
AWS SNS \
Slack \
Chime \
Other? 
>>>

## Detection

### Root Account Access
It is recommended that all access keys associated with the root account be removed: `./prowler -c check_112`

### New or unrecognized IAM users
Review the IAM credential report from your [Asset Inventory](./Compromised_IAM_Credentials.md/#asset-inventory)
Check if IAM users have two active access keys: `./prowler -c check_extra712`
Ensure IAM policies that allow full \"*:*\" administrative privileges are not created: `./prowler -c check_122`
Check if IAM Access Analyzer is enabled and its findings: `./prowler -c check_extra769`

### Unrecognized or unauthorized resources (e.g. EC2, Lambda)
aws ec2 describe-instances
aws lambda list-functions

### Looking for Secrets
Potential secret found in EC2 instance User Data: `./prowler -c check_extra741`
Potential secret found in Lambda function variables: `./prowler -c check_extra759`
Potential secret found in ECS task definition variables: `./prowler -c check_extra768`
Potential secret found in Autoscaling Configuration: `./prowler -c check_extra775`

### Unusual billing increases 
To view your AWS bill, open the [Bills](https://console.aws.amazon.com/billing/home#) pane of the Billing and Cost Management console, and then choose the month you want to view from the drop-down menu.

You can view the history of your AWS payments in the [Orders and invoices](https://console.aws.amazon.com/billing/home?/paymenthistory) pane of the Billing and Cost Management console.

You can also use [AWS Cost Anomaly Detection](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-ad.html) for dynamic monitoring and alerting.

Check your bill for the following:
* AWS services that you don't normally use
* Resources in AWS Regions that you don't normally use
* A significant change in the size of your bill

You can use this information to help you to delete or terminate any resources you don't want to keep.

### Notification that my AWS resources or account might be compromised
If you received a notification from AWS about your account, sign in to the AWS Support Center, and then respond to the notification.

If you can't sign in to your account, use the Contact Us form to request help from AWS Support.

If you have any questions or concerns, create a new AWS Support case in the AWS Support Center.
    * **NOTE**: Don't include sensitive information in your correspondence, such as AWS access keys, passwords, or credit card information.
Use AWS Git projects to scan for evidence of unauthorized use

### Identify any potentially unauthorized IAM user credentials
1. Open the IAM console.
1. Choose Users in the navigation pane.
1. Choose each IAM user from the list, and then check under Permissions policies for a policy named AWSExposedCredentialPolicy_DO_NOT_REMOVE. 1. If the user has this attached policy, you must rotate the access keys for the user.

## Escalation Procedures
- `Who is monitoring the logs/alerts, receiving them and acting upon each?`
- `Who gets notified when an alert is discovered?`
- `When do public relations and legal get involved in the process?`
- `When would you reach out to AWS Support for help?`

## Analysis
It is highly recommended to export logs to a security incident event management (SIEM) solution (such as Splunk, ELK stack, etc.) to aide in viewing and analyzing a variety of logs for a more complete attack timeline analysis.

### CloudTrail
Look for unusual login activity
1. Navigate to your [CloudTrail Dashboard](https://console.aws.amazon.com/cloudtrail)
1. In the left-hand margin select `Event History`
1. In the drop-down change from `Read-Only` to `Event Name`
1. In the search field enter `ConsoleLogin` or `AssumeRole` or `GetFederationToken` or `GetCredentialReport` or `GenerateCredentialReport` and review the available events for any suspicious activity
    * **NOTE** userIdentify  will show up as `"type": "Root"` for root or `"type": "IAMUser"` for users 

Locate the IAM access key ID and user name used to launch a suspicious EC2 instance
1. Open the CloudTrail console, and then choose Event history.
1. Select the Filter drop-down menu, and then choose Resource name.
1. In the Enter resource name field, paste the EC2 instance ID, and then choose enter on your device.
1. Expand the Event name for RunInstances.
1. Copy the AWS access key, and note the User name.

Review CloudTrail event history for activity by the compromised access key
1. Open the CloudTrail console, and then choose Event history from the navigation pane.
1. Select the Filter dropdown menu, and then choose AWS access key filter.
1. In the Enter AWS access key field, enter the compromised IAM access key ID.
1. Expand the Event name for the API call RunInstances.
    * **NOTE**: You can only view event history for the last 90 days unless you've previously configured a trail that saves into an S3 bucket

### VPC Flow Logs
VPC Flow Logs is a feature that enables you to capture information about the IP traffic going to and from network interfaces in your VPC. This can be useful for IP addresses discovered within CloudTrail to determine the types of external connections to any public resources. 

For further information and steps, including querying with Athena, please refer to the [AWS Documentation for VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html). It is recommended that Athena analysis be included in a separate playbook and linked to other relavent items. 

### Endpoint / Host Based
* **Identify last Creation time for a given instance name**: `aws ec2 describe-instances --query 'Reservations[].Instances[].{ip: PublicIpAddress, tm: LaunchTime}' --filters 'Name=tag:Name,Values= myInstanceName' | jq 'sort_by(.tm) | reverse | .[0]'`

* **Identify Last Modified time for lambda functions**: `aws lambda list-functions | grep "LastModified"`

## Containment
1. Disable the IAM user, create a backup IAM access key, and then disable the compromised access key
    1. Open the [IAM console](https://console.aws.amazon.com/iam/), and then paste the IAM access key ID in the Search IAM bar.
    1. Choose the user name, and then choose the Security credentials tab.
    1. In Console password, choose Manage.
        * **NOTE**: If the AWS Management Console password is set to Disabled, you can skip this step.
    1. In Console access, choose Disable, and then choose Apply.
        1. Important: Users whose accounts are disabled can't access the AWS Management Console. However, if the user has active access keys, they can still access AWS services using API calls.
    1. For the compromised IAM access key, choose Make inactive.
1. Disable the application IAM key, create a backup IAM access key, and then disable the compromised access key 
    1. First, create a second key. Then, modify your application to use the new key.
    1. Disable (but do not delete) the first key.
    1. If there are any problems with your application, reactivate the key temporarily. When your application is fully functional, and the first key is in the disabled state, then delete the first key.
1. Rotate and delete all inactive/compromsied AWS access keys
    * **!!Ensure you keep a record of all deleted access keys so you can continue to search them in CloudTrail!!**

## Eradication
### Review the findings from [Review CloudTrail event history for activity by the compromised access key](./#cloudtrail) 
Remove any resources created by the compromised key(s). Be sure to check all AWS Regions, even Regions where you never launched AWS resources.
    * **Important**: If you need to keep any resources for investigation, consider backing them up. For example, if you have a regulatory, compliance, or legal need to retain an EC2 instance, take an EBS snapshot before terminating the instance.

### Review the [Avoiding unexpected charges](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/checklistforunwantedcharges.html) 
Check for and remove any services that are recognized within your account. Pay special attention to the following resources:
* EC2 instances and AMIs, including instances in the stopped state
* EBS volumes and snapshots
* AWS Lambda functions and layers

To delete Lambda functions and layers, do the following:
1. Open the Lambda console.
1. In the navigation pane, choose Functions.
1. Select the functions that you want to delete.
1. For Actions, choose Delete.
1. In the navigation pane, choose Layers.
1. Select the layer that you want to delete.
1. Choose Delete.

Delete any IAM users that you didn't create
1. Sign in to the AWS Management Console and open the [IAM console](https://console.aws.amazon.com/iam/)
1. In the navigation pane, choose Users, and then select the check box next to the user name that you want to delete, not the name or row itself.
1. At the top of the page, choose Delete user.
1. In the confirmation dialog box, wait for the last accessed information to load before you review the data. The dialog box shows when each of the selected users last accessed an AWS service. If you attempt to delete a user that has been active within the last 30 days, you must select an additional check box to confirm that you want to delete the active user. If you want to proceed, choose Yes, Delete.

Documentation on deleting other services can be found on the [terminate all my resources](https://aws.amazon.com/premiumsupport/knowledge-center/terminate-resources-account-closure/) page.

## Recovery
AWS has a published guide for [what do I do if I notice unauthorized activity in my AWS account?](https://aws.amazon.com/premiumsupport/knowledge-center/potential-account-compromise/)

## Preventative Actions
### Prowler IAM Scan 
`./prowler -g check122,check111,check110,check19,check18,check17,check16,check15,check11,check116,check12,check114,check115,check14,check13,check112,check119,extra71,extra7100,extra7123,extra7125,extra769,extra774`

### Enable MFA
For increased security, it's a best practice to configure MFA to help protect your AWS resources. You can enable [MFA for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html) or the [AWS account root user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html). Enabling MFA for the root user affects only the root user credentials. IAM users in the account are distinct identities with their own credentials, and each identity has its own MFA configuration.

### Verify your account information
AWS needs accurate account information in order to contact you and help resolve any account issues. Check that the information on your account is correct.
* The account name and email address.
* Your contact information, especially your phone number.
* The alternate contacts for your account.

### Use AWS Git projects to scan for evidence of unauthorized use
AWS offers Git projects that you can install to help you protect your account:
* [Git Secrets](https://github.com/awslabs/git-secrets) can scan merges, commits, and commit messages for secret information (that is, access keys). If Git Secrets detects prohibited regular expressions, it can reject those commits from being posted to public repositories.
* Use [AWS Step Functions and AWS Lambda to generate Amazon CloudWatch Events](https://aws.amazon.com/step-functions) from AWS Health or by AWS Trusted Advisor. If there is evidence that your access keys are exposed, the projects can help you to automatically detect, log, and mitigate the event.

### Avoid using the root user for day-to-day operations
The access key for your AWS account root user gives full access to all your AWS resources, including your billing information. You can't reduce the permissions associated with your AWS account root user access key. It's a best practice not to use the root user access unless absolutely necessary.

If you don't already have an access key for your AWS account root user, don't create one unless absolutely necessary. Instead, create an IAM user for yourself with administrative permissions. You can sign in to the AWS Management Console with your AWS account email address and password to create an IAM user.

### Overall Security Posture
Execute a [Self-Service Security Assessment](https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) against the environment to further identify other risks and potentially other public exposure not identified throughout this playbook.

## Lessons Learned
`This is a place to add items specific to your company that do not necessarilly need "fixing", but are important to know when executing this playbook in tandem with operational and business requirements.`

## Addressed Backlog Items
- As an Incident Responder I need a playbook on how to handle session tokens and access keys after an incident
- As an Incident Responder I need a method to scan and remove keys from public code repositories
- As an Incident Responder I need a clear decision maker on when to break an environment vs allowing continious exposure
## Current Backlog Items
