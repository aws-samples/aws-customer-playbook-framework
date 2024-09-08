# Security Playbook for Compromised AWS Account Credentials

## Introduction

As part of our ongoing commitment to customers, AWS is providing this
security incident response playbook that describes the steps needed to
detect and respond to compromised credentials within your AWS
account(s). The purpose of this document is to provide prescriptive
guidance on the actions to take once you suspect a security event has
taken place.

![Image](/images/nist_life_cycle.png)

*Aspects of AWS incident response*

## Preparation

In order to quickly and effectively execute incident response activities, it is crucial to prepare the people, processes, and technology within your organization. Review the [*AWS Security Incident Response Guide*](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/preparation.html), and implement the steps required to help ensure preparedness of all three domains.

## How to Engage AWS Support for Assistance

It is important that you notify AWS as soon as you suspect a compromised credential(s) within your AWS account or Organization. Here are the steps to engage AWS Support:

### Open an AWS Support Case

-   Login to your AWS account:
    -   This is the first AWS account that was impacted by the security event, to validate AWS account ownership.
    -   Note: If you are unable to access account, use [this form](https://support.aws.amazon.com/#/contacts/aws-account-support/) to submit a support request.
-   Select “*Services*”, “*Support Center*”, “*Create Case*”.
-   Select issue type “*Account and Billing”*.
-   Select the impacted Service and Category:
    -   Example:
        -   Service: Account
        -   Category: Security
-   Choose a Severity:
    -   Enterprise Support or On-Ramp Customers: *Critical Business Risk Question*.
    -   Business Support Customers: *Urgent Business Risk Question*.
-   Describe your question or issue:
    -   Provide a detailed description of the security issue you’re experiencing, the impacted resources, and the business impact.
        -   Time the security event was first recognized.
        -   Summary of the event timeline to this point.
        -   Artifacts you’ve collected (screenshots, log files).
        -   ARN of IAM Users or Roles that you suspect are compromised.
        -   Description of affected resources, and the business impact.
        -   Level of engagement in your organization (ex: “This security event has the visibility of the CEO and Board of Directors”).
        -   Optional: Add additional case contacts.
-   Select “*Contact Us*”.
    -   Preferred contact language (may be subject to availability)
    -   Preferred contact method: Web, Phone, or Chat (recommended)
    -   Optional: Additional case contacts
    -   *Click “Submit”*

-   **Escalations**: Please notify your AWS account team as soon as possible, so they may engage the necessary resources and escalate as needed.

**Note:** It is very important to verify your 'Alternate Security Contact' is defined for each AWS account. For more details, please refer to [this
article](https://aws.amazon.com/blogs/security/update-the-alternate-security-contact-across-your-aws-accounts-for-timely-security-notifications/).

## Detection

There are multiple ways to detect compromised credentials within your AWS environment. Listed are some ways to detect potential Indicators of Compromise (IoC). For a detailed list of IoC’s please refer to [appendix A.](#appendix-a-reviewing-logs) **Note**: Document the details of each suspected IoC for further analysis.

1.  Review Amazon [GuardDuty findings](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_findings.html). The findings below are related to suspicious or anomalous activity
    by IAM entities. Review the [finding details](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-iam.html), and if the activity is unexpected, this could indicate a compromised credential.
    1.  CredentialAccess:IAMUser/AnomalousBehavior
    2.  DefenseEvasion:IAMUser/AnomalousBehavior
    3.  Discovery:IAMUser/AnomalousBehavior
    4.  Exfiltration:IAMUser/AnomalousBehavior
    5.  Impact:IAMUser/AnomalousBehavior
    6.  InitialAccess:IAMUser/AnomalousBehavior
    7.  PenTest:IAMUser/KaliLinux
    8.  PenTest:IAMUser/ParrotLinux
    9.  PenTest:IAMUser/PentooLinux
    10. Persistence:IAMUser/AnomalousBehavior
    11. Policy:IAMUser/RootCredentialUsage
    12. PrivilegeEscalation:IAMUser/AnomalousBehavior
    13. Recon:IAMUser/MaliciousIPCaller
    14. Recon:IAMUser/MaliciousIPCaller.Custom
    15. Recon:IAMUser/TorIPCaller
    16. Stealth:IAMUser/CloudTrailLoggingDisabled
    17. Stealth:IAMUser/PasswordPolicyChange
    18. UnauthorizedAccess:IAMUser/ConsoleLoginSuccess.B
    19. UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration
    20. UnauthorizedAccess:IAMUser/MaliciousIPCaller
    21. UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom
    22. UnauthorizedAccess:IAMUser/TorIPCaller

2.  Review AWS Billing for unusual spikes which can be a sign of account and credential(s) compromise by following these steps:

    1.  Sign into the AWS Management Console and open the [Billing console](https://console.aws.amazon.com/billing/).
    2.  Choose “*Bills”* to see details about your current charges.
    3.  Choose “*Payments”* to see your historical payment transactions.
    4.  Choose “*AWS Cost and Usage Reports”* to see reports that break down your costs.

3.  Download the IAM credentials report by going to the IAM console and choose the “*Credential report”* on the left under “*Access reports”* then review the following:

    1.  identify unusual IAM user creation by looking at creation date and the password last used/changed columns.
    2.  Check if any IAM users have two or more access keys.
    3.  Check if any IAM users have *AWSCompromisedKeyQuarantineV2* attached. If so, rotate its access keys.

4.  Review IAM Roles within the AWS account, to identify any unfamiliar roles that have been created or accessed

    1.  Within the AWS Console, select “*Service*s”, “*IAM*”, and “*Roles*”
    2.  Review any unfamiliar “*Role names*”.
    3.  Click on the Role name, and review the listed details:
        1.  Role Creation Date
        2.  ARN
        3.  Last Activity

    4.  Click “*Permissions*” to review IAM Policies that are attached to the role.
    5.  Click “*Trust Relationships*” to view entities that can assume the role.
    6.  Click “*Access Advisor*” to show which services have been accessed by the role, and the “*Last Accessed Date*”.

5.  Detect any unrecognized or unauthorized resources within your AWS account such as:
    1.  EC2 instances by running the following command in the AWS CLI terminal “*aws ec2 describe-instances*” and validate the results. Look for the creation time for each instance using the *--query 'Reservations\[\].Instances\[\].{ip: PublicIpAddress, tm: LaunchTime}' --filters 'Name=tag:Name,Values= myInstanceName' | jq 'sort\_by(.tm) | reverse | .\[0\]'* filter.
    2.  Lambda functions by running the following command in the AWS CLI terminal “*aws lambda list-functions*” and validate the last modified time by using the “| *grep “LastModified*”” filter.

6.  Review any security notifications from AWS about your account by signing into [AWS Support Center](https://support.console.aws.amazon.com/support/home#/) then reading and responding to the message(s).

7.  Review findings by following these steps:
    1.  Open the [IAM console](https://console.aws.amazon.com/iam/).
    2.  Choose “*Access analyzer”* from the left column under Access reports.
    3.  Under “*Active findings”* review findings to identify resources in your organization, such as S3 buckets, IAM roles, or Lambda functions that are shared with external identities.

## Analysis

Once you have identified any suspicious resources or activity that may indicate compromise, perform further analysis in your SIEM, or log analysis tools. AWS has various security services tools to aid in analyzing security events. Some of these tools include:

1.  **Amazon Guardduty** - Amazon GuardDuty is a threat detection service that continuously monitors for malicious activity and unauthorized behavior to protect your AWS accounts, Amazon Elastic Compute Cloud (EC2) workloads, container applications, Amazon Aurora databases, and data stored in Amazon Simple Storage Service (S3).

2.  **AWS Security Hub** - AWS Security Hub is a cloud security posture management (CSPM) service that performs automated, continuous security best practice checks against your AWS resources to help you identify misconfigurations, and aggregates your security alerts (i.e. findings) in a standardized format so that you can more easily enrich, investigate, and remediate them.

3.  **Amazon Detective** - Amazon Detective makes it easier to analyze, investigate, and quickly identify the root cause of potential security issues or suspicious activities.

You can also search your AWS CloudTrail event history, by following these steps to analyze and gather evidence:

1.  Analyze AWS CloudTrail logs for the following:
    1.  Any unusual activities associated with logins:
        1.  Go to the [CloudTrail Dashboard](https://console.aws.amazon.com/cloudtrail).
        2.  In the left-hand side, pick Event History.
        3.  In the dropdown menu change Read-Only to Event Name.
        4.  Review the available events for any suspicious activity by searching for the following terms via the search box: “*ConsoleLogin*”, “*AssumeRole*”, “*GetFederationToken*”,
            “*GetCredentialReport*”, “*GenerateCredentialReport*”. Also it is important to note that “*userIdentity*” should show up as "*type*": "*Root*" for the root user or "*type*": "*IAMUser*" for any local IAM users on the account.
    2.  Locate any IAM access key ID and user name used to launch a suspicious Amazon EC2 instance:
        1.  Open the AWS CloudTrail console and choose “*Event history”* from the navigation pane.
        2.  Select the “*Lookup attributes”* dropdown menu, then choose “*Resource name”*.
        3.  In the Enter resource name field, paste the EC2 instance ID, and then hit enter on your device.
        4.  Expand the Event name for *RunInstances*.
        5.  Copy the AWS access key, and note the User name.
    3.  Review AWS CloudTrail event history for activity by the compromised access key:
        1.  Open the CloudTrail console and choose “*Event > history”* from the navigation pane.
        2.  Select the “*Lookup attributes”* dropdown menu, then > choose “*AWS access key”*.
        3.  In the “*Enter an AWS access key field”*, enter the > compromised IAM access key ID.
        4.  Expand the Event name for the API call *RunInstances*.
    3.  If you are accessing AWS via 3rd Party Identity Provider, be sure to audit the access logs and follow that vendors guidance on responding to an event and securing your environment.

1.  Analyze IAM Access Advisor compromised credentials last accessed information to see what was the last service it access by following these steps:
    1.  Go to the [IAM console](https://console.aws.amazon.com/iam).
    2.  Go to users or roles, click on the name of the compromised principal, choose the “*access advisor”* tab and look at which resources it last accessed.

## Containment

After analyzing and gathering more information about the compromised credential(s) and any other affected resources, it’s time to control and suppress the security event.

1.  Disable the IAM user(s), create a backup IAM access key, and then disable the compromised access key by following these steps:
    1.  Open the [IAM console](https://console.aws.amazon.com/iam/) and paste the IAM access key ID in the Search IAM bar.
    2.  Choose the user name and then choose the “*Security credentials*” tab.
    3.  In the Console password field, choose “*Manage*”.
    4.  In Console access, choose “*Disable*”, and then select Apply.
    5.  For the compromised IAM access key, choose “*Make inactive*”.

2.  Rotate access keys by following these steps:
    1.  First, create a second key by going to the [IAM console](https://console.aws.amazon.com/iam/).
    2.  In the navigation pane, choose “*Users”*.
    3.  Choose the name of the intended user, and then choose the “*Security credentials”* tab.
    4.  In the “*Access keys”* section, choose “*Create access key”*. On the “*Access key best practices* *& alternatives”* page, choose “*Other”*, then choose “*Next”.*
    5.  Then, modify your application to use the new key.
    6.  Disable (but do not delete) the first key.
    7.  If there are any problems with your application, reactivate the key temporarily. When your application is fully functional, and the first key is disabled, only then is it safe to delete the first key. Make sure you keep a record of all deleted access keys to continue searching for them in AWS CloudTrail logs.

3.  Revoke the IAM role(s) active sessions by following these steps:
    1.  Open the [IAM console](https://console.aws.amazon.com/iam/) and go to role and click on the IAM role you want to revoke active sessions for.
    2.  Click on the IAM role name and go to “*revoke sessions”* tab.
    3.  Click on “*revoke active sessions*” button and confirm the step.

4.  Isolate affected resources by following these steps:
    1.  For Amazon EC2 instances, go to the EC2 instance console, check the box next to the EC2 instance you want to isolate, click on *actions*, click on *security*, then click on *change security groups*. Detach any current security groups and attach an isolated security group that blocks inbound and outbound communication to the EC2.
    2.  For Amazon S3 buckets, use [bucket policies](https://docs.aws.amazon.com/AmazonS3/latest/userguide/add-bucket-policy.html) to block any suspicious IP addresses from accessing the S3 buckets.

5.  Revoke Identity Center sessions:
    - With Identity Center, there are two sessions that to be concerned about which are the access portal session, and role/application sessions:
    1.  Access portal session:
        1.  Disable the user in Identity Center:
            1.  Navigate to the Identity Center console and select ‘Users’
            2.  Choose the username of the user being disabled
            3.  In the General information box for the user, click ‘Disable > user access’
        2.  Revoke any active sessions:
            1.  On the user’s page in Identity Center, select ‘Active > sessions’ tab
            2.  Select any listed sessions and then click ‘Delete session’

    2.  Revoke role sessions:
        - Use one of the two options proposed:
        1.  Identify the permission set(s) being used by the user.
            1.  From the Identity Center console, click on Permission Sets
            2.  Select the name of the permission set.
            3.  Scroll down to ‘Inline Policy’ and click on the Edit button
            4.  Add the following policy:

                ```json
                {
                "Version": "2012-10-17",
                "Statement": [
                    {
                    "Effect": "Deny",
                    "Action": "*",
                    "Resource": "*",
                    "Condition": {
                        "StringEquals": {
                        "identitystore:userId": "example"
                        },
                        "DateLessThan": {
                            "aws:TokenIssueTime": "2023-09-26T15:00:00.000Z"
                            }
                        }
                    }
                ]
                }
                ```

            5. For this policy, update ‘example’ to the user’s Identity Center User ID. The User ID can be found in the ‘General information’ box of the user’s
    User page. The value for aws:TokenIssueTime should equal the time in which you apply this policy.

        1. Alternatively, use a Service Control Policy to deny all actions for a specific user.
            1.  From the AWS management console, navigate to AWS Organizations.
            2.  Click on ‘Policies’ then select ‘Service Control Policies’.
            3.  Click ‘Create policy’ and add the following policy:

                ```json
                {
                    "Version": "2012-10-17",
                    "Statement": [
                        {
                            "Effect": "Deny",
                            "Action": [
                                "*"
                            ],
                            "Resource": "*",
                            "Condition": {
                                "StringEquals": {
                                    "identitystore:userId": "Add user ID here"
                                }
                            }
                        }
                    ]
                }
                ```

            4.  Replace ‘Add user ID here’ with the user’s Identity Center User ID found in the ‘General information’ box of the user’s User page.
            5.  Attach this policy to the Root or a specific Organizational Unit where the user resides:
                1.  Go back to the ‘Service Control Policies’ page.
                2.  Select the newly created policy.
                3.  Click on ‘Attach’.
                4.  Choose the target Root or Organizational Unit to apply this policy.

6.  Application sessions

Application sessions are created when a user accesses a third-party application or AWS service directly from the access portal.

To revoke application sessions, review the documentation of the application that was accessed.

7.  Compromised Identity Center

If your Identity provider is compromised, you should block access to Identity Center from a compromised identity provider (i.e. external
SAML-based Identity Provider or Active Directory) by changing the identity source to ‘Identity Source directory’.

To change your identity source:
    1. Navigate to the Identity Center console
    2. Select ‘Settings’
    3. On the Settings page, click on the ‘Identity source’ tab
    4. Click on the ‘Actions’ button and then select ‘Change identity
source’
    5. Select ‘Identity Center directory’ on the ‘Choose identity source’
page
    6. Click on the ‘Next’ button
    7. Read the information contained in the ‘Review and confirm’ box,
    8. Type ‘ACCEPT’ in the appropriate field, and click on the ‘Change
identity source’ button

Please note, If you are accessing AWS via 3rd Party Identity Provider, be sure to audit the access logs and follow that vendors guidance on responding to an event and securing your environment.

Also, If you are changing from Active Directory to the Identity Center directory, all users and groups will be deleted. Permission sets will not be deleted, but permission set assignments will be deleted. If you are changing from an external SAML-based identity provider, all users and groups will remain populated in Identity Center as will the
permission set assignments.

## Eradication

Once you are done with containing the security event, it’s time to work on removing and cleaning the cause of the security event.

1.  Remove any resources created by the compromised key(s) which were detected in the “Analysis phase step 1”. Check all AWS regions, even regions where you do not launch AWS resources in. If you need to keep a resource for investigation, consider backing them up.

2.  Check for and [remove](https://repost.aws/knowledge-center/terminate-resources-account-closure) any unrecognized services that are running within your account. Pay
    special attention to the following resources:
    1.  EC2 instances and AMIs, including instances in the stopped state.
    2.  EBS volumes and snapshots.
    3.  AWS Lambda functions and layers.

3.  Remove unnecessary permissions from logging S3 buckets or log aggregation resources identified in “Detection phase step 6” that could be used to avoid detection. This lets you identify unintended access to your resources and data.

4.  Remove any exposed data that is not needed for operations.

5.  Perform security vulnerability scans on the publicly facing resources. You can use tools like [Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/user/scanning-resources.html) to scan OS and 3rd party software application for vulnerabilities.

## Recovery

Once you are done with eradicating the security event causes. It’s time to restore the affected resources into a good-known state.

1.  Restore necessary data from known clean backups that predate the event:
    1.  [Restoring from an Amazon EBS snapshot or an AMI](https://docs.aws.amazon.com/prescriptive-guidance/latest/backup-recovery/restore.html).
    2.  [Restoring from a DB snapshot](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_RestoreFromSnapshot.html) Amazon RDS.
    3.  [Restoring previous versions](https://docs.aws.amazon.com/AmazonS3/latest/userguide/RestoringPreviousVersions.html) Amazon S3 object versions.

2.  If necessary, rebuild systems from scratch, including redeploying from trusted source using automation, sometime in a new AWS account.

3.  If access to Multi Factor Authentication “MFA” is lost and you don’t have another MFA device registered to you’re root account , please follow these steps:
    1.  Sign in to the [AWS Management Console](https://console.aws.amazon.com/) as the account owner by choosing Root user and entering your AWS account email address. On the next page, enter your password.
    2.  On the Additional verification required page, select an MFA method to authenticate with and choose Next.
    3.  Depending on the type of MFA you are using, you should see a different page, but the “*Troubleshoot MFA”* option functions the same. On the “*Additional verification required”* page or “*Multi-factor authentication”* page, choose “*Troubleshoot MFA”*.
    4.  If required, type your password again and choose *Sign in*.
    5.  On the “*Troubleshoot your authentication device”* page, in the “*Sign in using alternative factors of authentication”* section, choose “*Sign in using alternative
        factors”*.
    6.  On the “*Sign in using alternative factor"s of authentication”* page, authenticate your account by verifying the email address, choose “*Send verification email”*
    7.  Check the email that is associated with your AWS account for a message from Amazon Web Services (recover-mfa-no-reply@verify.signin.aws). Follow the directions in the email. If you don't see the email in your account, check your spam folder, or return to your browser and choose “*Resend the email*”.

4.  After you verify your email address, you can continue authenticating your account. To verify your primary contact phone number, choose Call me now.

5.  Answer the call from AWS and, when prompted, enter the 6-digit number from the AWS website on your phone keypad.
    * If you don't receive a call from AWS, choose Sign in to sign in to the console again and start over. Or see [Lost or unusable Multi-Factor Authentication (MFA) device](https://support.aws.amazon.com/#/contacts/aws-mfa-support) to contact support for help.

6.  After you verify your phone number, you can sign in to your account by choosing Sign in to the console.

7.  The next step varies depending on the type of MFA you are using:
    1.  For a virtual MFA device, remove the account from your device. Then go to the [AWS Security Credentials](https://console.aws.amazon.com/iam/home?#security_credential) page and delete the old MFA virtual device entity before you create> a new one.
    2.  For a FIDO security key, go to the [AWS Security Credentials](https://console.aws.amazon.com/iam/home?#security_credential) page and deactivate the old FIDO security key before enabling a new one.
    3.  For a hardware TOTP token, contact the third-party provider for help fixing or replacing the device. You can continue to sign in using alternative factors of authentication until you receive your new device. After you have the new hardware MFA device, go to the [AWS Security Credentials](https://console.aws.amazon.com/iam/home?#security_credential) page and delete the old MFA hardware device entity before you create a new one.

8.  Confirm that IAM principals have appropriate access and permissions following the security event.

9.  Address vulnerabilities and install the necessary patches using [AWS SSM patch manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager.html)
    or any other 3rd party tool you use.

10.  Replace any compromised/damaged files with clean versions from backup.

## Post Incident Activity

After you have successfully recovered from the security incident, you should conduct the exercises outlined in [*AWS Security Incident Response Guide, Post-Incident activity*](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/establish-framework-for-learning.html). This will help you document lessons learned from the incident, measure and improve the effectiveness of your incident response capabilities, and implement additional controls to prevent a similar incident from recurring.

## Conclusion

In this playbook, we have described the initial steps to take when a compromised credential security event occurs within your AWS account(s). This includes engaging AWS support, detecting a compromise, analysis of events within your account(s), containing the security event, eradicating the threat, recovering your account(s) to a “known-good”
operational state, and post-incident activities including lessons learned. As a next step, please review the following AWS resources, to help improve your Incident response capabilities:

1.  [AWS Security Incident Response Guide](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/aws-security-incident-response-guide.html)
2.  [AWS Customer Playbook Framework for Compromised IAM Credentials](https://github.com/aws-samples/aws-customer-playbook-framework/blob/main/docs/Compromised_IAM_Credentials.md)
3.  [AWS Security Best Practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

4.  [AWS Well Architected Framework – Security Pillar](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html)

## Appendix A – Reviewing Logs

**CloudTrail Log Structure**

When searching through CloudTrail logs, it is important to understand the various fields contained within a CloudTrail event record. For a full list, please refer to the [official CloudTrail documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-record-contents.html).

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>eventTime</td>
<td>The date and time the request was completed, in coordinated universal time (UTC).</td>
</tr>
<tr class="even">
<td>eventName</td>
<td>The requested action, which is one of the actions in the API for that service</td>
</tr>
<tr class="odd">
<td>eventSource</td>
<td>The service that the request was made to. This name is typically a short form of the service name without spaces plus .amazonaws.com.</td>
</tr>
<tr class="even">
<td>userIdentity</td>
<td>Information about the IAM identity that made a request. For more information, see <a href="https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html"><u>CloudTrail userIdentity element</u></a>.</td>
</tr>
<tr class="odd">
<td>resources</td>
<td><p>A list of resources accessed in the event. The field can contain the following information:</p>
<ul>
<li><p>Resource ARNs</p></li>
<li><p>Account ID of the resource owner</p></li>
<li><p>Resource type identifier in the format: AWS::aws-service-name::data-type-name</p></li>
</ul></td>
</tr>
<tr class="even">
<td>awsRegion</td>
<td>The AWS Region that the request was made to, such as us-east-2</td>
</tr>
<tr class="odd">
<td>sourceIPAddress</td>
<td>The IP address that the request was made from. For actions that originate from the service console, the address reported is for the underlying customer resource, not the console web server.</td>
</tr>
</tbody>
</table>

**Events**

There are a number of actions threat actors will perform after compromising account credentials. While it is impractical to list every possible action, here are some patterns to search for during your investigation.

**Note:** The API actions below do not necessarily indicate a security incident has occurred. Evaluate all logged activity within the context of your investigation.

**Changes to SAML / OIDC identity provider configuration**

Threat actors may create or modify SAML/OIDC identity provider configurations to avoid detection and maintain persistence within your cloud environment.

<table>
<colgroup>
<col style="width: 32%" />
<col style="width: 67%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>API Action</strong></th>
<th><strong>Description</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>iam:CreateSAMLProvider</td>
<td>Creates an IAM resource that describes an identity provider (IdP) that supports SAML 2.0</td>
</tr>
<tr class="even">
<td>iam:DeleteSAMLProvider</td>
<td>Deletes a SAML provider resource in IAM.</td>
</tr>
<tr class="odd">
<td>iam:UpdateSAMLProvider</td>
<td>Updates the metadata document for an existing SAML provider</td>
</tr>
<tr class="even">
<td>iam:CreateOpenIDConnectProvider</td>
<td>Creates an IAM entity to describe an identity provider (IdP) that supports OpenID Connect (OIDC).</td>
</tr>
<tr class="odd">
<td>iam:AddClientIDToOpenIDConnectProvider</td>
<td>Adds a new client ID (also known as audience) to the list of client IDs already registered for the specified IAM OpenID Connect (OIDC) provider resource</td>
</tr>
<tr class="even">
<td>iam:DeleteOpenIDConnectProvider</td>
<td>Deletes an OpenID Connect identity provider (IdP) resource object in IAM</td>
</tr>
<tr class="odd">
<td>iam:RemoveClientIDFromOpenIDConnectProvider</td>
<td>Removes the specified client ID (also known as audience) from the list of client IDs registered for the specified IAM OpenID Connect (OIDC) provider resource object</td>
</tr>
<tr class="even">
<td>iam:UpdateOpenIDConnectProviderThumbprint</td>
<td>Replaces the existing list of server certificate thumbprints associated with an OpenID Connect (OIDC) provider resource object with a new list of thumbprints</td>
</tr>
</tbody>
</table>

**Changes to IAM Configuration**

Threat actors may create or modify IAM principals, credentials, or permissions to avoid detection or maintain persistence in your cloud environment.

<table>
<colgroup>
<col style="width: 28%" />
<col style="width: 71%" />
</colgroup>
<thead>
<tr class="header">
<th>API Action</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>iam:ChangePassword</td>
<td>Changes the password of the IAM user who is calling this operation</td>
</tr>
<tr class="even">
<td>iam:CreateUser</td>
<td>Creates a new IAM user for your AWS account</td>
</tr>
<tr class="odd">
<td>iam:CreateRole</td>
<td>Creates a new role for your AWS account</td>
</tr>
<tr class="even">
<td>iam:CreateGroup</td>
<td>Creates a new group</td>
</tr>
<tr class="odd">
<td>iam:AttachUserPolicy</td>
<td>Attaches the specified managed policy to the specified user</td>
</tr>
<tr class="even">
<td>iam:AttachRolePolicy</td>
<td>Attaches the specified managed policy to the specified IAM role</td>
</tr>
<tr class="odd">
<td>iam:AttachGroupPolicy</td>
<td>Attaches the specified managed policy to the specified IAM group</td>
</tr>
<tr class="even">
<td>iam:ListAccessKeys</td>
<td>Returns information about the access key IDs associated with the specified IAM user</td>
</tr>
<tr class="odd">
<td>iam:CreatePolicyVersion</td>
<td>Creates a new version of the specified managed policy</td>
</tr>
<tr class="even">
<td>iam:UpdateLoginProfile</td>
<td>Changes the password for the specified IAM user</td>
</tr>
<tr class="odd">
<td>iam:CreateAccessKey</td>
<td>Creates a new AWS secret access key and corresponding AWS access key ID for the specified user</td>
</tr>
<tr class="even">
<td>iam:UpdateAccessKey</td>
<td>Changes the status of the specified access key from Active to Inactive, or vice versa.</td>
</tr>
<tr class="odd">
<td>iam:DeactivateMFADevice</td>
<td>Deactivates the specified MFA device and removes it from association with the username for which it was originally enabled</td>
</tr>
</tbody>
</table>

**Changes to Logging & Monitoring Configurations**

Threat actors may disable resource monitoring or delete logs to avoid detection or deter incident investigations.

<table>
<colgroup>
<col style="width: 46%" />
<col style="width: 53%" />
</colgroup>
<thead>
<tr class="header">
<th>cloudtrail:DeleteTrail</th>
<th>Deletes a trail – disabling the delivery of CloudTrail events to an Amazon S3 bucket, CloudWatch Logs, or Amazon EventBridge</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>cloudtrail:StopLogging</td>
<td>Suspends the recording of AWS API calls and log file delivery for the specified trail</td>
</tr>
<tr class="even">
<td>cloudtrail:UpdateTrail</td>
<td>Updates the settings that specify delivery of log files</td>
</tr>
<tr class="odd">
<td>cloudtrail:DeleteEventDataStore</td>
<td>Deletes a <a href="https://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store.html"><u>CloudTrail Lake Event Date Store</u></a></td>
</tr>
<tr class="even">
<td>GuardDuty:DeleteDetector</td>
<td>Deletes an Amazon GuardDuty detector and disables GuardDuty in a particular region</td>
</tr>
<tr class="odd">
<td>GuardDuty:DeletePublishingDestination</td>
<td>Deletes the publishing destination, where findings are exported to</td>
</tr>
<tr class="even">
<td>accessanalyzer:DeleteAnalyzer</td>
<td>Deletes the specified analyzer, disabling the IAM access analyzer service for that region.</td>
</tr>
<tr class="odd">
<td>config:DeleteConfigRule</td>
<td>Deletes the specified AWS Config rule and all of its evaluation results</td>
</tr>
<tr class="even">
<td>config:DeleteConfigurationRecorder</td>
<td>Deletes the AWS Config configuration recorder</td>
</tr>
<tr class="odd">
<td>config:DeleteDeliveryChannel</td>
<td>Deletes the AWS Config delivery channel</td>
</tr>
<tr class="even">
<td>config:StopConfigurationRecorder</td>
<td>Stops recording configurations and configuration changes for the specified recording group</td>
</tr>
</tbody>
</table>

**S3 Bucket Configuration Changes**

Threat actors may modify or delete S3 bucket configurations in order to exfiltrate data.

<table>
<colgroup>
<col style="width: 43%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th>S3:ListBuckets</th>
<th>Returns a list of all buckets owned by the authenticated sender of the request</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>S3:CreateBucket</td>
<td>Creates a new bucket</td>
</tr>
<tr class="even">
<td>S3:DeleteBucketPublicAccessBlock</td>
<td>Removes the PublicAccessBlock configuration for an Amazon S3 bucket</td>
</tr>
<tr class="odd">
<td>S3:PutBucketPolicy</td>
<td>Adds or updates a policy on a bucket</td>
</tr>
<tr class="even">
<td>S3:DeleteBucketPolicy</td>
<td>Deletes the policy from the bucket</td>
</tr>
<tr class="odd">
<td>S3:PutObjectAcl</td>
<td>Sets the access control list (ACL) permissions for an object in an Amazon S3 bucket</td>
</tr>
<tr class="even">
<td>S3:DeleteObjectAcl</td>
<td>Delete the access control list (ACL) of an object</td>
</tr>
<tr class="odd">
<td>S3:PutBucketCORS</td>
<td>Sets the CORS configuration for your bucket</td>
</tr>
<tr class="even">
<td>S3:DeleteBucketCORS</td>
<td>Deletes the CORS configuration information set for the bucket</td>
</tr>
<tr class="odd">
<td>S3:PutBucketEncryption</td>
<td>Configure default encryption and Amazon S3 Bucket Keys for an existing bucket</td>
</tr>
<tr class="even">
<td>S3:DeleteBucketEncryption</td>
<td>Resets the default encryption for the bucket as server-side encryption with Amazon S3 managed keys (SSE-S3)</td>
</tr>
</tbody>
</table>

**Other Actions**

Here are some other actions that you should highlight during your investigation

<table>
<colgroup>
<col style="width: 43%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th>kms:DisableKey</th>
<th>Sets the state of a KMS key to disabled. This change temporarily prevents use of the KMS key for cryptographic operations.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>kms:ScheduleKeyDeletion</td>
<td>Schedules the deletion of a KMS key.</td>
</tr>
<tr class="even">
<td>kms:PutKeyPolicy</td>
<td>Attaches a key policy to the specified KMS key. Key policies are the primary way to control access to KMS keys.</td>
</tr>
<tr class="odd">
<td>kms:CreateKey</td>
<td>Creates a unique customer managed KMS key in your AWS account and Region.</td>
</tr>
<tr class="even">
<td>EC2:DescribeInstances</td>
<td>Describes EC2 instances within your account.</td>
</tr>
<tr class="odd">
<td>EC2:RunInstances</td>
<td>Launches EC2 instances within your account.</td>
</tr>
<tr class="even">
<td>EC2:CreateVpc</td>
<td>Creates a VPC within your account.</td>
</tr>
<tr class="odd">
<td>EC2:DescribeVpcs</td>
<td>Describes VPCs within your account.</td>
</tr>
<tr class="even">
<td>EC2:CreateSecurityGroup</td>
<td>Creates a security group. A security group acts as a virtual firewall for your instance to control inbound and outbound traffic.</td>
</tr>
<tr class="odd">
<td>EC2:DeleteSecurityGroup</td>
<td>Deletes a security group.</td>
</tr>
<tr class="even">
<td>RDS:CreateDBInstance</td>
<td>Creates an RDS database instance.</td>
</tr>
<tr class="odd">
<td>RDS:DeleteDBInstance</td>
<td>Deletes an RDS database instance.</td>
</tr>
</tbody>
</table>
