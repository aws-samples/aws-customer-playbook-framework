# Responding to Ransom Attacks within AWS

## Notices

This document is provided for informational purposes only. It represents
the current product offerings and practices from Amazon Web Services
(AWS) as of the date of issue of this document, which are subject to
change without notice. Customers are responsible for making their own
independent assessment of the information in this document and any use
of AWS products or services, each of which is provided "as is" without
warranty of any kind, whether express or implied. This document does not
create any warranties, representations, contractual commitments,
conditions, or assurances from AWS, its affiliates, suppliers, or
licensors. The responsibilities and liabilities of AWS to its customers
are controlled by AWS agreements, and this document is not part of, nor
does it modify, any agreement between AWS and its customers.\
\
© 2021, Amazon Web Services, Inc. or its affiliates. All rights
reserved.

## Ransomware

Since the first recorded ransomware attack in 1989 called PC Cyborg,
ransomware has become a prominent monetization strategy used by criminal
organizations and threat actors across the internet. Other examples of
ransomware include:

-   CryptoLocker \[2014\]
-   Petya \[2016\]
-   WannaCry \[2017\]
-   NotPetya \[2017\]
-   Ryuk \[2019\])

Threat actors leverage issues and weaknesses across the customer's
infrastructure, exploiting vulnerable endpoints, insecure services, and
socially engineering employees.

Ransomware attacks are costing governments, nonprofits, and businesses
billions of dollars and interrupting operations. NotPetya forced
shipping giant Maersk to reinstall 4,000 servers and 45,000 PCs for
\$300M due to "serious business interruption." The ransomware attack on
the City of Baltimore cost over \$18M, and local governments from
Riviera Beach and Lake City, Florida will pay hackers \$1M combined to
get its systems and data back. The U.S. Federal Bureau of Investigation
(FBI) anticipates the threat of ransomware to become "more targeted,
sophisticated, and costly" in the near future. These warnings reach
beyond U.S. borders, with Europol also calling ransomware the "most
widespread and financially damaging form of cyberattack."

## What are ransom attacks? 

Ransom attacks are a monetization strategy, not a specific technology.
Attacks often leverage specific malicious software, or ransomware, which
threatens to publish victim information or block access until the ransom
is paid.

In theory, if the ransom is paid within the allotted time, systems and
data are decrypted and made available once again and normal operations
continue. However, if the ransom is not satisfied, organizations risk
permanent destruction or public-facing data leaks controlled by the
attacker. Attackers can also extort the customer further for release of
customer sensitive data and systems to third-parties or to the public.

## Ransomware does not care

Many ransom attacks are opportunistic in nature, meaning that ransomware
indiscriminately infects any accessible networks through human and/or
machine vectors. Security teams for education institutions, state and
local governments, and healthcare organizations are ramping up measures
to keep their data safe from an increase in ransomware attacks. Threat
actors understand how to identify weak spots in industry verticals. Many
education and government organizations are more prone to ransomware due
to a combination of shrinking budgets, gaps in security resources, and
legacy IT systems with known unpatched vulnerabilities. Similarly,
ransom attacks may target industries with intolerance for downtime, like
hospitals, in hopes of increasing the probability of payout.

## Why are ransom attacks effective? 

-   Security awareness among employees is low
-   Organizations are not backing up data, or fail to test existing backups
-   Attacks require little skill and result in significant payouts
-   Organizations are slow to patch critical common vulnerabilities and exposures (CVE)
-   Overburdened technical staff cannot address or anticipate all security gaps
-   Multiple vectors or channels are being used in a single attack
-   Stealing the data (data exfiltration, unauthorized copying) may not stop a business process, and customers may not react until something does such as encrypting or deleting the information

## To pay or not pay?

Active debates exist among cyber security professionals regarding the
decision to pay ransoms. Many experts, including the FBI, advise
organizations not to pay the ransom, arguing that paying does not
guarantee that locked systems or encrypted data will be made available
and will only continue to motivate nefarious behaviors. Even though
system and data access are not guaranteed after paying the ransom, some
organizations take a calculated risk to pay in hopes of resuming normal
operations. By doing so, they hope to reduce potential ancillary costs
of attacks, including lost productivity, decreased revenue over time,
exposure of sensitive data, and reputational damage. The ransomware
threat is serious but smart preparation and ongoing vigilance are
effective counters against it. The full armor of data security includes
both human and technical controls but there are features of the AWS
Cloud that help to mitigate ransomware attacks. AWS is committed to
providing you with tools, best practices, and services to help with high
availability, security, and resiliency to address bad actors on the
internet.

## Securing systems and data on AWS is a [shared responsibility](https://aws.amazon.com/compliance/shared-responsibility-model)

When you deploy applications and infrastructure in the AWS Cloud, AWS
helps by sharing the security responsibilities with you. AWS engineers
the underlying cloud infrastructure using secure design principles and
customers must implement their own security architecture for workloads
deployed on AWS. AWS is responsible for protecting the infrastructure
that runs all the services offered on the AWS Cloud. This infrastructure
is composed of the hardware, software, networking, and facilities that
run AWS Cloud services. The AWS Cloud services that a customer selects
determine the amount of configuration work the customer must perform as
part of their security responsibilities. Customers are always
responsible for managing and securing their data, classifying their
assets, and using AWS Identity Access Management (IAM) to apply the
appropriate permissions.

## AWS Support

If you or your customer(s) believe an account is under an active ransom
attack, the impacted customer should create a case from the AWS Support
Center from within the impacted account. If the root user cannot be
accessed or is compromised, then the customer should open a new AWS
account and open a support ticket from there. This process allows AWS to
perform identity verification with the customer and begin engaging
internal resources to best help with remediating the event.

## AWS Security Reference Architecture (AWS SRA)

The [Amazon Web Services (AWS) Security Reference Architecture (AWS
SRA)](https://docs.aws.amazon.com/prescriptive-guidance/latest/security-reference-architecture/welcome.html)
is a holistic set of guidelines for deploying the full complement of AWS
security services in a multi-account environment. It can be used to help
design, implement, and manage AWS security services so that they align
with AWS best practices.

## U.S. Cybersecurity & Infrastructure Security Agency

The [Ransomware Guide (Sept.
2020)](https://www.cisa.gov/sites/default/files/publications/CISA_MS-ISAC_Ransomware%20Guide_S508C.pdf)
provides best practices and references to help manage the risk posed by
ransomware and support an organization's coordinated and efficient
response to a ransomware incident.

## The European Union Agency for Cybersecurity (ENISA)

The [ENISA Threat Landscape 2020 -
Ransomware](https://www.enisa.europa.eu/publications/ransomware)
outlines the findings on ransomware, provides a description and analysis
of the domain, and lists relevant recent incidents. A series of proposed
actions for mitigation is provided.

## Australian Cyber Security Centre (ACSC)

The ACSC provides several guides including response to [Ransomware in
Australia](https://www.cyber.gov.au/sites/default/files/2020-10/Ransomware%20in%20Australia%20%28October%202020%29.pdf), [RANSOMWARE
ATTACKS EMERGENCY RESPONSE
GUIDE](https://www.cyber.gov.au/sites/default/files/2021-07/11515_ACSC_Emergency-Response-Guide_Accessible_08.12.20.pdf),
and [RANSOMWARE ATTACKS PREVENTION AND PROTECTION
GUIDE](https://www.cyber.gov.au/sites/default/files/2021-07/11515_ACSC_Prevention-And-Protection-Guide_Accessible_08.12.20.pdf).

## NIST Cybersecurity Framework

The guide, NIST Cybersecurity Framework (CSF): Aligning to the NIST CSF
in the AWS Cloud, is designed to help commercial and public sector
entities of any size and in any part of the world align with the CSF by
using AWS services and resources.

## Ransomware mitigation: Top 5 protections and recovery preparation actions

AWS has provided a public Security Blog covering [Ransomware mitigation:
Top 5 protections and recovery preparation
actions.](https://aws.amazon.com/blogs/security/ransomware-mitigation-top-5-protections-and-recovery-preparation-actions/)
These include:

-   Set up the ability to recover your apps and data
-   Encrypt your data
-   Apply critical patches
-   Follow a security standard
-   Make sure you are monitoring and automating responses

You should also use strong authentication for exposed remote access
services, notably remote desktop protocol and secure shell. [Multifactor
authentication combined with single
sign-on](https://docs.aws.amazon.com/singlesignon/latest/userguide/mfa-how-to.html)
is a technical mechanism that enables protection of remote connections
and resource requests.

## Development, Security & Operations (DevSecOps)

*Background*: The purpose of DevSecOps is to help customers safely
accelerate feedback loops with their customers (i.e., end users). By
accelerating these feedback loops, customers can ship more software
changes to production with higher quality, security, and stability. By
building security into the development process, customers can prevent
deploying vulnerabilities to end users in production environments.
According to the NIST, the cost of fixing a security defect once it's
made it to production can be up to 60 times more expensive than during
the development cycle
\[[Source](https://securityboulevard.com/2020/09/the-importance-of-fixing-and-finding-vulnerabilities-in-development/)\].

Based on data from the AWS Customer Incident Response Team, most
customer ransom attacks fall into one of three categories: 1/ A bad
actor scans source code repositories (e.g., GitHub) for AWS access keys.
Using these long-term access keys, they gain access to AWS resources in
customers' accounts. Risks increase based on the level of access
permitted by the access keys. 2/ By scanning public S3 buckets and
objects, a bad actor can block access and encrypt S3 buckets preventing
owners from accessing the data. 3/ A bad actor scans and leverages web
application vulnerabilities (e.g., code injection and cross-site
scripting) on public ports to obtain access to customer data.

Customers mistakenly believe that AWS provides many of these safeguards
automatically as part of having an AWS account. While AWS provides
thousands of security features including private by default capabilities
(e.g., EC2 Security Groups and S3 buckets), as part of the [shared
responsibility
model](https://aws.amazon.com/compliance/shared-responsibility-model/),
customers must detect and remediate security vulnerabilities in their
code and/or in their AWS environments. There are AWS services and tools
for detecting and remediating these non-compliant resources but
customers must have knowledge in applying the practices in an automatic
way across their AWS resources.

A deployment pipeline breaks up build, test, deploy, and release actions
into a series of stages. Each stage provides increasing confidence,
usually at the cost of extra time. Early stages can find most problems
yielding faster feedback, while later stages provide slower and more
thorough probing. Builders automate many of the actions in these
deployment pipelines; Deployment pipelines are a key architectural
construct of [Continuous
Delivery](https://aws.amazon.com/devops/continuous-delivery/). The
deployment pipeline\'s job is to detect any changes that will lead to
problems in production. These can include performance, security, or
usability issues
\[[Source](https://martinfowler.com/bliki/DeploymentPipeline.html)\].
For the purposes of this document, we refer to the practice of building
security tests and checks into deployment pipelines so they can prevent
and mitigate ransom attacks as Continuous Security.

The principles of using Continuous Security for preventing and
mitigating ransom attacks are: 1/ Prevent committing secrets to source
code repositories so that bad actors do not have keys to access AWS
resources. 2/ Prevent common developer mistakes that make web
applications vulnerable (static and runtime). 3/ Ensure there are
backups for compromised AWS resources so that bad actors do not have as
much to gain from blocking access to the resources. 4/ Encrypt all
relevant data so that if a bad actor gains access to data, they have
much less to gain through extortion. 5/ Detect when unauthorized users
access AWS resources so that mitigations can take place. 6/ Ensure IAM
access keys are short lived using IAM roles so there is limited access
duration to compromised resources. 7/ Ensure IAM permissions use least
privilege to prevent really bad things from happening if a bad actor
gains access to the keys. 8/ Prevent or detect misconfiguration that
make AWS resources vulnerable.

*Prevent and detect when committing secrets to source code repos***:**
From their developer environments (e.g., their IDEs), customers should
run secrets detection Static Application Security Testing (SAST) tools
prior to committing code to a source code repo. In addition, the first
stage of an automated deployment pipeline runs a secrets detection SAST
tool to detect when secrets have been committed to the source code repo.
When discovering secrets, configure the pipeline to fail, notify
developers, and stop future commits until applying a fix. Furthermore,
run automation to cleanse git history of the secrets and disable the AWS
access keys. Example tools for this solution are AWS CodePipeline, AWS
CodeBuild, git-secrets, and custom code for remediation of secrets.

*Prevent and detect common developer mistakes that make applications
vulnerable***:** From their developer environments (e.g., their IDEs),
customers should run SAST and code review tools that detect web
vulnerabilities such as code injection, cross-site scripting, and other
insecure coding practices. In addition, the first stage of an automated
deployment pipeline runs these tools to detect when web vulnerabilities
are committed to the source code repo. When discovering these web
vulnerabilities, configure the pipeline to fail, notify developers, and
stop future commits until applying a fix. What is more, customers can
configure runtime detection and remediation of these web vulnerabilities
as part of a shared services deployment pipeline that provisions and
configures these services as code. Example tools for this solution are
Amazon CodeGuru, AWS CloudFormation, AWS CodePipeline, AWS CodeBuild,
SonarQube/CheckMarx, and AWS WAF and WAF Security Automations.

*Backups for compromised AWS resources:* At runtime, detect untagged
and/or relevant AWS resources that do not have backups. In addition,
customers can configure the provisioning of AWS resources that provide
runtime detection as part of a shared services deployment pipeline that
provisions and configure these services as code. Example tools for this
solution are AWS Backup, AWS CloudFormation, AWS CodePipeline, AWS
CodeBuild, Amazon EventBridge, [AWS Config
Rules](https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
(e.g., required-tags, redshift-backup-enabled,
aurora-resources-protected-by-backup-plan,
backup-plan-min-frequency-and-min-retention-check,
backup-recovery-point-manual-deletion-disabled,
db-instance-backup-enabled, dynamodb-in-backup-plan,
ebs-in-backup-plan), Amazon SNS, and AWS Lambda.

*Encrypting data in transit and at rest***:** From their developer
environments (e.g., their IDEs), customers should run SAST tools that
detect web vulnerabilities such as code injection and cross-site
scripting. In addition, the first stage of an automated deployment
pipeline runs an IaC linting tool to detect when developers defined
unencrypted AWS resources as code and commit the configuration code to
the source code repo. When discovering these unencrypted resources,
configure the pipeline to fail, notify developers, and stop future
commits until applying a fix. Furthermore, customers can configure the
provisioning of AWS resources that provide runtime detection and
remediation of these unencrypted AWS resources as part of a shared
services deployment pipeline that provisions and configure these
services as code. Example tools for this solution are AWS
CloudFormation, AWS CodePipeline, AWS CloudFormation Guard, AWS
CodeBuild, Amazon EventBridge, [AWS Config
Rules](https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
(e.g., ec2-ebs-encryption-by-default, dynamodb-table-encrypted-kms,
rds-snapshot-encrypted, cloudwatch-log-group-encrypted,
cloud-trail-encryption-enabled,
s3-bucket-server-side-encryption-enabled, api-gw-ssl-enabled,
cloudfront-custom-ssl-certificate), Amazon SNS, AWS KMS, and AWS Lambda.

*Detect when unauthorized users access AWS resources:* At runtime,
detect data exfiltration \"spikes\" (e.g., using CloudWatch metrics
specifically
[NetworkOut](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html)).
For example, it is possible an attacker performed data destruction and
left a ransom note, and in these cases, there is no opportunity for data
recovery by working with the malicious actor. What is more, customers
can configure the provisioning of AWS resources that provide runtime
detection as part of a shared services deployment pipeline that
provisions and configure these services as code. Example tools for this
solution are AWS CloudFormation, AWS CodePipeline, AWS CodeBuild, Amazon
EventBridge, Amazon GuardDuty, Amazon SNS, and Amazon CloudWatch
metrics.

*Ensure IAM access keys are short lived***:** At runtime, run tools that
detect various states of IAM access keys and policies including rotating
long-lived access keys. Customers can configure the provisioning of AWS
resources that provide runtime detection as part of a shared services
deployment pipeline that provisions and configure these services as
code. Example tools for this solution are AWS CloudFormation, AWS
CodePipeline, AWS CodeBuild, Amazon EventBridge, [AWS Config
Rules](https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
(e.g., iam-policy-no-statements-with-admin-access,
iam-policy-no-statements-with-full-access, iam-root-access-key-check,
iam-user-no-policies-check, iam-user-unused-credentials-check,
access-keys-rotated), and AWS Lambda.

*Ensure IAM permissions use least privilege***:** From developers'
environments (e.g., their IDEs), customers should run SAST tools that
detect overly permissive permissions definitions prior to committing
code to a source code repo. In addition, the first stage of an automated
deployment pipeline runs the SAST tool to detect overly permissive
policy definitions after the code has been committed to a source code
repo. When discovering overly permissive policy definitions, configure
the pipeline to fail, notify developers, and stop future commits until
applying a fix. Moreover, detect overly permissive policies across the
AWS account or accounts (i.e., outside of source code repos). In turn,
customers can configure the provisioning of AWS resources that provide
runtime detection as part of a shared services deployment pipeline that
provisions and configure these services as code. Example tools for this
solution are AWS CloudFormation, AWS CodePipeline, AWS CodeBuild,
PMapper/AWSPX, AWS IAM Access Analyzer, Amazon EventBridge, AWS Config
Rules, and AWS Lambda.

*Prevent misconfiguration that make AWS resources vulnerable*: The
misconfiguration of or not enabling certain AWS resources can make
customers vulnerable to a ransom attack. The most notable resources are
those related to networking, compute, storage, and databases. What is
more, customers can configure the provisioning of AWS resources that
provide runtime detection as part of a shared services deployment
pipeline that provisions and configure these services as code. Example
tools for this solution are AWS CloudFormation, AWS CodePipeline, AWS
CodeBuild, Amazon EventBridge, AWS Config Rules (e.g.,
s3-account-level-public-access-blocks, s3-bucket-default-lock-enabled,
s3-bucket-level-public-access-prohibited, s3-bucket-logging-enabled,
s3-bucket-policy-not-more-permissive, s3-bucket-public-read-prohibited,
s3-bucket-public-write-prohibited, s3-bucket-replication-enabled,
cloud-trail-log-file-validation-enabled,
ec2-security-group-attached-to-eni, vpc-default-security-group-closed,
and vpc-flow-logs-enabled, guardduty-enabled-centralized), Amazon
Inspector Network Reachability, Amazon VPC Reachability Analyzer, Amazon
S3, and AWS Lambda.

## Amazon Elastic Compute Cloud (*Amazon EC2*) - Microsoft Windows

### Identify
-   Assess the security posture of the account to identify and remediate security gaps
    -   AWS developed a new open source [Self-Service Security Assessment](https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) tool that provides customers with a point-in-time assessment to gain valuable insights into the security posture of their AWS account.
-   Maintain a complete asset inventory of all resources including domain controllers, Microsoft Windows EC2 instances, Microsoft Windows Servers and Databases, and any integration with external identity providers.
-   Perform recurring vulnerability analysis of your hosts using utilities such as [Amazon Inspector](https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/)
-   To protect your organization, Microsoft recommends that you use the information in the [**Human-Operated Ransomware Mitigation Project Plan**](https://download.microsoft.com/download/7/5/1/751682ca-5aae-405b-afa0-e4832138e436/RansomwareRecommendations.pptx) PowerPoint presentation, which includes [securing privileged access](https://aka.ms/spa).
-   Use [CloudWatch metrics](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html), specifically NetworkPacketsOut, to look for data exfiltration "spikes." It is possible an attacker performed data destruction and left a ransom note, and in these cases, there is no opportunity for data recovery by working with the malicious actor

### Protect
-   Apply the latest updates to your operating systems and apps
-   Back up your files with File History if your PC's manufacturer has not already turned it on. [Learn more about File History](https://support.microsoft.com/en-us/windows/file-history-in-windows-5de0e203-ebae-05ab-db85-d5aa0a199255)
-   Back up important files regularly. Use the 3-2-1 rule. Keep three backups of your data, on two different storage types, and at least one backup offsite
-   Block Known Ransomware File Types
-   [Block Macros in Office Documents](https://support.microsoft.com/en-us/topic/enable-or-disable-macros-in-office-files-12b036fd-d140-4e74-b45e-16fed1a7e5c6)
-   Educate your employees so they can identify social engineering and spear-phishing attacks
-   [Follow Best Practices for securing your Active Directory Services](https://www.calcomsoftware.com/how-to-secure-your-active-directory-when-anonymous-users-must-have-access/)
-   [Follow Top 10 Most Important Group Policy Settings for Preventing Security Breaches](https://www.lepide.com/blog/top-10-most-important-group-policy-settings-for-preventing-security-breaches/)
-   [Implement controlled folder access](https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/controlled-folders). It can stop ransomware from encrypting files and holding the files for ransom
-   Perform backups of EC2 instances
    -   Consider using [AWS Backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) or [AWS CloudEndure](https://aws.amazon.com/cloudendure-disaster-recovery/)
-   The Microsoft 365 Defender Threat Intelligence Team has provided [comprehensive report](https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) identifying actions to secure your Microsoft resources prior to a ransomware event
    -   Harden internet-facing assets and ensure they have the latest security updates. Use threat and vulnerability management to audit these assets regularly for vulnerabilities, misconfigurations, and suspicious activities.
    -   Monitor for brute-force attempts. Check excessive failed authentication attempts (Windows security event ID 4625)
    -   Monitor for clearing of Event Logs, especially the Security Event log and PowerShell Operational logs. Microsoft Defender ATP raises the alert "Event log was cleared" and Windows generates an Event ID 1102 when this occurs
    -   Practice the principle of least-privilege and maintain credential hygiene. Avoid the use of domain-wide, admin-level service accounts. Enforce strong randomized, just-in-time local administrator passwords. Use tools like LAPS
    -   Secure Remote Desktop Gateway using solutions like Azure Multi-Factor Authentication (MFA). If you do not have an MFA gateway, enable network-level authentication (NLA)
    -   Turn on AMSI for Office VBA if you have Office 365
    -   Turn on attack surface reduction rules, including rules that block credential theft, ransomware activity, and suspicious use of PsExec and WMI. To address malicious activity initiated through weaponized Office documents, use rules that block advanced macro activity, executable content, process creation, and process injection initiated by Office applications. To assess the impact of these rules, deploy them in audit mode
    -   Turn on cloud-delivered protection and automatic sample submission on Windows Defender Antivirus. These capabilities use artificial intelligence and machine learning to quickly identify and stop new and unknown threats
    -   Turn on tamper protection features to prevent attackers from stopping security services
    -   Utilize the Windows Defender Firewall and your network security system to prevent RPC and SMB communication among endpoints whenever possible. This limits lateral movement as well as other attack activities
-   Turn on and update Windows Defender Antivirus - commercial, subscription paid Endpoint Detection and Response (EDR) solutions are preferred
-   Turn on [Controlled Folder Access](https://support.microsoft.com/en-us/topic/ransomware-protection-in-windows-security-445039d6-537a-488a-ad53-48906f346363) in Windows 10 to protect your important local folders from unauthorized programs like ransomware or other malware
-   Use [Systems Manager and Amazon Inspector](https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/) to check if EC2 instances running Microsoft Windows contain any common vulnerabilities and exposures (CVEs)
-   Verify your backups and ensure the infection has not spread into them

### Detect
-   The Microsoft 365 Defender Threat Intelligence Team has provided a [comprehensive report](https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) identifying things to look for against multiple ransomware variants
-   Use VPCFlowLogs to identify inappropriate database access from external IP addresses
    -   You can [investigate VPC Flow with Amazon Detective](https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) or [Amazon Athena](https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)

### Respond
-   Enforce NACLs based on network IoCs to prevent further traffic
-   Isolate the infected systems
-   Inspect backups for potential infection
-   Remove any compromised systems from the network.
    -   Follow regulatory requirements or internal company policy to determine if forensics of the EC2 instance is required
    -   If forensics of the instances are required OR data needs to be recovered, then [quarantine the EC2 instance](https://support.alertlogic.com/hc/en-us/articles/115005534366-Quarantine-an-EC2-Instance)
-   [Remove compromised Domain Controller metadata from the domain](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/ad-ds-metadata-cleanup)
-   Use flow logs to determine the exfiltration IP, look for any other instances communicating with the IPs that data was exfiltrated to/access from CloudTrail

### Recover
-   It is recommended not to pay the ransom
    -   Paying the ransom is a gamble as to whether the criminal will honor the transaction after receiving payment
    -   If no data backups exist, then you should do a cost benefit analysis and weigh the value of the data/reputational compromise against the payment to the attacker
    -   You directly enable the attacker to continue their operations against your company or others if you choose to pay the ransom
-   Create new EC2 instances from a trusted AMI
-   Use CloudEndure Disaster Recovery to select the latest recovery point before the ransomware attack or data corruption to restore your workloads on AWS
    -   If using an alternate data backup strategy, validate the backups have not been infected and restore from the last scheduled event prior to the ransomware event
-   Visit <https://www.nomoreransom.org/> to identify if a decryptor is available for the malware variant the infected your data

## Amazon Elastic Compute Cloud (*Amazon EC2*) - Unix/Linux

### Identify
-   Assess your security posture to identify and remediate security gaps
    -   AWS developed a new open source [Self-Service Security Assessment](https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) tool that provides customers with a point-in-time assessment to quickly gain valuable insights into the security posture of their AWS account.
-   Maintain a complete asset inventory of all resources including servers, networking devices, network/file shares and developer machines
-   Perform recurring vulnerability analysis of your hosts using utilities such as [Amazon Inspector](https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/)
-   Use [CloudWatch metrics](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html), specifically NetworkOut, to look for data exfiltration "spikes." It is possible an attacker performed data destruction and left a ransom note, and in these cases, there is no opportunity for data recovery by working with the malicious actor

### Protect
-   Apply the latest updates to your operating systems and apps
-   Back up important files regularly. Use the 3-2-1 rule. Keep three backups of your data, on two different storage types, and at least one backup offsite
-   Block Known Ransomware File Types
-   Consider using AWS Config to create rules to monitor for changes to AWS services. AWS Config has several managed rules prebuilt to monitor EBS and EC2 including:
    -   [ebs-in-backup-plan](https://docs.aws.amazon.com/config/latest/developerguide/ebs-in-backup-plan.html)
    -   [ebs-optimized-instance](https://docs.aws.amazon.com/config/latest/developerguide/ebs-optimized-instance.html)
    -   [ebs-snapshot-public-restorable-check](https://docs.aws.amazon.com/config/latest/developerguide/ebs-snapshot-public-restorable-check.html)
    -   [ec2-ebs-encryption-by-default](https://docs.aws.amazon.com/config/latest/developerguide/ec2-ebs-encryption-by-default.html)
    -   [ec2-imdsv2-check](https://docs.aws.amazon.com/config/latest/developerguide/ec2-imdsv2-check.html)
    -   [ec2-instance-detailed-monitoring-enabled](https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-detailed-monitoring-enabled.html)
    -   [ec2-instance-managed-by-systems-manager](https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-managed-by-systems-manager.html)
    -   [ec2-instance-multiple-eni-check](https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-multiple-eni-check.html)
    -   [ec2-instance-no-public-ip](https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-no-public-ip.html)
    -   [ec2-instance-profile-attached](https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-profile-attached.html)
    -   [ec2-managedinstance-applications-blacklisted](https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-applications-blacklisted.html)
    -   [ec2-managedinstance-applications-required](https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-applications-required.html)
    -   [ec2-managedinstance-association-compliance-status-check](https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-association-compliance-status-check.html)
    -   [ec2-managedinstance-inventory-blacklisted](https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-inventory-blacklisted.html)
    -   [ec2-managedinstance-patch-compliance-status-check](https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-patch-compliance-status-check.html)
    -   [ec2-managedinstance-platform-check](https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-platform-check.html)
    -   [ec2-security-group-attached-to-eni](https://docs.aws.amazon.com/config/latest/developerguide/ec2-security-group-attached-to-eni.html)
    -   [ec2-stopped-instance](https://docs.aws.amazon.com/config/latest/developerguide/ec2-stopped-instance.html)
    -   [ec2-volume-inuse-check](https://docs.aws.amazon.com/config/latest/developerguide/ec2-volume-inuse-check.html)
-   Educate your employees so they can identify social engineering and spear-phishing attacks
-   Have malware detection and antivirus enabled on all systems - commercial, subscription paid Endpoint Detection and Response (EDR) solutions are preferred.
    -   An example guide can be found at [How to Install and Use Linux Malware Detect (LMD) with ClamAV as Antivirus Engine](https://www.tecmint.com/install-linux-malware-detect-lmd-in-rhel-centos-and-fedora/)
-   Perform backups of EC2 instances
    -   Consider using [AWS Backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) or [AWS CloudEndure](https://aws.amazon.com/cloudendure-disaster-recovery/)
-   The Microsoft 365 Defender Threat Intelligence Team has provided a [comprehensive report](https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) identifying actions to secure Microsoft resources prior to a ransomware event. Many of these recommendations can be adapted for Unix & Linux including:
    -   Determine where highly privileged accounts are logging on and exposing credentials.
    -   Harden internet-facing assets and ensure they have the latest security updates. Use threat and vulnerability management to audit these assets regularly for vulnerabilities, mis-configurations, and suspicious activities.
    -   Monitor for brute-force attempts. Check excessive failed authentication attempts (/var/log/auth.log or /var/log/secure)
    -   Monitor for clearing of Event Logs
    -   Practice the principle of least-privilege and maintain credential hygiene. Avoid the use of domain-wide, admin-level service accounts. Enforce strong randomized, just-in-time local administrator passwords.
    -   Turn on tamper protection features to prevent attackers from stopping security services
-   Use an endpoint security agent such as [Wazuh](https://documentation.wazuh.com/current/getting-started/components/index.html)
-   Use a file integrity monitor such as [Tripwire](https://github.com/Tripwire/tripwire-open-source) to detect changes to critical files
-   Use [Systems Manager and Amazon Inspector](https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/) to check if EC2 instances contain any common vulnerabilities and exposures (CVEs)
-   Verify your backups and ensure the infection has not spread into them

### Detect
-   Table 1 from [No Strings on Me: Linux and Ransomware](https://www.sans.org/reading-room/whitepapers/tools/strings-me-linux-ransomware-39870), by Richard Horne, identifies multiple indicators to monitor for including:
    -   A new process that has never been seen on the endpoint prior that has external network connectivity requests
    -   Attempted communications with DNS names where entropy is found or directly with bare IPs
    -   Change in distribution Yum or Apt repositories
    -   Creation of .sh files inside of non-user-based home directories
    -   Enumeration of shared web, database, or file storage directory trees
    -   Escalation of privileges or attempts to gain sudo access
    -   External as well as internal network communications
    -   File names generated with entropy or that are in a quick succession
    -   Files being renamed multiple times in the same directory tree
    -   Files created with odd file extensions
    -   Focus being given to outliers and events that fall outside of the typical day to day operations
    -   High memory usage for short or sustained periods of time
    -   Large process tress where the Parent Process is terminated prior to the child processes
    -   Modification of boot options
    -   Multiple copies of the same file in multiple directories
    -   Multiple modifications within a single directory
    -   Possible calls for encryption libraries
    -   Possible creation and running of processes inside of the/tmp directory
    -   Quick and successive directory enumeration
    -   Removal of files from directories using wildcards or without confirmation
    -   The use of chmod with wildcards (or chmod 777)
    -   The use of wget or curl cmds
    -   Use of the chmod cmd to change executable files to overly permissive rights
    -   Use of the strings cmd to attempt to encode communications prior to or during infection
-   Use VPCFlowLogs to identify inappropriate database access from external IP addresses
    -   You can [investigate VPC Flow with Amazon Detective](https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) or [Amazon Athena](https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)

### Respond
-   Inspect backups for potential infection
-   Isolate the infected systems
-   Remove any compromised systems from the network.
    -   Follow regulatory requirements or internal company policy to determine is forensics of the EC2 instance is required
    -   If forensics of the instances are required OR data needs to be recovered, then [quarantine the EC2 instance](https://support.alertlogic.com/hc/en-us/articles/115005534366-Quarantine-an-EC2-Instance)

### Recover
-   It is recommended not to pay the ransom
    -   Paying the ransom is a gamble as to whether the criminal will honor the transaction after receiving payment
    -   If no data backups exist, then you should do a cost benefit analysis and weigh the value of the data/reputational compromise against the payment to the attacker
    -   You directly enable the attacker to continue their operations against your company or others if you choose to pay the ransom
-   Create new EC2 instances from a trusted AMI
-   Follow regulatory requirements or internal company policy to determine if forensics of the EC2 instance is required
-   Use CloudEndure Disaster Recovery to select the latest recovery point before the ransomware attack or data corruption to restore your workloads on AWS
    -   If using an alternate data backup strategy, validate the backups have not been infected and restore from the last scheduled event prior to the ransomware event
-   Visit <https://www.nomoreransom.org/> to identify if a decryptor is available for the malware variant the infected your data

## Amazon Simple Storage Service (S3)

### Identify
-   Assess your security posture to identify and remediate security gaps
    -   AWS developed a new open source [Self-Service Security Assessment](https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) tool that provides customers with a point-in-time assessment to gain valuable insights into the security posture of their AWS account.
-   Consider implementing [AWS GuardDuty](https://aws.amazon.com/guardduty/) to continuously monitor for malicious activity and unauthorized behavior to protect your AWS accounts, workloads, and data stored in Amazon S3
-   Consider using [AWS Config](https://aws.amazon.com/config/), which is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources
-   Darkbit provides an example of [AWS S3 Data Loss Prevention](https://darkbit.io/blog/simple-dlp-for-amazon-s3) that can be used to identify unauthorized copying of objects. Other specific operations could be added in the pattern as well based upon your business use case(s)
-   Maintain a complete asset inventory of all resources including servers, networking devices, network/file shares and developer machines

### Protect
-   Consider [enabling replication](http://docs.aws.amazon.com/AmazonS3/latest/dev/crr.html) - same region or cross region. Cross region replication protects data against application defects and operator errors by maintaining a second copy in another region
-   Consider using AWS Config to create rules to monitor for changes to AWS services. AWS Config has several managed rules prebuilt to monitor EBS and EC2 including:
    -   [s3-account-level-public-access-blocks](https://docs.aws.amazon.com/config/latest/developerguide/s3-account-level-public-access-blocks.html)
    -   [s3-account-level-public-access-blocks-periodic](https://docs.aws.amazon.com/config/latest/developerguide/s3-account-level-public-access-blocks-periodic.html)
    -   [s3-bucket-blacklisted-actions-prohibited](https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-blacklisted-actions-prohibited.html)
    -   [s3-bucket-default-lock-enabled](https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-default-lock-enabled.html)
    -   [s3-bucket-level-public-access-prohibited](https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-level-public-access-prohibited.html)
    -   [s3-bucket-logging-enabled](https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-logging-enabled.html)
    -   [s3-bucket-policy-grantee-check](https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-policy-grantee-check.html)
    -   [s3-bucket-policy-not-more-permissive](https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-policy-not-more-permissive.html)
    -   [s3-bucket-public-read-prohibited](https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-public-read-prohibited.html)
    -   [s3-bucket-public-write-prohibited](https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-public-write-prohibited.html)
    -   [s3-bucket-replication-enabled](https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-replication-enabled.html)
    -   [s3-bucket-server-side-encryption-enabled](https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-server-side-encryption-enabled.html)
    -   [s3-bucket-ssl-requests-only](https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-ssl-requests-only.html)
    -   [s3-bucket-versioning-enabled](https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-versioning-enabled.html)
    -   [s3-default-encryption-kms](https://docs.aws.amazon.com/config/latest/developerguide/s3-default-encryption-kms.html)
-   Consider using [AWS Key Management Service (KMS)](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) keys to encrypt all objects and to prevent an attacker from applying their encryption key
-   Consider using [AWS  S3 Block Public Access Feature](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) to mitigate unintentional exposure of  objects
-   Consider using [S3 Intelligent Tiering](https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) for object backups and cost optimization
-   Consider using [S3 Object Lock](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html) so you can store objects using a *write-once-read-many* (WORM) model. Object Lock can help prevent objects from being deleted or overwritten for a fixed amount of time or indefinitely.
    -   In Object Lock Compliance Mode**,** a protected object version cannot be overwritten or deleted by any user, including the root user in your AWS account. When an object is locked in compliance mode, its retention mode cannot be changed, and its retention period cannot be shortened. Compliance mode ***ensures*** that an object version cannot be overwritten or deleted for the retention period's duration.
-   Enable [CloudTrail event logging for S3 buckets and objects](https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html) containing sensitive or critical information. By default, CloudTrail trails do not log data events, but you can configure trails to log data events for S3 buckets that you specify, or to log data events for all the Amazon S3 buckets in your AWS account.
-   Enable [CloudTrail server level logging for S3 buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerLogs.html) and objects containing sensitive or critical information. Server access logging provides detailed records for the requests that are made to a bucket
-   Enable [S3 Object Versioning](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) to allow restoral of modified objects
    -   To set the number of versions kept, [set a lifecycle policy](http://docs.aws.amazon.com/AmazonS3/latest/dev/intro-lifecycle-rules.html) to apply to noncurrent versions
-   Enable [S3 MFA delete](https://docs.aws.amazon.com/AmazonS3/latest/userguide/MultiFactorAuthenticationDelete.html) to prevent an attacker from disabling versioning and overwriting all objects within a bucket
-   Enforce multi-factor authentication (MFA)
-   Enforce password complexity requirements and establish expiration periods
-   Implement [CIS AWS Foundations](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html) including expiration of accounts and mandatory credential rotations
-   Run an [IAM Credential Report](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) to list all users in your account and the status of their various credentials, including passwords, access keys, and MFA devices
-   Take steps to [prevent any new credentials from being publicly exposed](http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)
-   Use [AWS IAM Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) to identify the resources in your organization and accounts, such as Amazon S3 buckets or IAM roles that are shared with an external entity. This lets you identify unintended access to your resources and data, which is a security risk
-   Use IAM roles to manage permissions
    -   Implement least-privilege and do not allow s3:\* permissions
-   Use and routinely audit bucket policies
    -   Only make public what needs to be, and ensure all of objects are protected by being private
-   While we cannot recommend specific third-party solutions, there are also products from our Partners, including Veritas and CommVault, and other backup software providers that have the native capability to backup objects in S3 to their managed backup storage locations inside or outside of S3

### Detect

-   Check your CloudTrail log for unsanctioned activity such as creation of unauthorized IAM users, policies, roles or temporary security credentials
-   Check your CloudTrail log to review your AWS account for any unauthorized AWS usage, such as unauthorized EC2 instances, Lambda functions, or EC2 Spot bids. You can also check usage by logging into your AWS Management Console and reviewing each service page. The \"Bills\" page in the Billing console can also be checked for unexpected usage
    -   Please keep in mind that unauthorized usage can occur in any region and that your console may show you only one region at a time. To switch between regions, you can use the dropdown in the top-right corner of the console screen
-   Ransom note provided either as an object within the bucket or via e-mail to the customer
-   S3 Objects are deleted or entire S3 buckets are deleted
    -   Note: With data destruction events, a ransom note may or may not be provided. Also ensure you check CloudWatch metrics and CloudTrail S3 events to verify if data exfiltration occurred or not to delineate between a ransom or data destruction attack
-   S3 Objects are encrypted using a key from an account not owned by the customer

### Respond
-   Delete or rotate [IAM User Keys](https://console.aws.amazon.com/iam/home#users) and [Root User Keys](https://console.aws.amazon.com/iam/home#security_credential); you may wish to rotate all keys in your account if you cannot identify a specific key or keys that has been exposed
-   Delete unauthorized [IAM Users](https://console.aws.amazon.com/iam/home#users.)
-   Delete unauthorized [policies](https://console.aws.amazon.com/iam/home#/policies)
-   Delete unauthorized [roles](https://console.aws.amazon.com/iam/home#/roles)
-   If your application uses exposed Access Keys, you need to replace the Key. To replace the Key, first create a second Key (at that point, both Keys will be active) and then modify your application to use the new Key. Then disable (but do not delete) exposed Keys by clicking on the "Make inactive" option in the console. If there are any problems with your application, you can reactivate exposed Keys. When your application is fully functional using the new Key, please delete exposed Keys
-   [Revoke temporary credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Temporary credentials can also be revoked by deleting the IAM User. *NOTE:* Deleting IAM Users may impact production workloads and should be done with care

### Recover
-   It is recommended not to pay the ransom
    -   Paying the ransom is a gamble as to whether the criminal will honor the transaction after receiving payment
    -   If no data backups exist, then you should do a cost benefit analysis and weigh the value of the data/reputational compromise against the payment to the attacker
    -   You directly enable the attacker to continue their operations against your company or others if you choose to pay the ransom
-   Implement procedures under Protect before attempting to restore data or objects
-   Remove [delete markers](https://docs.aws.amazon.com/AmazonS3/latest/userguide/RemDelMarker.html) for versioned objects
-   Re-create deleted buckets
-   Restore objects from using [S3 Intelligent Tiering](https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) object backups or replicated region bucket
-   *Note:* There is currently no \"undelete\" capability for S3, and AWS does not have the ability to recover data that has been deleted. In the current era of data storage compliance and regulations such as [GDPR](https://gdpr-info.eu/) and [CCPA](https://oag.ca.gov/privacy/ccpa), Amazon S3 cannot continue to store customer data  explicitly deleted from the customer's account. Once an object is deleted, it can no longer be recovered by AWS regardless of how quickly the unintended deletion is reported to AWS

## Relational Database Service (RDS)

### Identify
-   Assess your security posture to identify and remediate security gaps
    -   AWS developed a new open source [Self-Service Security Assessment](https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) tool that provides customers with a point-in-time assessment to gain valuable insights into the security posture of their AWS account.
-   Consider using [AWS Config](https://aws.amazon.com/config/), which is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources
-   Consider implementing [AWS GuardDuty](https://aws.amazon.com/guardduty/) to continuously monitor for malicious activity and unauthorized behavior to protect your AWS accounts, workloads, and data stored in Amazon RDS
-   Maintain a complete asset inventory of all resources including servers, networking devices, network/file shares and developer machines

### Protect
-   Additional references and steps are available in [Security in Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.html)
-   Consider using AWS Config to create rules to monitor for changes to AWS services. AWS Config has several managed rules prebuilt to monitor RDS including:
    -   [rds-automatic-minor-version-upgrade-enabled](https://docs.aws.amazon.com/config/latest/developerguide/rds-automatic-minor-version-upgrade-enabled.html)
    -   [rds-cluster-deletion-protection-enabled](https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-deletion-protection-enabled.html)
    -   [rds-cluster-iam-authentication-enabled](https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-iam-authentication-enabled.html)
    -   [rds-cluster-multi-az-enabled](https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-multi-az-enabled.html)
    -   [rds-enhanced-monitoring-enabled](https://docs.aws.amazon.com/config/latest/developerguide/rds-enhanced-monitoring-enabled.html)
    -   [rds-instance-deletion-protection-enabled](https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-deletion-protection-enabled.html)
    -   [rds-instance-iam-authentication-enabled](https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-iam-authentication-enabled.html)
    -   [rds-instance-public-access-check](https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-public-access-check.html)
    -   [rds-in-backup-plan](https://docs.aws.amazon.com/config/latest/developerguide/rds-in-backup-plan.html)
    -   [rds-logging-enabled](https://docs.aws.amazon.com/config/latest/developerguide/rds-logging-enabled.html)
    -   [rds-multi-az-support](https://docs.aws.amazon.com/config/latest/developerguide/rds-multi-az-support.html)
    -   [rds-snapshots-public-prohibited](https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshots-public-prohibited.html)
    -   [rds-snapshot-encrypted](https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshot-encrypted.html)
    -   [rds-storage-encrypted](https://docs.aws.amazon.com/config/latest/developerguide/rds-storage-encrypted.html)
-   Have a third-party host-based intrusion detection system (HIDS) solution (such as OSSEC, Tripwire, Wazuh, [Amazon Inspector](https://aws.amazon.com/inspector/))
-   Run your DB instance in a virtual private cloud (VPC) based on the Amazon VPC service for the greatest possible network access control. For more information about creating a DB instance in a VPC, see [Amazon Virtual Private Cloud VPCs and Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.html).
-   Use AWS Identity and Access Management (IAM) policies to assign permissions that determine who can manage Amazon RDS resources. For example, you can use IAM to determine who can create, describe, modify, and delete DB instances, tag resources, or modify security groups.
-   Use Amazon RDS encryption to secure your DB instances and snapshots at rest. Amazon RDS encryption uses the industry standard AES-256 encryption algorithm to encrypt your data on the server that hosts your DB instance. For more information, see [Encrypting Amazon RDS resources](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Encryption.html)
-   Use Secure Socket Layer (SSL) or Transport Layer Security (TLS) connections with DB instances running the MySQL, MariaDB, PostgreSQL, Oracle, or Microsoft SQL Server database engines. For more information on using SSL/TLS with a DB  instance, see [Using  SSL/TLS to encrypt a connection to a DB instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html)
-   Use security groups to control what IP addresses or Amazon EC2 instances can connect to your databases on a DB instance. When you first create a DB instance, its security system prevents any database access except through rules specified by an associated security group.
-   Use network encryption and transparent data encryption with Oracle DB instances; for more information, see [Oracle native network encryption](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.NetworkEncryption.html) and [Oracle Transparent Data Encryption](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.AdvSecurity.html)
-   Use security features of your DB engine to control who can log in to the databases on a DB instance. These features work just as if the database was on your local network. 

### Detect
-   AWS GuardDuty machine learning can detect suspicious behavior including Generating Amazon Relational Database Service (Amazon RDS) snapshots as part of data exfiltration attempts. An example is provided in the AWS Security Blog [How you can use Amazon GuardDuty to detect suspicious activity within your AWS account](https://aws.amazon.com/blogs/security/how-you-can-use-amazon-guardduty-to-detect-suspicious-activity-within-your-aws-account/)
-   Review EC2 operating system and application logs for inappropriate logins, installation of unknown software, or the presence of unrecognized files
-   Use VPCFlowLogs to identify inappropriate database access from external IP addresses
    -   You can [investigate VPC Flow with Amazon Detective](https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) or [Amazon Athena](https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)
    -   The [AWS Security Analytics Bootstrap](https://github.com/awslabs/aws-security-analytics-bootstrap) enables customers to perform security investigations on AWS service logs by providing an Amazon Athena analysis environment that's quick to deploy, ready to use, and easy to maintain

### Respond
-   Delete or rotate [IAM User Keys](https://console.aws.amazon.com/iam/home#users) and [Root User Keys](https://console.aws.amazon.com/iam/home#security_credential); you may wish to rotate all keys in your account if you cannot identify a specific key or keys that has been exposed
-   Delete unauthorized [IAM Users](https://console.aws.amazon.com/iam/home#users.)
-   Delete unauthorized [policies](https://console.aws.amazon.com/iam/home#/policies)
-   Delete unauthorized [roles](https://console.aws.amazon.com/iam/home#/roles)
-   Delete unrecognized or unauthorized public snapshots or databases 
-   Identify an EC2 instances that had permissive access to the database(s) and investigate those as well
-   If your application uses exposed Access Keys, you need to replace the Key. To replace the Key, first create a second Key (at that point, both Keys will be active) and then modify your application to use the new Key. Then disable (but do not delete) exposed Keys by clicking on the "Make inactive" option in the console. If there are any problems with your application, you can reactivate exposed Keys. When your application is fully functional using the new Key, please delete exposed Keys
-   [Revoke temporary credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Temporary credentials can also be revoked by deleting the IAM User. *NOTE:* Deleting IAM Users may impact production workloads and should be done with care

### Recover
-   It is recommended not to pay the ransom
    -   Paying the ransom is a gamble as to whether the criminal will honor the transaction after receiving payment
    -   If no data backups exist, then you should do a cost benefit analysis and weigh the value of the data/reputational compromise against the payment to the attacker
    -   You directly enable the attacker to continue their operations against your company or others if you choose to pay the ransom
-   Delete the compromised database and create new RDS databases
-   Follow regulatory requirements or internal company policy to determine if forensics of the RDS database is required
-   Visit <https://www.nomoreransom.org/> to identify if a decryptor is available for the malware variant the infected your data
-   Use CloudEndure Disaster Recovery to select the latest recovery point before the ransomware attack or data corruption to restore your workloads on AWS
    -   If using an alternate data backup strategy, validate the backups have not been infected and restore from the last scheduled event prior to the ransomware event

### Following an Incident

### Report Ransom Attacks to Authorities
The FBI encourages organizations to report ransom incidents to law enforcement. The Internet Crime Complaint Center (IC3) accepts online internet crime complaints either from the actual victim or from a third party to the complainant and will work with them to determine the best course of action going forward. Be prepared to share:
-   Any relevant information you believe is necessary to support your complaint
-   Email header(s)
-   Financial transaction information (account information, transaction date and amount, recipient details)
-   Subject's name, address, telephone, email, website, and IP address
-   Specific details on how you were victimized
-   Victim's name, address, telephone, and email

### Perform root cause analysis
In most cases, your existing forensics tools will work in the AWS environment. Forensic teams benefit from the automated deployment of tools across AWS Regions and the ability to collect large volumes of data quickly with low friction using the same robust, scalable services their business-critical applications are built on. 

[Amazon Detective](https://aws.amazon.com/detective/%20) makes it easy to analyze, investigate, and quickly identify the root cause of potential security issues or suspicious activities. Amazon Detective automatically collects AWS service log data from your AWS resources and uses machine learning, statistical analysis, and graph theory to build a linked set of data that enables you to conduct faster and more efficient security investigations.

### Update security program with lessons learned
Documenting and cycling lessons learned during simulations and live incidents back into "new normal" processes and procedures allows organizations to better understand how they were breached -- such as where they were vulnerable, where automation may have failed, or where visibility was lacking -- and the opportunity to strengthen their overall security posture.

## Train your employees
Train your employees through an effective security awareness program. This needs to be made entertaining and engaging. Focusing on reporting phishing, notification procedures, and how to report "bad" things then awarding that behavior can be very effective.

## Conclusion
Ransom attacks are evolving, but so can your security awareness and preparedness. Government agencies, nonprofits, and businesses around the world trust AWS to power their infrastructure and keep their systems and data secure. Using the AWS services and best practices shared in this playbook, you can take proactive measures to reduce the likelihood and impact of ransom in your AWS environments.

## Appendix A: Services and Tools to Prevent or Detect Ransom Attacks
[Amazon CloudWatch Synthetics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries.html)  
[Amazon CloudWatch](https://aws.amazon.com/cloudwatch)  
[Amazon CodeGuru](https://aws.amazon.com/codeguru/)  
[Amazon EventBridge](https://aws.amazon.com/eventbridge/)  
[Amazon GuardDuty](https://aws.amazon.com/guardduty/)  
[Amazon Inspector Network Reachability](https://docs.aws.amazon.com/inspector/latest/userguide/inspector_network-reachability.html)  
[Amazon Inspector](https://aws.amazon.com/inspector/)  
[Amazon Macie](https://aws.amazon.com/macie/)  
[Amazon VPC Reachability Analyzer](https://docs.aws.amazon.com/vpc/latest/reachability/what-is-reachability-analyzer.html)  
[AWS CDK](https://aws.amazon.com/cdk/)  
[AWS Cloud9](https://aws.amazon.com/cloud9/)  
[AWS CloudFormation Guard](https://github.com/aws-cloudformation/cloudformation-guard)  
[AWS CloudFormation](https://aws.amazon.com/cloudformation/)  
[AWS CodeArtifact](https://aws.amazon.com/codeartifact/)  
[AWS CodeBuild](https://aws.amazon.com/codebuild/)  
[AWS CodeCommit](https://aws.amazon.com/codecommit/)  
[AWS CodeDeploy](https://aws.amazon.com/codedeploy/)  
[AWS CodePipeline](https://aws.amazon.com/codepipeline/)  
[AWS Config Rules](https://aws.amazon.com/blogs/aws/aws-config-rules-dynamic-compliance-checking-for-cloud-resources/)  
[AWS Control Tower](https://aws.amazon.com/controltower/)  
[AWS IAM Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html)  
[AWS IAM](https://aws.amazon.com/iam/)  
[AWS KMS](https://aws.amazon.com/kms/)  
[AWS Lambda](https://aws.amazon.com/lambda/)  
[AWS Organizations](https://aws.amazon.com/organizations/)  
[AWS Secrets Manager](https://aws.amazon.com/secrets-manager/)  
[AWS Security Hub](https://aws.amazon.com/security-hub/)  
[AWS Step Functions](https://aws.amazon.com/step-functions/)  
[AWS Systems Manager](https://aws.amazon.com/systems-manager/)  
[AWS WAF](https://aws.amazon.com/waf/)  
[AWSPX](https://github.com/FSecureLABS/awspx)  
[CheckMarx](https://checkmarx.com/)  
[EC2 Image Builder](https://aws.amazon.com/image-builder/)  
[ECR Native Container Image Scanning](https://aws.amazon.com/blogs/containers/amazon-ecr-native-container-image-scanning/)  
[git-secrets](https://github.com/awslabs/git-secrets)  
[PMapper](https://github.com/nccgroup/PMapper)  
[Prowler](https://github.com/toniblyx/prowler)  
[SonarQube](https://www.sonarqube.org/)  
