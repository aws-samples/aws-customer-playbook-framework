# Incident Response Playbook: Analyzing VPC Flow Logs
This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2021 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

## Points of Contact

Author: `Author Name` \
Approver: `Approver Name` \
Last Date Approved:   

## Executive Summary
This playbook outlines example processes and queries to analyze AWS VPC Flow Logs. 

## Preparation
This playbook references and integrates, where possible, with [Prowler](https://github.com/toniblyx/prowler) which is a command line tool that helps you with AWS security assessment, auditing, hardening and incident response.

It follows guidelines of the CIS Amazon Web Services Foundations Benchmark (49 checks) and has more than 100 additional checks including related to GDPR, HIPAA, PCI-DSS, ISO-27001, FFIEC, SOC2 and others. 

This tool provides a rapid snapshot of the current state of security within a customer environment. Additionally, [AWS Security Hub](https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) provides for automated compliance scanning and can [integrate with Prowler](https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

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

### Incident Classification & Handling
* **Tactics, techniques, and procedures**: Tool: Athena
* **Category**: Log Analysis
* **Resource**: Athena
* **Indicators**: Cyber Threat Intelligence, Third Party Notice
* **Log Sources**: VPC Flow Logs
* **Teams**: Security Operations Center (SOC), Forensic Investigators, Cloud Engineering

## Incident Handling Process
### The incident response process has the following stages:
* Preparation
* Detection & Analysis
* Containment & Eradication
* Recovery
* Post-Incident Activity

### Response Steps
1. [**PREPARATION**] Create a Publicly Exposed Asset Inventory
2. [**PREPARATION**] Setup logging
3. [**PREPARATION**] Implement a training program to identify and assess log analysis capabilities
4. [**DETECTION AND ANALYSIS**] Viewing VPC Flow Logs in Console
5. [**DETECTION AND ANALYSIS**] Querying flow logs using Amazon Athena
6. [**DETECTION AND ANALYSIS**] Create the VPC Flow Logs table as a DDL
7. [**DETECTION AND ANALYSIS**] Example Athena Queries: Create the NEW Logs table as a DDL
8. [**DETECTION AND ANALYSIS**] Example Athena Queries: Check to see if it is loaded and how many lines are in a file
9. [**DETECTION AND ANALYSIS**] Example Athena Queries: Check to see if viewable and usable data is returned:
10. [**DETECTION AND ANALYSIS**] Example Athena Queries: Check for the Top IPs
11. [**DETECTION AND ANALYSIS**] Example Athena Queries: Standard Queries for performing investigations
12. [**DETECTION AND ANALYSIS**] Example Athena Queries: Look up Data Ratio for Source/Destination IPs
13. [**DETECTION AND ANALYSIS**] Example Athena Queries: Searching for known IOCs
14. [**DETECTION AND ANALYSIS**] Example Athena Queries: Threat table
15. [**DETECTION AND ANALYSIS**] Example Athena Queries: Lookup view to target threat type

## Preparation
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
    * Open the [Amazon CloudWatch console](https://console.aws.amazon.com/cloudwatch/)
    * In the navigation pane, choose Metrics.
    * Under All metrics, choose the NAT gateway metric namespace.
    * To view the metrics, select the metric dimension.
1. To create an alarm for outbound traffic through the NAT gateway:
    * Open the [Amazon CloudWatch console](https://console.aws.amazon.com/cloudwatch/)
    * In the navigation pane, choose Alarms, Create Alarm.
    * Choose NAT gateway.
    * Select the NAT gateway and the BytesOutToDestination metric and choose Next.
    * Configure the alarm as follows, and choose Create Alarm when you are done:
    * Under Alarm Threshold, enter a name and description for your alarm. For Whenever, choose >= and enter 5000000. Enter 1 for the consecutive periods.
    * Under Actions, select an existing notification list or choose New list to create a new one.
    * Under Alarm Preview, select a period of 15 minutes and specify a statistic of Sum.
1. To create an alarm to monitor port allocation errors
    * Open the [Amazon CloudWatch console](https://console.aws.amazon.com/cloudwatch/)
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

## Detection and Analysis
### Viewing VPC Flow Logs in Console
**To view information about flow logs for your network interfaces**
1. Open the Amazon [EC2 console](https://console.aws.amazon.com/ec2/)
1. In the navigation pane, choose Network Interfaces.
1. Select a network interface, and choose Flow Logs. Information about the flow logs is displayed on the tab. The Destination type column indicates the destination to which the flow logs are published.

**To view information about flow logs for your VPCs or subnets**
1. Open the [Amazon VPC console](https://console.aws.amazon.com/vpc/)
1. In the navigation pane, choose Your VPCs or Subnets.
1. Select your VPC or subnet, and choose Flow Logs. 
    - Information about the flow logs is displayed on the tab. 
    - The Destination type column indicates the destination to which the flow logs are published.

**To view flow log records published to Amazon S3**
1. Open the [Amazon S3 console](https://console.aws.amazon.com/s3/)
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

## Deep Dive Analysis:

Reference: https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html

### Create the VPC Flow Logs table as a DDL 
The following code is used to create the table as DDL for the VPC Flow Logs that will be examined. (Please take note of the TODO items inline):
```
/* 
    Creates a VPC flow table for VPC flow logs delivered directly to an S3 bucket
    Includes partitioning configuration for multi-account, multi-region deployment as well as date in YYYY/MM/DD format
    Includes all available fields, including non-default v3 and v4 fields.
    NOTE: VPC flow fields must be configured in the order listed when configured, if they were configured in a different order you'll need to adjust the order their listed in the DDL below
*/

/*
    TODO: optionally update the table name "vpcflow" to the name you'd like to use for the VPC flow log table 
*/
CREATE EXTERNAL TABLE IF NOT EXISTS vpcflow (
  /*
    TODO: verify that VPC flow logs were configured to be generated in this order, if they weren't you'll need to rearange the order below to match the ourder in which they were generated
    TIP: download a sample and check the first line of each log file for the field order,
       Don't worry if they field names don't match exactly with the names in the top line of the log, Athena will import them based on their order
    NOTE: These v2 fields are in the default format and usually don't need to be adjusted, pay closer attention to the order of the v3/v4/v5 fields below as there is no default ordering of those fields. Fields and descriptions can be found at https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html
  */
  version int,
  account string,
  interfaceid string,
  sourceaddress string,
  destinationaddress string,
  sourceport int,
  destinationport int,
  protocol int,
  numpackets int,
  numbytes bigint,
  starttime int,
  endtime int,
  action string,
  logstatus string,
  /*
    NOTE: start of non-default v3 and v4 fields
          don't worry if your source logs don't all include these fields, they'll just show as blank 
    TODO: If your VPC flow logs include these fields, be sure to check the order they're listed below is the same order they're configured in the flow log format
  */
  vpcid string, 
  type string, 
  tcpflags smallint,
  subnetid string,
  sublocationtype string,
  sublocationid string,
  region string,
  pktsrcaddr string,
  pktdstaddr string,
  instanceid string,
  azid string,
  /*
    NOTE: start of non-default v5 fields
          don't worry if your source logs don't all include these fields, they'll just show as blank 
    TODO: If your VPC flow logs include these fields, be sure to check the order they're listed below is the same order they're configured in the flow log format
  */
  pkt-src-aws-service string,
  pkt-dst-aws-service string,
  flow-direction string,
  traffic-path int
)
PARTITIONED BY
(
 date_partition STRING,
 region_partition STRING,
 account_partition STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ' '
/*
    TODO: replace bucket_name and optional_prefix in LOCATION value, 
    if there's no prefix then remove the extra /
    example: s3://my_central_log_bucket/AWSLogs/ or s3://my_central_log_bucket/PROD/AWSLogs/
*/
LOCATION 's3://<bucket_name>/<optional_prefix>/AWSLogs/' 
TBLPROPERTIES
(
 "skip.header.line.count"="1", 
 "projection.enabled" = "true",
 "projection.date_partition.type" = "date",
 /* TODO: replace <YYYY>/<MM>/<DD> with the first date of your logs, example: 2020/11/30 */
 "projection.date_partition.range" = "<YYYY>/<MM>/<DD>,NOW", 
 "projection.date_partition.format" = "yyyy/MM/dd",
 "projection.date_partition.interval" = "1",
 "projection.date_partition.interval.unit" = "DAYS",
 "projection.region_partition.type" = "enum",
 "projection.region_partition.values" = "us-east-2,us-east-1,us-west-1,us-west-2,af-south-1,ap-east-1,ap-south-1,ap-northeast-3,ap-northeast-2,ap-southeast-1,ap-southeast-2,ap-northeast-1,ca-central-1,cn-north-1,cn-northwest-1,eu-central-1,eu-west-1,eu-west-2,eu-south-1,eu-west-3,eu-north-1,me-south-1,sa-east-1",
 "projection.account_partition.type" = "enum",
 /*
    TODO: replace values in projection.account_partition.values with the list of AWS account numbers that you want to include in this table
    example: "0123456789,0123456788,0123456777"
    note: do not use any spaces, seperate the values with a comma only (including spaces will cause a syntax error)
    if there is only one account, include it by itself with no comma, for example: "0123456789"
 */
 "projection.account_partition.values" = "<account_num_1>,<account_num_2>,...",
 /*
    TODO: Same as LOCATION, replace bucket_name and optional_prefix in storage.location.template value, 
    if there's no prefix then remove the extra /
    Ensure that only the values within the <> brackets are replaced, the values within the {} brackets are variables and must be left as defined below.
    example: s3://my_central_log_bucket/AWSLogs/... or s3://my_central_log_bucket/PROD/AWSLogs/...
 */
 "storage.location.template" = "s3://<bucket_name>/<optional_prefix>/AWSLogs/${account_partition}/vpcflowlogs/${region_partition}/${date_partition}"
);
```
### Example Queries:
#### Create the NEW Logs table as a DDL 
Create a new Athena table by executing the following query below.  Keep in mind:

* Replace <bucket_name> with the target bucket.  
* Check the LOCATION to ensure it includes any relevant bucket prefixes and its trailing slash (“/”)
```
CREATE EXTERNAL TABLE IF NOT EXISTS vpc_flow_logs (
is_rejected_packet STRING,
avg_in_packet_size INT,
vpc_id STRING,
local_port INT,
source_dc STRING,
ip STRING,
end_time STRING,
remote_port INT,
start_time STRING,
protocol INT,
in_bytes BIGINT,
source_tag STRING,
account_id STRING,
instance_id STRING,
interface_public_ips STRING,
out_bytes BIGINT,
is_skipped_rejected_packets STRING,
source_az STRING,
source_region STRING,
avg_out_packet_size INT,
instance_type STRING,
interface_local_ips STRING
)
ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
WITH SERDEPROPERTIES ( 'ignore.malformed.json' = 'true')
LOCATION 's3://<bucket_name>/';
```
#### Check to see if it is loaded and how many lines are in the file:
```
SELECT count(*) FROM vpc_flow_logs;
```
#### Check to see if viewable and usable data is returned:
```
SELECT * FROM vpc_flow_logs limit 1000;
```
#### Check for the Top IPs
```
select ip, count(*) cnt
from vpc_flow_logs
group by ip
order by cnt desc
limit 100;

Take some top IPs and order by time(swap out the IP below)

select * from vpc_flow_logs
where ip = '123.123.123.123'
order by start_time;
```

### S3 Gateway / VPC Interface Notes
For VPC Endpoints, connections will show in the flow logs as traffic over their private addresses:

S3 Gateway show in the flow logs as the private address connecting to public ranges in the prefix list:

### Standard Queries for performing investigations

#### Look up Data Ratio for Source/Destination IPs
The following query aggregates VPC Flow Log records by IP, record count (total number of records of the same source or destination address), and total bytes (total bytes of communication between source or destination address). The query also gives the data ratio, which is how many bytes sent per communication (total bytes divided by record count).
```
SELECT sourceaddress,
         destinationaddress,
         count(*) AS record_count,
         SUM(numbytes) AS total_bytes,
         SUM(numbytes)/count(*) AS data_ratio,
         date_format(from_unixtime(min(starttime)),
         '%Y/%m/%d %H:%i:%s') AS first_seen, date_format(from_unixtime(max(endtime)), '%Y/%m/%d %H:%i:%s') AS last_seen
FROM "<database>"."<table>"
WHERE (sourceaddress = '10.10.10.10'
        OR destinationaddress = '10.10.10.10')
GROUP BY  sourceaddress, destinationaddress
ORDER BY  total_bytes DESC 
```
#### Searching for known IOCs
The following query assumes IOCs are external IP addresses and uses a lookup table to manage those

#### Threat table
```
CREATE EXTERNAL TABLE IF NOT EXISTS incident_iocs (
  threat_ip string,
  threat_comment string
)

ROW FORMAT SERDE
'org.apache.hadoop.hive.serde2.OpenCSVSerde'

WITH SERDEPROPERTIES (
'separatorChar' = ',',
'quoteChar' = '\"',
'escapeChar' = '\\'
)
STORED AS TEXTFILE
LOCATION 's3://bucket-analysis/iocs/iocfilehere/'
```
#### Lookup view to target threat type
```
CREATE VIEW incident_vpc_flow_directional AS
SELECT vpc.account,
         vpc.interfaceid,
         vpc.sourceaddress || ':' || CAST(vpc.sourceport AS VARCHAR) AS source,
         vpc.destinationaddress || ':' || CAST(vpc.destinationport AS VARCHAR) AS destination,
         'INBOUND' AS direction,
         vpc.sourceaddress AS connecting_ip,
         vpc.sourceport AS connecting_port,
         vpc.protocol,
         vpc.numpackets,
         vpc.numbytes,
         vpc.action,
         vpc.logstatus,
         CAST(from_unixtime(vpc.starttime) AS TIMESTAMP) AS startTime, 
         CAST(from_unixtime(vpc.endtime) AS TIMESTAMP) AS endTime,
         date_diff('second', from_unixtime(vpc.starttime), from_unixtime(vpc.endtime)) as sessionTime
         
FROM "<database>"."<table>" AS vpc

WHERE 
    vpc.destinationaddress IN (SELECT threat_ip FROM incident_iocs)

UNION ALL

SELECT vpc.account,
         vpc.interfaceid,
         vpc.sourceaddress || ':' || CAST(vpc.sourceport AS VARCHAR) AS source,
         vpc.destinationaddress || ':' || CAST(vpc.destinationport AS VARCHAR) AS destination,
         'OUTBOUND' AS direction,
         vpc.destinationaddress AS connecting_ip,
         vpc.destinationport AS connecting_port,
         vpc.protocol,
         vpc.numpackets,
         vpc.numbytes,
         vpc.action,
         vpc.logstatus,
         CAST(from_unixtime(vpc.starttime) AS TIMESTAMP) AS startTime, 
         CAST(from_unixtime(vpc.endtime) AS TIMESTAMP) AS endTime,
         date_diff('second', from_unixtime(vpc.starttime), from_unixtime(vpc.endtime)) as sessionTime
         
FROM "<database>"."<table>" AS vpc
WHERE
    vpc.sourceaddress IN (SELECT threat_ip FROM incident_iocs)

ORDER BY startTime, endTime, sessionTime, interfaceid, connecting_ip, direction
```

## Addressed Backlog Items
- As an Incident Responder I need to be able to monitor all critical VPC environments
- As an Incident Responder I need a playbook on quering VPC FlowLogs at scale

## Current Backlog Items
