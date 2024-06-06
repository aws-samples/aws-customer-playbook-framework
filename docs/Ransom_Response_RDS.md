# Incident Response Playbook: Ransom Response for RDS
This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2024 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

## Points of Contact

Author: `Author Name`   
Approver: `Approver Name`  
Last Date Approved:   

## Executive Summary
This playbook outlines the process for responses to Ransom attacks against Amazon Relational Database Service (RDS) 

For additional information, please review the [AWS Security Incident Response Guide](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

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
3. [**CONTAINMENT**] Isolate Affected Resources Immediately
5. [**DETECTION AND ANALYSIS**] Use CloudWatch metrics determine if data may have been exfiltrated
6. [**DETECTION AND ANALYSIS**] Use VPCFlowLogs to identify inappropriate database access from external IP addresses
7. [**RECOVERY**] Execute recovery procedures as appropriate

***The response steps follow the Incident Response Life Cycle from [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)  

![Image](/images/nist_life_cycle.png)***

### Incident Classification & Handling
* **Tactics, techniques, and procedures**: Ransom & Data Destruction
* **Category**: Ransom Attack
* **Resource**: RDS
* **Indicators**: Cyber Threat Intelligence, Third Party Notice, Cloudwatch Metrics
* **Log Sources**: RDS Database Log Files, S3 Access Logs, CloudTrail, CloudWatch, AWS Config
* **Teams**: Security Operations Center (SOC), Forensic Investigators, Cloud Engineering

## Incident Handling Process
### The incident response process has the following stages:
* Preparation
* Detection & Analysis
* Containment & Eradication
* Recovery
* Post-Incident Activity

## Preparation
* Assess your security posture to identify and remediate security gaps 
    *  AWS developed a new open source Self-Service Security Assessment (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) tool that provides customers with a point-in-time assessment to gain valuable insights into the security posture of their AWS account.
* Maintain a complete asset inventory of all resources including servers, networking devices, network/file shares and developer machines
* Consider using [AWS Config](https://aws.amazon.com/config/), which is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources
* Consider implementing [AWS GuardDuty](https://aws.amazon.com/guardduty/) to continuously monitor for malicious activity and unauthorized behavior to protect your AWS accounts, workloads, and data stored in Amazon RDS
* [Run your DB instance in a virtual private cloud (VPC)](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.html) based on the Amazon VPC service for the greatest possible network access control
* Use AWS Identity and Access Management (IAM) policies to assign permissions that determine who is allowed to manage Amazon RDS resources. For example, you can use IAM to determine who is allowed to create, describe, modify, and delete DB instances, tag resources, or modify security groups. 
* Use security groups to control what IP addresses or Amazon EC2 instances can connect to your databases on a DB instance. When you first create a DB instance, its firewall prevents any database access except through rules specified by an associated security group. 
* [Use Secure Socket Layer (SSL) or Transport Layer Security (TLS) connections](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html) with DB instances running the MySQL, MariaDB, PostgreSQL, Oracle, or Microsoft SQL Server database engines 
* [Use Amazon RDS encryption](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Encryption.html) to secure your DB instances and snapshots at rest. Amazon RDS encryption uses the industry standard AES-256 encryption algorithm to encrypt your data on the server that hosts your DB instance 
* [Use network encryption](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.NetworkEncryption.html) and [transparent data encryption](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.AdvSecurity.html) with Oracle DB instances
* Use the security features of your DB engine to control who can log in to the databases on a DB instance. These features work just as if the database was on your local network. 
* It is highly recommended to have a third-party host-based intrusion detection system (HIDS) solution (such as OSSEC, Tripwire, Wazuh, [Amazon Inspector](https://aws.amazon.com/inspector/))
* Additional references and steps are available in [Security in Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.html)

### Use AWS Config to view configuration compliance:
1. Sign in to the AWS Management Console and open the AWS Config console at https://console.aws.amazon.com/config/
1. In the AWS Management Console menu, verify that the region selector is set to a region that supports AWS Config rules. For the list of supported regions, see AWS Config Regions and Endpoints in the Amazon Web Services General Reference
1. In the navigation pane, choose Resources. On the Resource inventory page, you can filter by resource category, resource type, and compliance status. Choose Include deleted resources if appropriate. The table displays the resource identifier for the resource type and the resource compliance status for that resource. The resource identifier might be a resource ID or a resource name
1. Choose a resource from the resource identifier column
1. Choose the Resource Timeline button. You can filter by Configuration events, Compliance events, or CloudTrail Events
1. Specifically focus on the following events:
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
    * rds-snapshots-public-prohibited
    * rds-snapshot-encrypted
    * rds-storage-encrypted

## Escalation Procedures
- `I need a business decision on when EC2 forensics should be conducted`
- `Who is monitoring the logs/alerts, receiving them and acting upon each?`
- `Who gets notified when an alert is discovered?`
- `When do public relations and legal get involved in the process?`
- `When would you reach out to AWS Support for help?`

## Detection and Analysis
* [AWS GuardDuty machine learning](https://aws.amazon.com/blogs/security/how-you-can-use-amazon-guardduty-to-detect-suspicious-activity-within-your-aws-account/) can detect suspicious behavior including  Generating Amazon Relational Database Service (Amazon RDS) snapshots as part of data exfiltration attempts
* Review EC2 operating system and application logs for inappropriate logins, installation of unknown software, or the presence of unrecognized files

### [Use CloudWatch metrics](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html)
Look for data exfiltration “spikes.” It is possible an attacker performed data destruction and left a ransom note, and in these cases there is no opportunity for data recovery by working with the malicious actor
1. Open the CloudWatch console at https://console.aws.amazon.com/cloudwatch/
1. In the navigation pane, choose Metrics then All Metrics
1. On the All metrics tab, select the region the instance is deployed in
1. On the All metrics tab, enter the search term `NetworkPacketsOut` and press Enter
1. Select one of the results for your search to view the metrics
1. To graph one or more metrics, select the check box next to each metric. To select all metrics, select the check box in the heading row of the table
1. (Optional) To change the type of graph, choose Graph options. You can then choose between a line graph, stacked area chart, bar chart, pie chart, or number
1. (Optional) To add an anomaly detection band that shows expected values for the metric, choose the anomaly detection icon under Actions next to the metric

### Use VPCFlowLogs to identify inappropriate database access from external IP addresses
1. Use the [AWS Security Analytics Bootstrap](https://github.com/awslabs/aws-security-analytics-bootstrap) to analyze log data
1. Get a summary with the number of bytes for each src_ip,src_port,dst_ip,dst_port quad across all records to or from a specific IP
```
SELECT sourceaddress, destinationaddress, sourceport, destinationport, sum(numbytes) as byte_count FROM vpcflow
WHERE (sourceaddress = '192.0.2.1' OR destinationaddress = '192.0.2.1')
AND date_partition >= '2020/07/01'
AND date_partition <= '2020/07/31'
AND account_partition = '111122223333'
AND region_partition in ('us-east-1','us-east-2','us-west-2', 'us-west-2')
GROUP BY sourceaddress, destinationaddress, sourceport, destinationport
ORDER BY byte_count DESC
```
1. Other example queries are provided in the [vpcflow_demo_queries.sql](https://github.com/awslabs/aws-security-analytics-bootstrap/blob/main/AWSSecurityAnalyticsBootstrap/sql/dml/analytics/vpcflow/vpcflow_demo_queries.sql)


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
* [Revoke temporary credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Temporary credentials can also be revoked by deleting the IAM User. NOTE: Deleting IAM Users may impact production workloads and should be done with care 
* Use CloudEndure Disaster Recovery to select the latest recovery point before the ransomware attack or data corruption to restore your workloads on AWS
    * If using an alternate data backup strategy, validate the backups have not been infected and restore from the last scheduled event prior to the ransomware event
* Delete any unrecognized or unauthorized public snapshots or databases 
* Delete the compromised database and create new RDS databases
* Identify an EC2 instances that had permissive access to the database(s) and investigate those as well

## Lessons Learned
`This is a place to add items specific to your company that do not necessarilly need "fixing", but are important to know when executing this playbook in tandem with operational and business requirements.`

## Addressed Backlog Items
- As an Incident Responder I need a runbook to conduct EC2 Forensics
- As an Incident Responder I need a business decision on when EC2 forensics should be conducted
- As an Incident Responder I need to have logging enabled in all regions that are enabled regardless of intention of use
- As an Incident Responder I need to be able to detect crypto mining on my existing EC2 instances

## Current Backlog Items

