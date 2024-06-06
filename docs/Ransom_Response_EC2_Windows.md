# Incident Response Playbook: Ransom Response for EC2 Microsoft Windows
This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2024 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

## Points of Contact

Author: `Author Name`  
Approver: `Approver Name`  
Last Date Approved:   

## Executive Summary
This playbook outlines the process for responses to Ransom attacks against EC2 instances. 

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
4. [**DETECTION AND ANALYSIS**] Use procedures found in the Microsoft 365 Defender Threat Intelligence Team
5. [**DETECTION AND ANALYSIS**] Use CloudWatch metrics determine if data may have been exfiltrated
6. [**DETECTION AND ANALYSIS**] Use VPCFlowLogs to identify inappropriate database access from external IP addresses
7. [**CONTAINMENT AND ERADICATION**] Remove any compromised systems from the network.
8. [**CONTAINMENT AND ERADICATION**] Enforce NACLs based on network IoCs to prevent further traffic
9. [**CONTAINMENT AND ERADICATION**] Other Items of Interest
10. [**RECOVERY**] Execute recovery procedures as appropriate

***The response steps follow the Incident Response Life Cycle from [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)  

![Image](/images/nist_life_cycle.png)***

### Incident Classification & Handling
* **Tactics, techniques, and procedures**: Ransom & Data Destruction
* **Category**: Ransom Attack
* **Resource**: EC2
* **Indicators**: Cyber Threat Intelligence, Third Party Notice, Cloudwatch Metrics
* **Log Sources**: CloudTrail, CloudWatch, AWS Config
* **Teams**: Security Operations Center (SOC), Forensic Investigators, Cloud Engineering

## Incident Handling Process
### The incident response process has the following stages:
* Preparation
* Detection & Analysis
* Containment & Eradication
* Recovery
* Post-Incident Activity

## Preparation
* Assess the security posture of the account to identify and remediate security gaps 
    *  AWS developed a new open source Self-Service Security Assessment (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) tool that provides customers with a point-in-time assessment to gain valuable insights into the security posture of their AWS account
* To protect your organization, Microsoft recommends that you use the information in the [Human-Operated Ransomware Mitigation Project Plan](https://download.microsoft.com/download/7/5/1/751682ca-5aae-405b-afa0-e4832138e436/RansomwareRecommendations.pptx) PowerPoint presentation, which includes [securing privileged access](https://aka.ms/spa)
* Maintain a complete asset inventory of all resources including domain controllers, Microsoft Windows EC2 instances, Microsoft Windows Servers and Databases, and any integration with external identity providers
* Perform recurring vulnerability analysis of your hosts using utilities such as [Amazon Inspector](https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/)
* Turn on and update Windows Defender Antivirus - commercial, subscription paid Endpoint Detection and Response (EDR) solutions are preferred
* Perform backups of EC2 instances
    * Consider using [AWS Backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) or [AWS CloudEndure](https://aws.amazon.com/cloudendure-disaster-recovery/)
* [Back up your files with File History](https://support.microsoft.com/en-us/windows/file-history-in-windows-5de0e203-ebae-05ab-db85-d5aa0a199255)
* Verify your backups and ensure the infection has not spread into them
* Back up important files regularly. Use the 3-2-1 rule. Keep three backups of your data, on two different storage types, and at least one backup offsite
* Apply the latest updates to your operating systems and apps
* Educate your employees so they can identify social engineering and spear-phishing attacks
* Implement [controlled folder access](https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/controlled-folders). It can stop ransomware from encrypting files and holding the files for ransom
* [Block Macros in Office Documents](https://support.microsoft.com/en-us/topic/enable-or-disable-macros-in-office-files-12b036fd-d140-4e74-b45e-16fed1a7e5c6)
* Block Known Ransomware File Types
* In Windows 10 turn on [Controlled Folder Access](https://support.microsoft.com/en-us/topic/ransomware-protection-in-windows-security-445039d6-537a-488a-ad53-48906f346363) to protect your important local folders from unauthorized programs like ransomware or other malware 
* Follow [Best Practices for securing your Active Directory Services](https://www.calcomsoftware.com/how-to-secure-your-active-directory-when-anonymous-users-must-have-access/)
* Follow [Top 10 Most Important Group Policy Settings for Preventing Security Breaches](https://www.lepide.com/blog/top-10-most-important-group-policy-settings-for-preventing-security-breaches/)
* The Microsoft 365 Defender Threat Intelligence Team has provided a [comprehensive report](https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) identifying actions to secure your Microsoft resources prior to a ransomware event
    * Harden internet-facing assets and ensure they have the latest security updates. Use threat and vulnerability management to audit these assets regularly for vulnerabilities, misconfigurations, and suspicious activities.
    * Secure Remote Desktop Gateway using solutions like Azure Multi-Factor Authentication (MFA). If you don’t have an MFA gateway, enable network-level authentication (NLA)
    * Practice the principle of least-privilege and maintain credential hygiene. Avoid the use of domain-wide, admin-level service accounts. Enforce strong randomized, just-in-time local administrator passwords
    * Monitor for brute-force attempts. Check excessive failed authentication attempts (Windows security event ID 4625)
    * Monitor for clearing of Event Logs, especially the Security Event log and PowerShell Operational logs. Microsoft Defender ATP raises the alert “Event log was cleared” and Windows generates an Event ID 1102 when this occurs
    * Turn on tamper protection features to prevent attackers from stopping security services
    * Determine where highly privileged accounts are logging on and exposing credentials. Monitor and investigate logon events (event ID 4624) for logon type attributes. Domain admin accounts and other accounts with high privilege should not be present on workstations
    * Turn on cloud-delivered protection and automatic sample submission on Windows Defender Antivirus. These capabilities use artificial intelligence and machine learning to quickly identify and stop new and unknown threats
    * Turn on attack surface reduction rules, including rules that block credential theft, ransomware activity, and suspicious use of PsExec and WMI. To address malicious activity initiated through weaponized Office documents, use rules that block advanced macro activity, executable content, process creation, and process injection initiated by Office applications. To assess the impact of these rules, deploy them in audit mode
    * Turn on AMSI for Office VBA if you have Office 365
    * Utilize the Windows Defender Firewall and your network firewall to prevent RPC and SMB communication among endpoints whenever possible. This limits lateral movement as well as other attack activities
* Use [Systems Manager and Amazon Inspector](https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/) to check if EC2 instances running Microsoft Windows contain any common vulnerabilities and exposures (CVEs)

### Use AWS Config to view configuration compliance:
1. Sign in to the AWS Management Console and open the AWS Config console at https://console.aws.amazon.com/config/
1. In the AWS Management Console menu, verify that the region selector is set to a region that supports AWS Config rules. For the list of supported regions, see AWS Config Regions and Endpoints in the Amazon Web Services General Reference
1. In the navigation pane, choose Resources. On the Resource inventory page, you can filter by resource category, resource type, and compliance status. Choose Include deleted resources if appropriate. The table displays the resource identifier for the resource type and the resource compliance status for that resource. The resource identifier might be a resource ID or a resource name
1. Choose a resource from the resource identifier column
1. Choose the Resource Timeline button. You can filter by Configuration events, Compliance events, or CloudTrail Events
1. Specifically focus on the following events:
    * ebs-in-backup-plan
    * ebs-optimized-instance
    * ebs-snapshot-public-restorable-check
    * ec2-ebs-encryption-by-default
    * ec2-imdsv2-check 
    * ec2-instance-detailed-monitoring-enabled 
    * ec2-instance-managed-by-systems-manager
    * ec2-instance-multiple-eni-check 
    * ec2-instance-no-public-ip 
    * ec2-instance-profile-attached
    * ec2-managedinstance-applications-blacklisted
    * ec2-managedinstance-applications-required 
    * ec2-managedinstance-association-compliance-status-check 
    * ec2-managedinstance-inventory-blacklisted
    * ec2-managedinstance-patch-compliance-status-check
    * ec2-managedinstance-platform-check
    * ec2-security-group-attached-to-eni
    * ec2-stopped-instance
    * ec2-volume-inuse-check

## Escalation Procedures
- `I need a business decision on when EC2 forensics should be conducted`
- `Who is monitoring the logs/alerts, receiving them and acting upon each?`
- `Who gets notified when an alert is discovered?`
- `When do public relations and legal get involved in the process?`
- `When would you reach out to AWS Support for help?`

## Detection and Analysis
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

### The Microsoft 365 Defender Threat Intelligence Team 
The team has provided a comprehensive report (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) identifying things to look for against multiple ransomware variants

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

## Containment
### Isolate Affected Resources Immediately
**NOTE**: Ensure you have a process in place to request escalation and approval to isolate resources to ensure a business impact analysis is conducted first on how the isolation will impact current operations and revenue streams.
1. Determine if the instance is part of an Auto Scaling group or attached to a load balancer
    * Autoscaling Group: detach the instance from the group
    * Elastic Load Balancer: deregister the instance from the ELB, and delete the instance from target groups
1. Create a *new* security group that blocks all ingress and egress traffic; ensure you remove the default `allow all` rule for egress traffic
1. Attach the *new* security group to the impacted instance(s)

### Enforce NACLs based on network IoCs to prevent further traffic
1. Open the [Amazon VPC console](https://console.aws.amazon.com/vpc/)
1. In the navigation pane, choose Network ACLs
1. Choose Create Network ACL
1. In the Create Network ACL dialog box, optionally name your network ACL, and select the ID of your VPC from the VPC list. Then choose Yes, Create
1. In the details pane, choose either the Inbound Rules or Outbound Rules tab, depending on the type of rule that you need to add, and then choose Edit
1. In Rule #, enter a rule number (for example, 100). The rule number must not already be in use in the network ACL. We process the rules in order, starting with the lowest number
    * We recommend that you leave gaps between the rule numbers (such as 100, 200, 300), rather than using sequential numbers (101, 102, 103). This makes it easier add a new rule without having to renumber the existing rules
1. Select a rule from the Type list. For example, to add a rule for HTTP, choose HTTP. To add a rule to allow all TCP traffic, choose All TCP. For some of these options (for example, HTTP), we fill in the port for you. To use a protocol that's not listed, choose Custom Protocol Rule
1. (Optional) If you're creating a custom protocol rule, select the protocol's number and name from the Protocol list. For more information, see IANA List of Protocol Numbers
1. (Optional) If the protocol you selected requires a port number, enter the port number or port range separated by a hyphen (for example, 49152-65535).
1. In the Source or Destination field (depending on whether this is an inbound or outbound rule), enter the CIDR range that the rule applies to
1. From the Allow/Deny list, select ALLOW to allow the specified traffic or DENY to deny the specified traffic
1. (Optional) To add another rule, choose Add another rule, and repeat the previous steps as required
1. When you are done, choose Save
1. In the navigation pane, choose Network ACLs, and then select the network ACL
1. In the details pane, on the Subnet Associations tab, choose Edit. Select the Associate check box for the subnet to associate with the network ACL, and then choose Save

## Eradication
### Remove any compromised systems from the network.
* Follow regulatory requirements or internal company policy to determine if forensics of the EC2 instance is required
* If forensics of the instances are required OR data needs to be recovered, then follow the [Playbook: EC2 Forensics](./docs/EC2_Forensics.md)

### Other Items of Interest
*  [Remove compromised Domain Controller metadata from the domain](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/ad-ds-metadata-cleanup)
* Inspect backups for potential infection

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
* Create new EC2 instances from a trusted AMI
* Use CloudEndure Disaster Recovery to select the latest recovery point before the ransomware attack or data corruption to restore your workloads on AWS
    * If using an alternate data backup strategy, validate the backups have not been infected and restore from the last scheduled event prior to the ransomware event

## Lessons Learned
`This is a place to add items specific to your company that do not necessarilly need "fixing", but are important to know when executing this playbook in tandem with operational and business requirements.`

## Addressed Backlog Items
- As an Incident Responder I need a runbook to conduct EC2 Forensics
- As an Incident Responder I need a business decision on when EC2 forensics should be conducted
- As an Incident Responder I need to have logging enabled in all regions that are enabled regardless of intention of use
- As an Incident Responder I need to be able to detect crypto mining on my existing EC2 instances

## Current Backlog Items
