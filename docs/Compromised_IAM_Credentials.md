# Incident Response Playbook: Compromised IAM Credentials
This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided "as is" without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2021 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

## Points of Contact

Author: `Author Name` \
Approver: `Approver Name` \
Last Date Approved:

## Executive Summary
This playbook outlines the response process when you observe unauthorized activity within your AWS account or believe that an unauthorized party accessed your account.

## Potential Indicators of Compromise
- CloudWatch alarm for root user login
- New or unrecognized IAM users
- Unrecognized or unauthorized resources (e.g., EC2, Lambda)
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
Throughout the execution of the playbook, focus on the desired outcomes, taking notes to enhance incident response capabilities.

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

### Response Steps
The response steps follow the Incident Response Life Cycle from [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf).

1. [**PREPARATION**] Perform an Asset Inventory
2. [**PREPARATION**] Implement a training plan to identify and respond to exposed IAM credentials
3. [**PREPARATION**] Implement a communication strategy for incident response
4. [**DETECTION**] Identify Root Account Access (authorized and not)
5. [**DETECTION**] Identify New or unrecognized IAM users
6. [**DETECTION**] Identify Unrecognized or unauthorized resources (e.g., EC2, Lambda)
7. [**DETECTION**] Identify and find exposed secrets
8. [**DETECTION**] Identify Unusual billing increases
9. [**DETECTION**] Respond to notification from AWS or a third party that my AWS resources or account might be compromised
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

![Image](/images/nist_life_cycle.png)

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
This playbook references and integrates, where possible, with [Prowler](https://github.com/toniblyx/prowler), a command-line tool that helps you with AWS security assessment, auditing, hardening, and incident response.

It follows guidelines of the CIS Amazon Web Services Foundations Benchmark (49 checks) and has more than 100 additional checks, including those related to GDPR, HIPAA, PCI-DSS, ISO-27001, FFIEC, SOC2, and others.

This tool provides a quick snapshot of the current security state within a customer environment. As an alternative, [AWS Security Hub](https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) provides automated compliance scanning and can [integrate with Prowler](https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration).

### Asset Inventory
Identify all existing users and have an updated list of the purpose for each account:
1. Navigate to the [AWS Console](https://console.aws.amazon.com/).
1. Go to `Services` and select `IAM`.
1. In the left-hand menu, select `Credential report`.
1. Select `Download Report`.
1. Pay special attention to each IAM user's creation date and the `password last used` and `password last changed` columns.
    * **NOTE** If a user has multiple access keys, validate the purpose for each.

### Training
> ***What training is in place for analysts within the company to become familiar with AWS API (command-line environment), S3, RDS, and other AWS services?***

Opportunities here for Threat Detection and incident response include:
- [AWS RE:INFORCE](https://reinforce.awsevents.com/faq/)
- [Self-Service Security Assessment](https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)

Understanding, documenting, and socializing the answers to the following questions are critical to incident response:
- Which roles can make changes to services within your account?
- Which users have those roles assigned to them? Is the principle of least privilege followed, or do "super-admin" users exist?
- Has a Security Assessment been performed against your environment? Do you have a known baseline to detect "new" or "suspicious" things?

### Communication Technology
> ***What technology is used within the team/company to communicate issues? Is there anything automated?***

Understand, document, and socialize services such as the following that your users should expect to interact with during a security incident:
- Telephone
- E-mail
- AWS SES
- AWS SNS
- Slack
- Chime
- Other?

## Detection

### Root Account Access
Best practices recommend removing all access keys associated with the root account: `./prowler -c check_112`

### New or unrecognized IAM users
Review the IAM credential report from your [Asset Inventory](./Compromised_IAM_Credentials.md/#asset-inventory)  
Check if IAM users have two active access keys: `./prowler -c check_extra712`  
Ensure IAM policies that allow full \"*:*\" administrative privileges are not created: `./prowler -c check_122`  
Check if IAM Access Analyzer is enabled and its findings: `./prowler -c check_extra769`  

### Unrecognized or unauthorized resources (e.g., EC2, Lambda)
```bash
aws ec2 describe-instances
aws lambda list-functions
```

### Looking for Secrets
Potential secret found in EC2 instance User Data: `./prowler -c check_extra741`  
Potential secret found in Lambda function variables: `./prowler -c check_extra759`  
Potential secret found in ECS task definition variables: `./prowler -c check_extra768`  
Potential secret found in Autoscaling Configuration: `./prowler -c check_extra775`  

### Unusual billing increases
To view your AWS bill, open the [Bills](https://console.aws.amazon.com/billing/home#) pane of the Billing and Cost Management console, and then choose the month you want to view from the dropdown menu.

You can view the history of your AWS payments in the [Orders and invoices](https://console.aws.amazon.com/billing/home?/paymenthistory) pane of the Billing and Cost Management console.

You can also use [AWS Cost Anomaly Detection](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-ad.html) for dynamic monitoring and alerting.

Check your bill for the following:
* AWS services that you don't usually use.
* Resources in AWS regions that you don't usually use.
* A significant change in the size of your bill.

You can use this information to help you to stop, snapshot, delete, or terminate any resources you don't want running in your environments.

### Notification that my AWS resources or account might be compromised
If you received a notification from AWS about your account, sign in to the AWS Support Center and then respond to the message.

If you can't sign in to your account, use the Contact Us form to request help from AWS Support.

Create a new AWS Support case in the AWS Support Center if you have any questions or concerns.
    * **NOTE**: Don't include sensitive information in your correspondence, such as AWS access keys, passwords, or credit card information.
Use AWS Git projects to scan for evidence of unauthorized use.

### Identify any potentially unauthorized IAM user credentials
1. Open the IAM console.
1. Choose Users in the navigation pane.
1. Choose each IAM user from the list, then check under Permissions policies for a policy named `AWSExposedCredentialPolicy_DO_NOT_REMOVE`. 1. If the user has this attached policy, you must rotate the access keys for the user.

## Escalation Procedures
Understand, document, and socialize the following:
- Who monitors the logs/alerts, receives them, and acts upon each?
- Who gets notified when a security incident is declared?
- When do public relations and legal get involved in the incident response process?
- When would you reach out to AWS Support for help?

## Analysis
Best practices recommend exporting logs to a security incident event management (SIEM) solution (such as Splunk, ELK stack, etc.) to aid in viewing and analyzing various records for a complete attack timeline analysis.

### CloudTrail
Look for unusual login activity:
1. Navigate to your [CloudTrail Dashboard](https://console.aws.amazon.com/cloudtrail).
1. In the left-hand margin, select `Event History`.
1. Change from `Read-Only` to `Event Name` in the dropdown.
1. In the search field, enter `ConsoleLogin` or `AssumeRole` or `GetFederationToken` or `GetCredentialReport` or `GenerateCredentialReport` and review the available events for any suspicious activity.
    * **NOTE** `userIdentity` will show up as `"type": "Root"` for the root user of the account or `"type": "IAMUser"` for IAM users.

Locate the IAM access key ID and user name used to launch a suspicious EC2 instance:
1. Open the CloudTrail console and choose `Event history` from the navigation pane.
1. Select the `Lookup attributes` dropdown menu, then choose `Resource name`.
1. In the Enter resource name field, paste the EC2 instance ID, and then hit enter on your device.
1. Expand the Event name for RunInstances.
1. Copy the AWS access key, and note the User name.

Review CloudTrail event history for activity by the compromised access key:
1. Open the CloudTrail console and choose `Event history` from the navigation pane.
1. Select the `Lookup attributes` dropdown menu, then choose `AWS access key`.
1. In the `Enter an AWS access key field`, enter the compromised IAM access key ID.
1. Expand the `Event name` for the API call `RunInstances`.
    * **NOTE**: You can only view event history for the last 90 days unless you've previously configured a trail that saves into an S3 bucket.

### VPC Flow Logs
VPC Flow Logs is a feature that enables you to capture information about the IP traffic going to and from network interfaces inside your VPCs. Use Flow Logs to discover IP addresses within CloudTrail to determine the different types of external connections to any public resources.

For further information and steps, including querying with Athena, please refer to the [AWS Documentation for VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html). In addition, best practices recommend Athena's analysis be included in a separate playbook and linked to other relevant items.

### Endpoint / Host Based
* **Identify the Creation time for a given instance name using `jq`**: `aws ec2 describe-instances --query 'Reservations[].Instances[].{ip: PublicIpAddress, tm: LaunchTime}' --filters 'Name=tag:Name,Values= myInstanceName' | jq 'sort_by(.tm) | reverse | .[0]'`

* **Identify Last Modified time for lambda functions**: `aws lambda list-functions | grep "LastModified"`

## Containment
1. Disable the IAM user, create a backup IAM access key, and then disable the compromised access key:
    1. Open the [IAM console](https://console.aws.amazon.com/iam/) and paste the IAM access key ID in the Search IAM bar.
    1. Choose the user name and then choose the Security credentials tab.
    1. In the Console password field, choose Manage.
        * **NOTE**: If the AWS Management Console password is set to Disabled, you can skip this step.
    1. In Console access, choose Disable, and then select Apply.
        1. Important: Users whose accounts are disabled can't access the AWS Management Console. However, users can still access AWS services using API calls if they have active access keys.
    1. For the compromised IAM access key, choose `Make inactive`.
1. Disable the application IAM key, create a backup IAM access key, and then disable the compromised access key:
    1. First, create a second key. Then, modify your application to use the new key.
    1. Disable (but do not delete) the first key.
    1. If there are any problems with your application, reactivate the key temporarily. When your application is fully functional, and the first key is disabled, only then is it safe to delete the first key.
1. Rotate and delete all inactive/compromised AWS access keys:
    * **!!Ensure you keep a record of all deleted access keys to continue searching for them in CloudTrail!!**

## Eradication
### Review the findings from [Review CloudTrail event history for activity by the compromised access key](./#cloudtrail)
Remove any resources created by the compromised key(s). Check all AWS regions, even regions where you never launched AWS resources.  
    * **Important**: If you need to keep any resources for investigation, consider backing them up. For example, if you have a regulatory, compliance, or legal need to retain an EC2 instance, take an EBS snapshot before terminating the instance.

### Review the [Avoiding unexpected charges](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/checklistforunwantedcharges.html)
Check for and remove any unrecognized services that are running within your account. Pay special attention to the following resources:
* EC2 instances and AMIs, including instances in the stopped state
* EBS volumes and snapshots
* AWS Lambda functions and layers

To delete Lambda functions and layers, do the following:
1. Open the Lambda console.
1. In the navigation pane, choose `Functions`.
1. Select the functions that you want to delete.
1. For Actions, choose `Delete`.
1. In the navigation pane, choose Layers.
1. Select the layer that you want to delete.
1. Choose `Delete`.

Delete any IAM users that you didn't create:
1. Sign in to the AWS Management Console and open the [IAM console](https://console.aws.amazon.com/iam/)
1. In the navigation pane, choose Users, and then select the check box next to the user name you want to delete, not the name or row itself.
1. At the top of the page, choose Delete user.
1. In the confirmation dialog box, wait for the last accessed information to load before you review the data. The dialog box shows when each selected user last accessed an AWS service. If you attempt to delete a user that has been active within the last 30 days, you must select an additional check box to confirm that you want to delete the active user. If you wish to proceed, choose Yes, Delete.

Documentation on deleting other services can be found on the [terminate all my resources](https://aws.amazon.com/premiumsupport/knowledge-center/terminate-resources-account-closure/) page.

## Recovery
AWS has a published guide for [what do I do if I notice unauthorized activity in my AWS account?](https://aws.amazon.com/premiumsupport/knowledge-center/potential-account-compromise/)

## Preventative Actions
### Prowler IAM Scan
`./prowler -g check122,check111,check110,check19,check18,check17,check16,check15,check11,check116,check12,check114,check115,check14,check13,check112,check119,extra71,extra7100,extra7123,extra7125,extra769,extra774`

### Enable MFA
For increased security, it's a best practice to configure MFA to help protect your AWS resources. You can enable [MFA for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html) or the [AWS account root user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html). Enabling MFA for the root user affects only the root user credentials. IAM users in the account are distinct identities with their credentials, and each identity has its own MFA configuration.

### Verify your account information
AWS needs accurate account information in order to contact you and help resolve any account issues. Check that the following information on your account is correct:
* The account name and e-mail address.
* Your contact information, especially your phone number.
* The alternate contacts for your account.

### Use AWS Git projects to scan for evidence of unauthorized use
AWS offers Git projects that you can install to help you protect your account:
* [Git Secrets](https://github.com/awslabs/git-secrets) can scan merges, commits, and commit messages for secret information such as AWS access key pairs. If Git Secrets detects prohibited regular expressions, it can reject those commits from being posted to public repositories.
* Use [AWS Step Functions and AWS Lambda to generate Amazon CloudWatch Events](https://aws.amazon.com/step-functions) from AWS Health or by AWS Trusted Advisor. If there is evidence that your access keys are exposed, the projects can help you to automatically detect, log, and mitigate the event.

### Avoid using the root user for day-to-day operations
The access key for your AWS account root user gives full access to all your AWS resources, including your billing information. You can't reduce the permissions associated with your AWS account root user's access key. It's a best practice not to use the root user via the console or access keys unless necessary.

If you don't already have an access key for your AWS account root user, do not create one unless absolutely necessary. Instead, create an IAM user that can temporarily assume a role with administrative permissions. You can sign in to the AWS Management Console with your AWS account number, username, e-mail address, and password you've created and then assume the administrative role only as required.

### Overall Security Posture
Execute a [Self-Service Security Assessment](https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) against the environment to further identify other risks and potentially additional public exposure not identified throughout this playbook.

## Lessons Learned
> This is a place to add items specific to your company that do not necessarily need "fixing" but are important to know when executing this playbook in tandem with operational and business requirements.

## Current Backlog Items
> This is a place to track action items from closed security incidents that are still in progress.

## Addressed Backlog Items
> This is a place to track action items from closed security incidents that are completed. For example:
- As an Incident Responder, I need a playbook on how to handle session tokens and access keys after an incident
- As an Incident Responder, I need a method to scan and remove keys from public code repositories
- As an Incident Responder, I need a clear decision-maker on when to break an environment vs. allowing continuous exposure

