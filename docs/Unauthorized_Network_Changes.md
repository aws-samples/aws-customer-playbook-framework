# Incident Response Playbook: Unauthorized Network Changes
This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2021 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

[[_TOC_]]

## Points of Contact

Author: `Author Name` \
Approver: `Approver Name` \
Last Date Approved:   

## Executive Summary
This playbook outlines the process for responding to unauthorized or suspicious changes to network assets within an AWS environment.

## Potential Indicators of Compromise
- New or unrecognized service resources
- Unrecognized or unauthorized resources (e.g. EC2, Lambda)
- Unusual billing increases 
- Notification from security researcher
- Notification that my AWS resources or account might be compromised

## Potential AWS GuardDuty Findings
- Backdoor:EC2/C&CActivity.B
- Backdoor:EC2/C&CActivity.B!DNS
- Backdoor:EC2/Spambot
- Behavior:EC2/NetworkPortUnusual
- Behavior:EC2/TrafficVolumeUnusual
- CryptoCurrency:EC2/BitcoinTool.B
- CryptoCurrency:EC2/BitcoinTool.B!DNS
- Impact:EC2/AbusedDomainRequest.Reputation
- Impact:EC2/BitcoinDomainRequest.Reputation
- Impact:EC2/MaliciousDomainRequest.Reputation
- Impact:EC2/SuspiciousDomainRequest.Reputation
- Impact:EC2/WinRMBruteForce
- Recon:EC2/PortProbeEMRUnprotectedPort
- Recon:EC2/PortProbeUnprotectedPort
- Trojan:EC2/BlackholeTraffic
- Trojan:EC2/BlackholeTraffic!DNS
- Trojan:EC2/DGADomainRequest.B
- Trojan:EC2/DGADomainRequest.C!DNS
- Trojan:EC2/DNSDataExfiltration
- Trojan:EC2/DriveBySourceTraffic!DNS
- Trojan:EC2/DropPoint
- Trojan:EC2/DropPoint!DNS
- Trojan:EC2/PhishingDomainRequest!DNS
- UnauthorizedAccess:EC2/MaliciousIPCaller.Custom
- UnauthorizedAccess:EC2/MetadataDNSRebind
- UnauthorizedAccess:EC2/RDPBruteForce
- UnauthorizedAccess:EC2/SSHBruteForce
- UnauthorizedAccess:EC2/TorClient
- UnauthorizedAccess:EC2/TorRelay
- Discovery:S3/MaliciousIPCaller
- CredentialAccess:IAMUser/AnomalousBehavior
- DefenseEvasion:IAMUser/AnomalousBehavior
- Discovery:IAMUser/AnomalousBehavior
- Exfiltration:IAMUser/AnomalousBehavior
- Impact:IAMUser/AnomalousBehavior
- InitialAccess:IAMUser/AnomalousBehavior
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

![Image](images/aws_caf.png)
* * *

### Response Steps
1. [**PREPARATION**] Publicly Exposed Asset Inventory
2. [**PREPARATION**] Setup Logging
3. [**PREPARATION**] Implement a training program to identify and assess cloud forensics capabilities
4. [**PREPARATION**] Identify, document, and test escalation Procedures
5. [**DETECTION AND ANALYSIS**] Review CloudTrail
6. [**DETECTION AND ANALYSIS**] Review CloudWatch
7. [**DETECTION AND ANALYSIS**] Use AWS Config to check the configuration of a security group
8. [**DETECTION AND ANALYSIS**] Viewing VPC Flow Logs in Console
9. [**DETECTION AND ANALYSIS**] Querying flow logs using Amazon Athena
10. [**CONTAINMENT**] Identify what user or role executed the unauthorized network changes
11. [**ERADICATION**] Review the findings from Review CloudTrail event history for activity by the compromised access key
12. [**ERADICATION**] Review the Avoiding unexpected charges
13. [**RECOVERY**] Revert any changes that occurred to network resources
14. [**PREPARATION**] Additional Preventative Actions: AWS Config and AWS System Manager
15. [**PREPARATION**] Additional Preventative Actions: AWS Security Hub
16. [**PREPARATION**] Additional Preventative Actions: Overall Security Posture

***The response steps follow the Incident Response Life Cycle from NIST Special Publication 800-61r2
[NIST Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)
![Image](images/nist_life_cycle.png)***

### Incident Classification & Handling
* **Tactics, techniques, and procedures**: Tools: Forensics
* **Category**: Forensics
* **Resource**: EC2, VPC
* **Indicators**: Cyber Threat Intelligence, Third Party Notice, Cloudwatch Metrics
* **Log Sources**: Endpoint
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

### Publicly Exposed Asset Inventory
You can use the [VPC Dashboard](https://console.aws.amazon.com/vpc/home) and the [Network Manager Dashboard](https://console.aws.amazon.com/networkmanager/home) which enable you to visualize and monitor your network resources. The Network Manager Dashboard includes information about the resources in your global network, their geographic location, the network topology, Amazon CloudWatch metrics and events, and enables you to perform route analysis.

You should maintain an inventory of and monitor changes to the following resources: 
- Virtual Private Clouds (VPCs)
- VPC Network Access Control Lists (NACLs)
- VPC Security Groups
- VPC Route Tables
- VPC Elastic Network Interfaces (ENIs)
- VPC Internet Gateways
- VPC Peering Connections
- VPC NAT Gateways
- VPC Endpoints
- VPN Customer Gateways
- VPC Elastic IP Addresses (EIPs)

Prowler can also help you find many of the resources exposed to the internet: `./prowler -g group17`

### Setup Logging
**Flow logs:**
- Flow logs capture information about the IP traffic going to and from network interfaces in your VPC. 
- You can create a flow log for a VPC, subnet, or individual network interface. 
- Flow log data is published to CloudWatch Logs or Amazon S3, and can help you diagnose overly restrictive or overly permissive security group and network ACL rules. 

**To create a flow log for a network interface using the console**
1. Open the Amazon [EC2 console](https://console.aws.amazon.com/ec2/)
1. In the navigation pane, choose Network Interfaces.
1. Select the checkboxes for one or more network interfaces and choose Actions, Create flow log.
1. For Filter, specify the type of traffic to log. 
    - Choose All to log accepted and rejected traffic, Reject to log only rejected traffic, or Accept to log only accepted traffic.
1. For Maximum aggregation interval, choose the maximum period of time during which a flow is captured and aggregated into one flow log record.
1. For Destination, choose Send to an Amazon S3 bucket.
1. For S3 bucket ARN, specify the Amazon Resource Name (ARN) of an existing Amazon S3 bucket. 
    - You can include a subfolder in the bucket ARN. The bucket cannot use AWSLogs as a subfolder name, as this is a reserved term.
    - For example, to specify a subfolder named my-logs in a bucket named my-bucket, use the following ARN: `arn:aws:s3:::my-bucket/my-logs/`
    - If you own the bucket, we automatically create a resource policy and attach it to the bucket. For more information, see Amazon S3 bucket permissions for flow logs.
1. For Log record format, select the format for the flow log record.
    * To use the default format, choose AWS default format.
    * To use a custom format, choose Custom format and then select fields from Log format.
1. Choose Create flow log.

**To create a flow log for a VPC or a subnet using the console**
1. Open the Amazon [VPC console](https://console.aws.amazon.com/vpc/)
1. In the navigation pane, choose Your VPCs or choose Subnets.
1. Select the checkbox for one or more VPCs or subnets and then choose Actions, Create flow log.
1. For Filter, specify the type of traffic to log. 
    - Choose All to log accepted and rejected traffic, Reject to log only rejected traffic, or Accept to log only accepted traffic.
1. For Maximum aggregation interval, choose the maximum period of time during which a flow is captured and aggregated into one flow log record.
1. For Destination, choose Send to an Amazon S3 bucket.
1. For S3 bucket ARN, specify the Amazon Resource Name (ARN) of an existing Amazon S3 bucket. 
    - You can include a subfolder in the bucket ARN. The bucket cannot use AWSLogs as a subfolder name, as this is a reserved term.
    - For example, to specify a subfolder named my-logs in a bucket named my-bucket, use the following ARN: `arn:aws:s3:::my-bucket/my-logs/`
    - If you own the bucket, we automatically create a resource policy and attach it to the bucket. For more information, see Amazon S3 bucket permissions for flow logs.
1. For Log record format, select the format for the flow log record.
    * To use the default format, choose AWS default format.
    * To use a custom format, choose Custom format and then select fields from Log format.
1. Choose Create flow log.

**Monitoring NAT gateways:** You can monitor your NAT gateway using CloudWatch, which collects information from your NAT gateway and creates readable, near real-time metrics. 
1. NAT gateway metrics are sent to CloudWatch at 1-minute intervals. 
1.  You can view the metrics for your NAT gateways as follows.
    * Open the CloudWatch console at https://console.aws.amazon.com/cloudwatch/
    * In the navigation pane, choose Metrics.
    * Under All metrics, choose the NAT gateway metric namespace.
    * To view the metrics, select the metric dimension.
1. To create an alarm for outbound traffic through the NAT gateway:
    * Open the CloudWatch console at https://console.aws.amazon.com/cloudwatch/
    * In the navigation pane, choose Alarms, Create Alarm.
    * Choose NAT gateway.
    * Select the NAT gateway and the BytesOutToDestination metric and choose Next.
    * Configure the alarm as follows, and choose Create Alarm when you are done:
    * Under Alarm Threshold, enter a name and description for your alarm. For Whenever, choose >= and enter 5000000. Enter 1 for the consecutive periods.
    * Under Actions, select an existing notification list or choose New list to create a new one.
    * Under Alarm Preview, select a period of 15 minutes and specify a statistic of Sum.
1. To create an alarm to monitor port allocation errors
    * Open the CloudWatch console at https://console.aws.amazon.com/cloudwatch/
    * In the navigation pane, choose Alarms, Create Alarm.
    * Choose NAT Gateway.
    * Select the NAT gateway and the ErrorPortAllocation metric and choose Next.
    * Configure the alarm as follows, and choose Create Alarm when you are done:
    * Under Alarm Threshold, enter a name and description for your alarm. For Whenever, choose > and enter 0. Enter 3 for the consecutive periods.
    * Under Actions, select an existing notification list or choose New list to create a new one.
    * Under Alarm Preview, select a period of 5 minutes and specify a statistic of Maximum.

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

### CloudTrail
1. Navigate to your [CloudTrail Dashboard](https://console.aws.amazon.com/cloudtrail)
1. In the left-hand margin select `Event History`
1. In the drop-down change from `Read-Only` to `Event Name`
1. Review CloudTrail logs for the eventnames `AuthorizeSecurityGroupIngress`, `RevokeSecurityGroupIngress`, `AuthorizeSecurityGroupEgress` and `RevokeSecurityGroupEgress`

### CloudWatch
Create a CloudWatch Events rule that triggers on an event using the CloudWatch console

1. Open the CloudWatch console.
1. In the navigation pane, choose Rules under Events, and then choose Create rule.
1. Select the Event Pattern.
1. For Service Name, choose EC2.
1. For Event Type, choose AWS API Call via CloudTrail.
1. Choose Specific Operation and provide the following API calls. These API calls are used to add or remove security group rules.
    - AuthorizeSecurityGroupIngress
    - AuthorizeSecurityGroupEgress
    - RevokeSecurityGroupIngress
    - RevokeSecurityGroupEgress
1. These settings create the following event pattern.
    ```json
    {
    "source": [
        "aws.ec2"
    ],
    "detail-type": [
        "AWS API Call via CloudTrail"
    ],
    "detail": {
        "eventSource": [
        "ec2.amazonaws.com"
        ],
        "eventName": [
        "AuthorizeSecurityGroupIngress",
        "AuthorizeSecurityGroupEgress",
        "RevokeSecurityGroupIngress",
        "RevokeSecurityGroupEgress"
        ]
    }
    }
    ```
1. Choose Add target.
1. In the list of targets, choose SNS topic.
1. For Topic, choose an existing topic of create a new one.
1. Choose Configure details.
1. On the Configure rule details page, enter a name and an optional description. 
    - For State, leave the Enabled box selected.
1. Choose Create rule.

### Use AWS Config to check the configuration of a security group
When planning to use AWS Config and AWS Config Rules,first determine whether to use the service reactively or proactively.
    - To use AWS Config reactively, simply enable the service and it will start creating an inventory of AWS resources and a history of changes.
    - To use AWS Config for proactive change notification, do the following tasks:
        1. Create an Amazon SNS topic to receive configuration change notifications.
        1. Develop AWS Lambda function(s)or EC2-based SQS message processors to identify network-specific changes and to send appropriate notifications.
        1. Consider at least two different recipients for change notifications:
            - A network security email or paging distribution list to receive notifications that require immediate human intervention (e.g.,modifying a security group to allow all access globally or attaching an Internet gateway to an internal-only VPC).
            - A central logging or change management system that can be used to reconcile approved change requests with actual configuration changes. 
        1. Subscribe your AWS Lambda function(s) or SQS queues to the change notification SNS topic.
        1. Configure AWS Config to publish change notifications to the change notification SNS topic.

Following is an example of using AWS Config in combination with a Lambda function:

1. Create a [Lambda execution role](http://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#lambda-intro-execution-role) and an [AWS Config role](http://docs.aws.amazon.com/config/latest/developerguide/iamrole-permissions.html).
1. Enable AWS Config to record the configuration of security groups so that AWS Config can monitor security groups for changes to their configurations. The following screenshot shows an example of the settings.
Screenshot showing an example of the configuration settings
1. [Create a security group](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html) that enables ports relavent to your business requirements (e.g. 80/443 for HTTP, 25 for SMTP)
1. Create the Python Lambda function using the [code from the AWS Config rules library on GitHub](https://github.com/awslabs/aws-config-rules/blob/master/python/ec2_security_group_ingress.py).
    - The Lambda function code defines a list named REQUIRED_PERMISSIONS with elements that represent a protocol, port range, and IP range that together define a security permission. In this case, only ports 80 and 443 will be authorized.
    - Customize this Lmabda function with the ports and IP ranges required for your business needs.
1. [Create the AWS Config rule](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_develop-rules_nodejs.html#creating-a-custom-rule-with-the-AWS-Config-console) using the Lambda function you created in Step 4. 
    - For Trigger type, choose Configuration changes. 
    - For Scope of changes, choose EC2: SecurityGroup, and then type the ID of the security group you created in Step 3. 
1. Run the Config rule. This will queue the rule for execution, and the rule should run to completion in about 10 minutes.
1. Check the security group you created in Step 3.

## Escalation Procedures
- `Who is monitoring the logs/alerts, receiving them and acting upon each?`
- `Who gets notified when an alert is discovered?`
- `When do public relations and legal get involved in the process?`
- `When would you reach out to AWS Support for help?`

## Analysis
### Viewing VPC Flow Logs in Console
**To view information about flow logs for your network interfaces**
1. Open the Amazon [EC2 console](https://console.aws.amazon.com/ec2/)
1. In the navigation pane, choose Network Interfaces.
1. Select a network interface, and choose Flow Logs. Information about the flow logs is displayed on the tab. The Destination type column indicates the destination to which the flow logs are published.

**To view information about flow logs for your VPCs or subnets**
1. Open the Amazon VPC console at https://console.aws.amazon.com/vpc/
1. In the navigation pane, choose Your VPCs or Subnets.
1. Select your VPC or subnet, and choose Flow Logs. 
    - Information about the flow logs is displayed on the tab. 
    - The Destination type column indicates the destination to which the flow logs are published.

**To view flow log records published to Amazon S3**
1. Open the Amazon S3 console at https://console.aws.amazon.com/s3/
1. For Bucket name, select the bucket to which the flow logs are published.
1. For Name, select the check box next to the log file. On the object overview panel, choose Download.

### Querying flow logs using Amazon Athena
Amazon Athena is an interactive query service that enables you to analyze data in Amazon S3, such as your flow logs, using standard SQL. You can use Athena with VPC Flow Logs to quickly get actionable insights about the traffic flowing through your VPC. For example, you can identify which resources in your virtual private clouds (VPCs) are the top talkers or identify the IP addresses with the most rejected TCP connections. 

You can streamline and automate the integration of your VPC flow logs with Athena by generating a CloudFormation template that creates the required AWS resources and predefined queries that you can run to obtain insights about the traffic flowing through your VPC.

1. The CloudFormation template creates the following resources:
    - An Athena database. The database name is vpcflowlogsathenadatabase<flow-logs-subscription-id>.
    - An Athena workgroup. The workgroup name is <flow-log-subscription-id><partition-load-frequency><start-date><end-date>workgroup
    - A partitioned Athena table that corresponds to your flow log records. The table name is <flow-log-subscription-id><partition-load-frequency><start-date><end-date>.
    - A set of Athena named queries. For more information, see Predefined queries.
    - A Lambda function that loads new partitions to the table on the specified schedule (daily, weekly, or monthly).
    - An IAM role that grants permission to run the Lambda functions.
1. Requirements
    - You must select a Region that supports AWS Lambda and Amazon Athena.
    - The Amazon S3 buckets must be in the selected Region.
1. Pricing
    - You incur standard Amazon Athena charges for running queries. 
    - You incur standard AWS Lambda charges for the Lambda function that loads new partitions on a recurring schedule (when you specify a partition load frequency but do not specify a start and end date.) 
1. Do one of the following:
    - Open the Amazon VPC console. In the navigation pane, choose Your VPCs and then select your VPC.
    - Open the Amazon VPC console. In the navigation pane, choose Subnets and then select your subnet.
    - Open the Amazon EC2 console. In the navigation pane, choose Network Interfaces and then select your network interface.
1. On the Flow logs tab, select a flow log that publishes to Amazon S3 and then choose Actions, Generate Athena integration.
1. Specify the partition load frequency. 
    - If you choose None, you must specify the partition start and end date, using dates that are in the past.
    - If you choose Daily, Weekly, or Monthly, the partition start and end dates are optional. 
    - If you do not specify start and end dates, the CloudFormation template creates a Lambda function that loads new partitions on a recurring schedule.
1. Select or create an S3 bucket for the generated template, and an S3 bucket for the query results.
1. Choose Generate Athena integration.
1. (Optional) In the success message, choose the link to navigate to the bucket that you specified for the CloudFormation template, and customize the template.
1. In the success message, choose Create CloudFormation stack to open the Create Stack wizard in the AWS CloudFormation console. 
    - The URL for the generated CloudFormation template is specified in the Template section. 
    - Complete the wizard to create the resources that are specified in the template.

**Running a predefined query**

The generated CloudFormation template provides a set of predefined queries that you can run to quickly get meaningful insights about the traffic in your AWS network. After you create the stack and verify that all resources were created correctly, you can run one of the predefined queries.

1. To run a predefined query using the console
    - Open the Athena console. In the Workgroups panel, select the workgroup created by the CloudFormation template.
    - Select one of the predefined queries, modify the parameters as needed, and then run the query.
    - Open the Amazon S3 console. Navigate to the bucket that you specified for the query results, and view the results of the query.

The following are the Athena named queries provided by the generated CloudFormation template:
* VpcFlowLogsAcceptedTraffic – The TCP connections that were allowed based on your security groups and network ACLs.
* VpcFlowLogsAdminPortTraffic – The traffic recorded on administrative web app ports.
* VpcFlowLogsIPv4Traffic – The total bytes of IPv4 traffic recorded.
* VpcFlowLogsIPv6Traffic – The total bytes of IPv6 traffic recorded.
* VpcFlowLogsRejectedTCPTraffic – The TCP connections that were rejected based on your security groups or network ACLs.
* VpcFlowLogsRejectedTraffic – The traffic that was rejected based on your security groups or network ACLs.
* VpcFlowLogsSshRdpTraffic – The SSH and RDP traffic.
* VpcFlowLogsTopTalkers – The 50 IP addresses with the most traffic recorded.
* VpcFlowLogsTopTalkersPacketLevel – The 50 packet-level IP addresses with the most traffic recorded.
* VpcFlowLogsTopTalkingInstances – The IDs of the 50 instances with the most traffic recorded.
* VpcFlowLogsTopTalkingSubnets – The IDs of the 50 subnets with the most traffic recorded.
* VpcFlowLogsTopTCPTraffic – All TCP traffic recorded for a source IP address.
* VpcFlowLogsTotalBytesTransferred – The 50 pairs of source and destination IP addresses with the most bytes recorded.
* VpcFlowLogsTotalBytesTransferredPacketLevel – The 50 pairs of packet-level source and destination IP addresses with the most bytes recorded.
* VpcFlowLogsTrafficFrmSrcAddr – The traffic recorded for a specific source IP address.
* VpcFlowLogsTrafficToDstAddr – The traffic recorded for a specific destination IP address

## Containment
### User Identification
Identify what user or role executed the unauthorized network changes

Following is an example to identify the user or profile behind creation of an EC2 instance:

1. Identify one of the instance IDs for the suspicious EC2 instances from Step 1
1. Open AWS Console
1. Select Services then CloudTrail
1. In the left-hand margin, select "Event History"
1. Clear the current filter by selecting the "X" to the right of "false"
1. On the far right (under Custom) click on the gear icon
1. Scroll down and enable "AWS Access Key"
1. In the middle change the filter dropdown to "Resource name"
1. Enter the EC2 instance ID in the search box and press enter
1. You should see a RunInstances event; click on this
1. Record the username(s) you discovered. 
	- There may be multiple events to walk through to find original events such as those created from a CloudFormation template
1. Go back to the Event History and change the dropdown to "User name" and post the entry from the previous step into search, then hit enter
1. Identify any other events from the suspicious or compromised user
1. Now let's go to Services, IAM (top left of screen)
1. Under IAM Resources, select Users
1. Look for the account we identified in CloudTrail and select it
1. Select Security Credentials
1. Scroll down to the access key and select the "x" to delete the key
1. Go to your challenge dashboard and select the Check my progress button

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

## Recovery
Revert any changes that occurred to network resources

Identify resources exposed to the internet and secure them: `./prowler -g group17`

## Preventative Actions
### AWS Config and AWS System Manager
[How to auto-remediate internet accessible ports with AWS Config and AWS System Manager](https://aws.amazon.com/blogs/security/how-to-auto-remediate-internet-accessible-ports-with-aws-config-and-aws-system-manager/)

### AWS Security Hub
[Enable the new AWS Foundational Security Best Practices Security standard](https://aws.amazon.com/blogs/security/aws-foundational-security-best-practices-standard-now-available-security-hub/?secd_dir1)

### Overall Security Posture
Execute a [Self-Service Security Assessment](https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) against the environment to further identify other risks and potentially other public exposure not identified throughout this playbook.

## Lessons Learned
`This is a place to add items specific to your company that do not need "fixing", but are important to know when executing this playbook in tandem with operational and business requirements.`

## Addressed Backlog Items
- As an Incident Responder I need to be able to monitor all critical VPC environments
- As an Incident Responder I need a way to coordinate with the business team, Infrastructure team, and security teams 
- As an Incident Responder I need a playbook on quering VPC FlowLogs at scale
- As an Incident Responder I need to ensure the proper utilization of Config in all accounts

## Current Backlog Items
