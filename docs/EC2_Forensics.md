# Incident Response Playbook: EC2 Forensics
This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2021 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

[[_TOC_]]

## Points of Contact

Author: `Author Name`  
Approver: `Approver Name`  
Last Date Approved:   

## Executive Summary
This playbook outlines the process to perform forensics on EC2 instances that have been identified as malicious or have indicators of compromise that warrant additional investigation. 

It is also a best practice to perform the investigation in the same AWS Region where the event occurred, rather than replicating the data to another AWS Region. We recommend this practice primarily because of the additional time required to transfer the data between regions. For each AWS Region you choose to operate in, make sure that both your incident response process and the responders abide by the relevant data privacy laws. If you do need to move data between AWS Regions, consider the legal implications of moving data between jurisdictions. It is generally a best practice to keep the data within the same national jurisdiction.

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
1. [**PREPARATION**] Identify best practices before executing forensics procedures
2. [**PREPARATION**] Create a Forensics VPC
3. [**PREPARATION**] Add VPC Endpoint for SSM
4. [**PREPARATION**] Create a Network Access Control List for the Forensics VPC
5. [**PREPARATION**] Create a Forensics EC2 Golden AMI
6. [**PREPARATION**] Implement a training program to identify and assess cloud forensics capabilities
7. [**PREPARATION**] Identify, document, and test escalation Procedures
8. [**DETECTION AND ANALYSIS**] Create an Amazon EBS Snapshot
9. [**DETECTION AND ANALYSIS**] Share an Amazon EBS Snapshot
10. [**DETECTION AND ANALYSIS**] Share an unencrypted snapshot using the console
11. [**DETECTION AND ANALYSIS**] Use an unencrypted snapshot that was privately shared with you
12. [**DETECTION AND ANALYSIS**] Share an encrypted snapshot using the console
13. [**DETECTION AND ANALYSIS**] Use an encrypted snapshot that was shared with you
14. [**DETECTION AND ANALYSIS**] Create a Forensics EC2 from Forensics Golden AMI
15. [**DETECTION AND ANALYSIS**] Mount Suspected EBS Volume to Forensics EC2
16. [**DETECTION AND ANALYSIS**] Execute Forensics Processes
17. [**CONTAINMENT**] Isolate Affected Resources Immediately
18. [**RECOVERY**] Perform recovery processes as appropriate

***The response steps follow the Incident Response Life Cycle from [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)  

![Image](/images/nist_life_cycle.png)***

### Incident Classification & Handling
* **Tactics, techniques, and procedures**: Tools: Forensics
* **Category**: Forensics
* **Resource**: EC2, VPC
* **Indicators**: Cyber Threat Intelligence, Third Party Notice, Cloudwatch Metrics, AWS Shield
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
### [Best Practices](https://d1.awsstatic.com/Marketplace/scenarios/security/SEC_11_TSB_Final.pdf)
* Turn on AWS CloudTrail in every region
Do not limit your CloudTrail logging to AWS Regions that you actively use. Turning CloudTrail on in every AWS Region will allow you to identify unusual behavior more easily, such as AWS services being provisioned from an AWS Region that your organization does not use. 

* Protect Log Information
Verify that audit trails are enabled and active for system components, and regularly back them up. Consider storing them in a location that requires different credentials, so attackers cannot delete log files that may provide evidence of their malicious activity. 

* Never Ignore AWS Abuse Communication
AWS will automatically send an email to your registered email address when an abuse case is filed. Respond immediately, and consider setting up a dedicated email response address. The more promptly you respond to a security incident, the better you can limit its impact on your business. It is good to have a [distribution list for the alternate contact for security](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-account-payment.html#manage-account-payment-alternate-contacts) in the account that consists of multiple stakeholders so the messages don't get "stuck" in a single employee's inbox (e.g. on vacation, termination, out sick, etc.).

### Create a Forensics VPC
**NOTE**: It is recommended to have a forensics VPC setup within each region where forensics may need to be conducted against production resources.

1. Open the [Amazon VPC console](https://console.aws.amazon.com/vpc/)
1. In the navigation pane, choose Your VPCs, Create VPC.
1. Specify the following VPC details as needed.
    * Name tag: Optionally provide a name for your VPC. Doing so creates a tag with a key of Name and the value that you specify.
    * IPv4 CIDR block: Specify an IPv4 CIDR block for the VPC. The smallest CIDR block you can specify is /28, and the largest is /16. We recommend that you specify a CIDR block from the private (non-publicly routable) IP address ranges as specified in RFC 1918; for example, 10.0.0.0/16, or 192.168.0.0/16.
      * Note: You can specify a range of publicly routable IPv4 addresses. However, we currently do not support direct access to the internet from publicly routable CIDR blocks in a VPC. 
      * Windows instances cannot boot correctly if launched into a VPC with ranges from 224.0.0.0 to 255.255.255.255 (Class D and Class E IP address ranges).
    * IPv6 CIDR block: Optionally associate an IPv6 CIDR block with your VPC. Choose one of the following options, and then choose Select CIDR:
      * Amazon-provided IPv6 CIDR block: Requests an IPv6 CIDR block from Amazon's pool of IPv6 addresses. For Network Border Group, select the group from which AWS advertises IP addresses.
      * IPv6 CIDR owned by me: (BYOIP) Allocates an IPv6 CIDR block from your IPv6 address pool. For Pool, choose the IPv6 address pool from which to allocate the IPv6 CIDR block.
      * Tenancy: Select a tenancy option. 
          * Select Dedicated to ensure that instances launched in this VPC are dedicated tenancy instances, regardless of the tenancy attribute specified at launch. 
          * Select Default to ensure that instances launched in this VPC use the tenancy attribute specified at launch.
    * (Optional) Add or remove a tag.
    * Choose Create.

### [Add VPC Endpoint for SSM](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-systems-manager-vpc-endpoints/)

1. Verify that SSM Agent is installed on the instance.
2. Create an AWS Identity and Access Management (IAM) instance profile for Systems Manager. You can create a new role, or add the needed permissions to an existing role.
3. Attach the IAM role to your private EC2 instance.
4. Open the Amazon EC2 console, and then select your instance. On the Description tab, note the VPC ID and Subnet ID.
5. Create a VPC endpoint for Systems Manager.
    * For Service Name, select com.amazonaws.[region].ssm (for example, com.amazonaws.us-east-1.ssm). For a full list of Region codes, see Available Regions.
    * For VPC, choose the VPC ID for your instance.
    * For Subnets, choose a Subnet ID in your VPC. For high availability, choose at least two subnets from different Availability Zones within the Region.
    * Note: If you have more than one subnet in the same Availability Zone, you don't need to create VPC endpoints for the extra subnets. Any other subnets within the same Availability Zone can access and use the interface.
    * For Enable DNS name, select Enable for this endpoint. For more information, see Private DNS for interface endpoints.
    * For Security group, select an existing security group, or create a new one. The security group must allow inbound HTTPS (port 443) traffic from the resources in your VPC that communicate with the service.
    * If you created a new security group, open the VPC console, choose Security Groups, and then select the new security group. On the Inbound rules tab, choose Edit inbound rules. Add a rule with the following details, and then choose Save rules:
    * For Type, choose HTTPS.
    * For Source, choose your VPC CIDR. For advanced configuration, you can allow specific subnets' CIDR used by your EC2 instances.
    * Note the Security group ID. You'll use this ID with the other endpoints.
6. Create policies for VPC interface endpoints for AWS Systems Manager.
    * Repeat step 5 with the following change: For Service Name, select com.amazonaws.[region].ec2messages.
    * Repeat step 5 with the following change: For Service Name, select com.amazonaws.[region].ssmmessages. You must do this if you want to use Session Manager.

### Create a Network Access Control List for the Forensics VPC
This process will isolate the VPC so traffic can neither enter nor leave from instances contained within

1. Open the [Amazon VPC console](https://console.aws.amazon.com/vpc/)
1. In the navigation pane, choose Security, Network ACLs.
1. Select the check box next to the Forensics VPC
1. At the top-right select the dropdown Actions and Edit Inbound Rules
1. Change Rule 100 from `All Traffic` to `SSH`
1. Select Save Changes
1. Repeat these steps for Outbound Rules

### Create a Forensics EC2 Golden AMI
1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/)
1. Select `Launch Instance`
1. In the search bar type in `Ubuntu` and press enter
1. Select `Ubuntu Server 18.04 LTS`
1. Choose an Instance Type
    * Note: For forensics purposes, it is recommend to choose an M5 type instance
1. Select `Next: Configure Instance Details`
    * Network: Choose the forensics VPC network
    * Auto-assign Public IP: Disable
    * IAM Role: Select an appropriate role for forensics purposes
    * Enable termination protection: Ensure this box is checked
1. Select `Next: Add Storage`
    * Select `Add New Volume` and create a volume that is sufficient in size to handle forensics data being captured. A good starting value is 100GB, but base this value on your organizational needs and experiences
1. Select `Next: Add Tags`
1. Select `Review and Launch`
1. Select `Launch`
1. Login to the EC2 instances either through EC2 Session Manager
1. Install your forensics tools on the instance. Following are a couple of example for Open Source forensics tools:
    1. Installing SANS SIFT Toolkit
        * sudo su - ubuntu
        * sudo curl -Lo /usr/local/bin/sift https://github.com/teamdfir/sift-cli/releases/download/v1.10.0-rc5/sift-cli-linux
        * sudo chmod +x /usr/local/bin/sift
        * sudo /usr/local/bin/sift install --mode=server --user=ubuntu
        * sudo /usr/local/bin/sift upgrade
    1. Installing Google Rapid Response
        * sudo apt install mysql-server
        * sudo mysql_secure_installation
        * create database grr; grant all privileges on grr.* to grr@localhost identified by 'password'; flush privileges;
        * wget https://storage.googleapis.com/releases.grr-response.com/grr-server_3.2.4-6_amd64.deb
        * sudo apt install ./grr-server_3.2.4-6_amd64.deb
        * sudo systemctl restart grr-server
        * sudo systemctl status grr-server
            1. For more information refer to [how to Install and Setup GRR clients on Ubuntu 18.04](https://kifarunix.com/how-to-install-and-setup-grr-clients-on-ubuntu-18-04-debian-9/)
1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/)
1. Navigate to Instances
1. Right-click the instance you want to use as the basis for your AMI, and choose Create Image from the context menu. 
1. In the Create Image dialog box, type a unique name and description, and then choose Create Image. 
    * By default, Amazon EC2 shuts down the instance, takes snapshots of any attached volumes, creates and registers the AMI, and then reboots the instance. 
    * Choose No reboot if you don't want your instance to be shut down. 
    * If you choose No reboot, we can't guarantee the file system integrity of the created image. 

It may take a few minutes for the AMI to be created. After it is created, it will appear in the AMIs view in AWS Explorer. To display this view, double-click the Amazon EC2 | AMIs node in AWS Explorer. To see your AMIs, from the Viewing drop-down list, choose Owned By Me. You may need to choose Refresh to see your AMI. When the AMI first appears, it may be in a pending state, but after a few moments, it transitions to an available state. 

### Training
- `What is the expectation of knowledge for personnel with forensics responsiblities? (knowledge/skills/abilities)`
- `What forensics training and procedures do my personnel receive?`
- `What regulatory requirements for forensics apply to my company?`
- `What training is in place for analysts within the company to become familiar with AWS API (command-line environment), S3, RDS, and other AWS services?`

**Need to add in here more context and items since forensics is a skill, not just a playbook

## Escalation Procedures
- `I need a business decision on when EC2 forensics should be conducted`
- `Who is monitoring the logs/alerts, receiving them and acting upon each?`
- `Who gets notified when an alert is discovered?`
- `When do public relations and legal get involved in the process?`
- `When would you reach out to AWS Support for help?`

## Detection and Analysis
### Possible Sources of Alerts
* Billing
* CloudWatch (EC2 Metric / Event Rule)
* Config
* GuardDuty
* Inspector
* Macie
* Security Hub
* Trusted Advisor
* External Notification

### Data Protection
You should **ALWAYS**:
* Make evidence READ-ONLY
* Evidence should **NEVER** be modified
* Create copies of evidence and work ONLY from those
    * **NEVER** work from (directly access) source evidence for analysis
* Mount all evidence READ-ONLY
* Keep track of WHO accessed WHAT and WHEN
* Take copious investigation notes of:
    * What you analyzed (which piece of data)
    * When you analyzed it (date / time)
    * What you performed (specific commands / procedure / process)
    * What you found (results)

### Metadata Acquisition
Capture information related to the Instance that may be useful for the investigation
* Instance Type / ID
* IP Address(es)
* Security Group(s)
* VPC / Subnet ID
* Region
* AMI ID
* Launch Time

### Memory Acquisition
It is recommended to use a third-party utility to acquire memory from running instances.  
It is essential to capture memory **PRIOR TO** system isolation/shutdown in order to capture all available volatile (and valuable) data

### [Create an Amazon EBS Snapshot](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-modifying-snapshot-permissions.html)
1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/)
1. Choose Snapshots under Elastic Block Store in the navigation pane.
1. Choose Create Snapshot.
1. For Select resource type, choose Volume.
1. For Volume, select the volume.
1. (Optional) Enter a description for the snapshot.
1. (Optional) Choose Add Tag to add tags to your snapshot. For each tag, provide a tag key and a tag value.
1. Choose Create Snapshot.

### [Sharing an Amazon EBS Snapshot](https://d1.awsstatic.com/whitepapers/aws_security_incident_response.pdf)
#### Share an unencrypted snapshot using the console
1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/)
1. Choose Snapshots in the navigation pane.
1. Select the snapshot and then choose Actions, Modify Permissions.
1. Make the snapshot public or share it with specific AWS accounts as follows:
    * To make the snapshot public, choose Public.
    * This option is not valid for encrypted snapshots or snapshots with an AWS Marketplace product code.
    * To share the snapshot with one or more AWS accounts, choose Private, enter the AWS account ID (without hyphens) in AWS Account Number, and choose Add Permission. Repeat for any additional AWS accounts.
1. Choose Save.
#### To use an unencrypted snapshot that was privately shared with you
1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/)
1. Choose Snapshots in the navigation pane.
1. Choose the Private Snapshots filter.
1. Locate the snapshot by ID or description. You can use this snapshot as you would any other; for example, you can create a volume from the snapshot or copy the snapshot to a different Region.

#### Share an encrypted snapshot using the console
1. Open the [AWS KMS console](https://console.aws.amazon.com/kms)
1. To change the AWS Region, use the Region selector in the upper-right corner of the page.
1. Choose Customer managed keys in the navigation pane.
1. In the Alias column, choose the alias (text link) of the customer managed key that you used to encrypt the snapshot. The key details open in a new page.
1. In the Key policy section, you see either the policy view or the default view. The policy view displays the key policy document. The default view displays sections for Key administrators, Key deletion, Key Use, and Other AWS accounts. The default view displays if you created the policy in the console and have not customized it. If the default view is not available, you'll need to manually edit the policy in the policy view. For more information, see Viewing a Key Policy (Console) in the AWS Key Management Service Developer Guide.
Use either the policy view or the default view, depending on which view you can access, to add one or more AWS account IDs to the policy, as follows:
    * (Default view) Scroll down to Other AWS accounts. Choose Add other AWS accounts and enter the AWS account ID as prompted. To add another account, choose Add another AWS account and enter the AWS account ID. When you have added all AWS accounts, choose Save changes.
    * (Policy view) Choose Edit. Add one or more AWS account IDs to the following statements: "Allow use of the key" and "Allow attachment of persistent resources". Choose Save changes. In the following example, the AWS account ID 444455556666 is added to the policy.
```
    {
      "Sid": "Allow use of the key",
      "Effect": "Allow",
      "Principal": {"AWS": [
        "arn:aws:iam::111122223333:user/CMKUser",
        "arn:aws:iam::444455556666:root"
      ]},
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey"
      ],
      "Resource": "*"
    },
    {
      "Sid": "Allow attachment of persistent resources",
      "Effect": "Allow",
      "Principal": {"AWS": [
        "arn:aws:iam::111122223333:user/CMKUser",
        "arn:aws:iam::444455556666:root"
      ]},
      "Action": [
        "kms:CreateGrant",
        "kms:ListGrants",
        "kms:RevokeGrant"
      ],
      "Resource": "*",
      "Condition": {"Bool": {"kms:GrantIsForAWSResource": true}}
    }    
```
1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/)
1. Choose Snapshots in the navigation pane.
1. Select the snapshot and then choose Actions, Modify Permissions.
1. For each AWS account, enter the AWS account ID in AWS Account Number and choose Add Permission. When you have added all AWS accounts, choose Save.

#### To use an encrypted snapshot that was shared with you
1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/)
1. Choose Snapshots in the navigation pane.
1. Choose the Private Snapshots filter. Optionally add the Encrypted filter.
1. Locate the snapshot by ID or description.
1. Select the snapshot and choose Actions, Copy.
1. (Optional) Select a destination Region.
1. The copy of the snapshot is encrypted by the key displayed in Master Key. By default, the selected key is your account's default CMK. To select a customer managed CMK, click inside the input box to see a list of available keys.
1. Choose Copy.

### Create a Forensics EC2 from Forensics Golden AMI
Create a new Forensics instance using your previously created Forensics Golden AMI

### Mount Suspected EBS Volume to Forensics EC2
1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/)
1. In the navigation pane, choose Elastic Block Store, Volumes.
1. Select an available volume and choose Actions, Attach Volume.
1. For Instance, start typing the name or ID of the instance. 
1. Select the instance from the list of options (only instances that are in the same Availability Zone as the volume are displayed).
1. For Device, you can keep the suggested device name, or type a different supported device name. 
    * For more information, see [Device names on Linux instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/device_naming.html).
1. Choose Attach.
1. Connect to your instance and mount the volume. 
    * For more information, see [Make an Amazon EBS volume available for use on Linux](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html).

### Execute Forensics Processes
* `What forensics outputs is the company looking for?`
* `What will be included in the final report?`
* `Who does the report go to?`
* `Are there legal or regulatory requirements identifying information required from a forensics investigation?`
* `Are there personnel in the company that are properly trained/licensed/certified to perform forensics?`
* `Are there any retention requirements for logs, data, or evidence? Potentially look into Glacier Vaults to support`
* An example of [Digital Forensic Analysis ofAmazon Linux EC2 Instances](https://www.sans.org/reading-room/whitepapers/cloud/digital-forensic-analysis-amazon-linux-ec2-instances-38235) is available from SANS institute.

## Containment
### Isolate Affected Resources Immediately
**NOTE**: Ensure you have a process in place to request escalation and approval to isolate resources to ensure a business impact analysis is conducted first on how the isolation will impact current operations and revenue streams.

### Instance Decommission
Enable Termination Protections
* [Disable Termination of Instance (via Console/API/CLI)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html)
    1. Set Instance Shutdown Behavior to Stop (vs. Terminate)
    1. Disable “DeleteOnTermination” for all Instance Volumes
* [Remove (detach) from Auto-Scaling group](https://docs.aws.amazon.com/autoscaling/ec2/userguide/detach-instance-asg.html)
    1. Open the Amazon EC2 Auto Scaling console at https://console.aws.amazon.com/ec2autoscaling/
    1. Select the check box next to your Auto Scaling group.
    1. A split pane opens up in the bottom part of the Auto Scaling groups page, showing information about the group that's selected.
    1. On the Instance management tab, in Instances, select an instance and choose Actions, Detach.
    1. In the Detach instance dialog box, select the check box to launch a replacement instance, or leave it unchecked to decrement the desired capacity. Choose Detach instance.
* [Deregister from Load Balancer(s)](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-deregister-register-instances.html)
    1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/
    1. On the navigation pane, under LOAD BALANCING, choose Load Balancers.
    1. Select your load balancer.
    1. In the bottom pane, select the Instances tab.
    1. Choose Edit Instances.
    1. Select the instance to register with your load balancer.
    1. Choose Save.
1. Create a *new* security group that blocks all ingress and egress traffic; ensure you remove the default `allow all` rule for egress traffic
    * Attach the *new* security group to the impacted instance(s)

## Eradication
There are no steps for this process applicable to this runbook.

## Recovery
Upon conclusion of forensics activities, and following issuance of the final report you should terminate the forensics EC2 instance and delete the EBS snapshot **UNLESS** there is a legal or regulatory requirement to maintain the data. 

## Lessons Learned
`This is a place to add items specific to your company that do not necessarilly need "fixing", but are important to know when executing this playbook in tandem with operational and business requirements.`

## Addressed Backlog Items
- As an Incident Responder I need a runbook to conduct EC2 Forensics
- As an Incident Responder I need a business decision on when EC2 forensics should be conducted
- As an Incident Responder I need to have logging enabled in all regions that are enabled regardless of intention of use
- As an Incident Responder I need to ensure only approved AMIs are being used
- As an Incident Responder I need to be able to detect crypto mining on my existing EC2 instances
- As an Incident Responder I need a prepared method to delete mass numbers of instances

## Current Backlog Items
