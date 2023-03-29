# Incident Response Playbook: Public Resources Exposure - RDS
This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2021 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

## Points of Contact

Author: `Author Name` \
Approver: `Approver Name` \
Last Date Approved:   

## Executive Summary
This playbook outlines the process to identify owners of public resources, determine who may have accessed those while they were exposed, determine impact of revoking access to the resource, and determine root cause of public accessibility.

## Potential Indicators of Compromise
- Public Access warning from AWS service dashboard
- CloudTrail event name `PubliclyAccessible`
- Notification from Security Researcher about public access to resources
- Deletion of resources from public internet protocol (IP) address

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
1. [**PREPARATION**] Create an Asset Inventory
2. [**PREPARATION**] Create an RDS Instance Inventory
3. [**PREPARATION**] Establish RDS Security and Logging Checks
4. [**PREPARATION**] Implement a training program to identify and assess RDS access and log analysis
5. [**PREPARATION**] Identify, document, and test escalation Procedures[**DETECTION AND ANALYSIS**] 
6. [**DETECTION AND ANALYSIS**] Perform Instance Checks
7. [**DETECTION AND ANALYSIS**] Review CloudTrail for RDS Public Resources
8. [**DETECTION AND ANALYSIS**] Review VPC Flow Logs
9. [**DETECTION AND ANALYSIS**] Review RDS Endpoint / Host Based Logs 
10. [**CONTAINMENT**] Contain RDS Public Exposure
11. [**ERADICATION**] Delete any unrecognized or unauthorized public snapshots or databases
12. [**PREPARATION**] Additional Preventative Actions: RDS Security Checks
13. [**PREPARATION**] Additional Preventative Actions: Security Control Policy - RDS Encryption
14. [**PREPARATION**] Additional Preventative Actions: AWS Config
15. [**PREPARATION**] Additional Preventative Actions: Overall Security Posture

***The response steps follow the Incident Response Life Cycle from [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)  

![Image](/images/nist_life_cycle.png)***

### Incident Classification & Handling
* **Tactics, techniques, and procedures**: AWS Service Public Access
* **Category**: Public Access
* **Resource**: RDS
* **Indicators**: Cyber Threat Intelligence, Third Party Notice, Cloudwatch Metrics
* **Log Sources**: RDS Database Log Files, CloudTrail, CloudWatch
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

This tool provides a rapid snapshot of the current state of security within a customer environment. As an alternative, [AWS Security Hub](https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) provides for automated compliance scanning and can [integrate with Prowler](https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Asset Inventory
Identify all existing resources and have an updated asset inventory list coupled with who owns each

#### RDS Instance Inventory
- To inventory RDS instances, use the AWS API [describe-db-instances](https://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-instances.html) to list the names of all instances in a given region: `aws rds describe-db-instances --region us-east-1 --query 'DBInstances[*].[DBInstanceIdentifier,ReadReplicaDBInstanceIdentifiers]'`

#### RDS Security and Logging Checks
- Check if RDS instances storage is encrypted: `./prowler -c extra735`
- Check if RDS instances have backup enabled: `./prowler -c extra739`
- Check if RDS instances is integrated with CloudWatch Logs: `./prowler -c extra747`
- Ensure RDS instances have minor version upgrade enabled: `./prowler -c extra7131`
- Check if RDS instances has [enhanced monitoring](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Monitoring.OS.html) enabled: `./prowler -c extra7132`
- RDS Security Checks: `./prowler -g group13`

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

### RDS Instance Checks

#### AWS Config
AWS Config has several [managed rules to evaluate your RDS instances](https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
* rds-automatic-minor-version-upgrade-enabled
* rds-cluster-deletion-protection-enabled
* rds-cluster-iam-authentication-enabled
* rds-cluster-multi-az-enabled
* rds-enhanced-monitoring-enabled
* rds-instance-deletion-protection-enabled
* rds-instance-iam-authentication-enabled
* rds-instance-public-access-check
* rds-in-backup-plan
* rds-logging-enabled
* rds-multi-az-support
* rds-resources-protected-by-backup-plan
* rds-snapshots-public-prohibited
* rds-snapshot-encrypted
* rds-storage-encrypted
#### Prowler
- Ensure there are no Public Accessible RDS instances: `./prowler -c extra78`
- Check if RDS Snapshots and Cluster Snapshots are public: `./prowler -c extra723`
- Identify the resources in your organization and accounts; such as Amazon S3 buckets or IAM roles; that are shared with an external entity: `./prowler -c extra769`
- Find resources exposed to the internet: `./prowler -g group17`

## Escalation Procedures
- `Who is monitoring the logs/alerts, receiving them and acting upon each?`
- `Who gets notified when an alert is discovered?`
- `When do public relations and legal get involved in the process?`
- `When would you reach out to AWS Support for help?`

## Analysis
It is highly recommended to export logs to a security incident event management (SIEM) solution (such as Splunk, ELK stack, etc.) to aide in viewing and analyzing a variety of logs for a more complete attack timeline analysis.

### CloudTrail: RDS Public
1. Navigate to your [CloudTrail Dashboard](https://console.aws.amazon.com/cloudtrail)
1. In the left-hand margin select `Event History`
1. In the drop-down change from `Read-Only` to `Event Name`
1. Review CloudTrail logs for the eventname `ModifyDBInstance`, `ModifyDBSnapshotAttribute`, or `ModifyDBClusterSnapshotAttribute` and look for the value `PubliclyAccessible` events from public IP addresses

### VPC Flow Logs
VPC Flow Logs is a feature that enables you to capture information about the IP traffic going to and from network interfaces in your VPC. This can be useful for IP addresses discovered within CloudTrail to determine the types of external connections to any public resources. 

For further information and steps, including querying with Athena, please refer to the [AWS Documentation for VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html). It is recommended that Athena analysis be included in a separate playbook and linked to other relavent items. 

### Endpoint / Host Based
- Review EC2 operating system and application logs for inappropriate logins, installation of unknown software, or the presence of unrecognized files. 

- It is highly recommended to have a third-party host-based intrusion detection system (HIDS) solution (such as OSSEC, Tripwire, Wazuh, [Amazon Inspector](https://aws.amazon.com/inspector/), other) 

## Containment

### RDS Public Exposure
When you launch a DB instance inside a VPC, the DB instance has a private IP address for traffic inside the VPC. This private IP address isn't publicly accessible. You can use the Public access option to designate whether the DB instance also has a public IP address in addition to the private IP address. 

The following illustration shows the Public access option in the Additional connectivity configuration section. To set the option, open the Additional connectivity configuration section in the Connectivity section. 

![Image](/images/VPC-example.png)

## Eradication

### RDS Unauthorized / Unrecognized Resources
Delete any unrecognized or unauthorized public snapshots or databases

1. Sign in to the AWS Management Console and open the Amazon RDS console at https://console.aws.amazon.com/rds/
1. In the navigation pane, choose Snapshots.
1. The Manual snapshots list appears.
1. Choose the DB snapshot that you want to delete.
1. For Actions, choose Delete Snapshot.
1. Choose Delete on the confirmation page.

## Recovery
Same procedures as those listed for Eradication

## Preventative Actions

### RDS Security Checks
- RDS Security Checks: `./prowler -g group13`

### Security Control Policy: RDS Encryption
[Enforce Mandatory RDS Encryption](https://medium.com/@cbchhaya/aws-scp-to-mandate-rds-encryption-6b4dc8b036a)

### AWS Config
[AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) has multiple automated rules to protect against public access including [rds-snapshots-public-prohibited](https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshots-public-prohibited.html)

### Overall Security Posture
Execute a [Self-Service Security Assessment](https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) against the environment to further identify other risks and potentially other public exposure not identified throughout this playbook.

## Lessons Learned
`This is a place to add items specific to your company that do not necessarilly need "fixing", but are important to know when executing this playbook in tandem with operational and business requirements.`

## Addressed Backlog Items
- As a Incident Responder I need a runbook on how to mitigate resources that are incorrectly made public
- As a Incident Responder I need to be able to detect public resources(AMIs, EBS Volumes, ECR Repos, etc)
- As a Incident Responder I need to know which roles are capable of making critical changes within AWS
- As a Incident Responder I need to ensure that all snapshots (RDS, EBS, ECR) require encryption

## Current Backlog Items
