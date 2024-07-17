# Incident Response Playbook: Denial of Service / Distributed Denial of Service (DoS/DDoS)
This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2024 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

## Points of Contact

Author: `Author Name` \
Approver: `Approver Name` \
Last Date Approved:   

## Executive Summary
This playbook outlines the process for responding to Denial of Service / Distributed Denial of Service (DoS/DDoS) attacks against AWS hosted resources.

## Potential Indicators of Compromise
- An internet facing application is under heavy load, either due to popularity or malicious intent
- A single EC2 instance is being used to host an application and we are dropping/shaping traffic (customer is notified via AWS Abuse)
- Results presented in the AWS Shield Dashboard

## Potential AWS GuardDuty Findings
- Backdoor:EC2/DenialOfService.Dns
- Backdoor:EC2/DenialOfService.Tcp
- Backdoor:EC2/DenialOfService.Udp
- Backdoor:EC2/DenialOfService.UdpOnTcpPorts
- Backdoor:EC2/DenialOfService.UnusualProtocol
- Backdoor:EC2/Spambot
- Behavior:EC2/NetworkPortUnusual
- Behavior:EC2/TrafficVolumeUnusual

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
[AWS Cloud Adoption Framework Security Perspective](https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* **Directive**
* **Detective**
* **Responsive**
* **Preventative**

![Image](/images/aws_caf.png)
* * *

### Response Steps
1. [**PREPARATION**] Create a Publicly Exposed Asset Inventory
2. [**PREPARATION**] Implement training to address DoS/DDoS attacks
3. [**PREPARATION**] Develop a communication strategy to escalate and report events
4. [**PREPARATION**] Perform documentation reviews to ensure procedures are maintained and current
5. [**DETECTION AND ANALYSIS**] Implement AWS Shield
6. [**DETECTION AND ANALYSIS**] Utilize the Global Threat Environment Dashboard with AWS Shield
7. [**DETECTION AND ANALYSIS**] Consider using AWS Shield Advanced
8. [**PREPARATION**] Implement Escalation and Notification Procedures
9. [**DETECTION AND ANALYSIS**] Perform Detection and Mitigation
10. [**DETECTION AND ANALYSIS**] Identify Top Contributers
11. [**CONTAINMENT**] Perform Containment
12. [**ERADICATION**] Perform Eradication
13. [**POST-INCIDENT ACTIVITIES**] Request a credit in AWS Shield Advanced
14. [**PREPARATION**] Additional Preventative Actions: Mitigation Techniques
15. [**PREPARATION**] Additional Preventative Actions: Attack Surface Reduction
16. [**PREPARATION**] Additional Preventative Actions: Operational Techniques
17. [**PREPARATION**] Additional Preventative Actions: Overall Security Posture

***The response steps follow the Incident Response Life Cycle from [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)  

![Image](/images/nist_life_cycle.png)***

### Incident Classification & Handling

* **Tactics, techniques, and procedures**: Denial of Service / Distributed Denial of Service Attacks
* **Category**: DoS/DDoS
* **Resource**: Network
* **Indicators**: Cyber Threat Intelligence, Third Party Notice, Cloudwatch Metrics, AWS Shield
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

This tool provides a rapid snapshot of the current state of security within a customer environment. Additionally, [AWS Security Hub](https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) provides for automated compliance scanning and can [integrate with Prowler](https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Publicly Exposed Asset Inventory
Find resources exposed to the internet: `./prowler -g group17`

You can use Security Hub and Firewall Manager to [set up centralized monitoring for DDoS events and auto-remediate noncompliant resources](https://aws.amazon.com/blogs/security/set-up-centralized-monitoring-for-ddos-events-and-auto-remediate-noncompliant-resources/)

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

### Documentation Review
1. Be familiar with the [AWS Shield Documentation](https://docs.aws.amazon.com/waf/latest/developerguide/shield-chapter.html)
1. If subscribed, follow the [Getting started with AWS Shield Advanced guide](https://docs.aws.amazon.com/waf/latest/developerguide/getting-started-ddos.html)

## Detection
### Cloudwatch Metrics to detect DoS/DDoS events
The Recommended Amazon CloudWatch Metrics table lists descriptions of Amazon CloudWatch metrics that are commonly used to detect and react to DDoS attacks.  

* AWS Shield Advanced: DDoSDetected      
    * Indicates a DDoS event for a specific Amazon Resource Name (ARN).  
* AWS Shield Advanced: DDoSAttackBitsPerSecond  
    * The number of bytes observed during a DDoS event for a specific ARN. This metric is only available for layer 3/4 DDoS events.  
* AWS Shield Advanced: DDoSAttackPacketsPerSecond  
    * The number of packets observed during a DDoS event for a specific ARN. This metric is only available for layer 3/4 DDoS events.  
* AWS Shield Advanced: DDoSAttackRequestsPerSecond
    * The number of requests observed during a DDoS event for a specific ARN. This metric is only available for layer 7 DDoS events and is only reported for the most significant layer 7 events. 
* AWS WAF: AllowedRequests 
    * The number of allowed web requests. 
* AWS WAF: BlockedRequests 
    * The number of blocked web requests. 
* AWS WAF: CountedRequests 
    * The number of counted web requests. 
* AWS WAF: PassedRequests 
    * The number of passed requests. This is only used for requests that go through a rule group evaluation without matching any of the rule group rules. 
* CloudFront: Requests 
    * The number of HTTP/S requests. 
* CloudFront: TotalErrorRate 
    * The percentage of all requests for which the HTTP status code is 4xx or 5xx. 
* Amazon Route 53: HealthCheckStatus 
    * The status of the health check endpoint. 
* ALB: ActiveConnectionCount 
    * The total number of concurrent TCP connections that are active from clients to the load balancer, and from the load balancer to targets. 
* ALB: ConsumedLCUs 
    * The number of load balancer capacity units (LCU) used by your load balancer. 
* ALB: HTTPCode_ELB_4XX_Count HTTPCode_ELB_5XX_Count 
    * The number of HTTP 4xx or 5xx client error codes generated by the load balancer. 
* ALB: NewConnectionCount 
    * The total number of new TCP connections established from clients to the load balancer,and from the load balancer to targets. 
* ALB: ProcessedBytes 
    * The total number of bytes processed by the load balancer. 
* ALB: RejectedConnectionCount 
    * The number of connections rejected because the load balancer reached its maximum number of connections. 
* ALB: RequestCount 
    * The number of requests that were processed. 
* ALB: TargetConnectionErrorCount 
    * The number of connections that were not successfully established between the load balancer and the target. 
* ALB: TargetResponseTime 
    * The time elapsed, in seconds, after the request leaves the load balancer until a response from the target is received. 
* ALB: UnHealthyHostCount 
    * The number of targets that are considered unhealthy. 
* NLB: ActiveFlowCount 
    * The total number of concurrent TCP flows (or connections) from clients to targets. 
* NLB: ConsumedLCUs 
    * The number of load balancer capacity units (LCU) used by your load balancer. 
* NLB: NewFlowCount 
    * The total number of new TCP flows (or connections) established from clients to targets in the time period. 
* NLB: ProcessedBytes 
    * The total number of bytes processed by the load balancer, including TCP/IP headers. 
* Global Accelerator NewFlowCount 
    * The total number of new TCP and UDP flows (or connections) established from clients to endpoints in the time period. 
* Global Accelerator: ProcessedBytesIn 
    * The total number of incoming bytes processed by the accelerator, including TCP/IP headers. 
* Auto Scaling: GroupMaxSize 
    * The maximum size of the Auto Scaling group. 
* Amazon EC2: CPUUtilization 
    * The percentage of allocated EC2 compute units that are currently in use. 
* Amazon EC2: NetworkIn 
    * The number of bytes received by the instance on all network interfaces. 
### AWS Shield
The [AWS WAF & Shield console](https://console.aws.amazon.com/wafv2/shieldv2) or API provide a summary and details about attacks that have been detected. 
1. Sign in to the AWS Management Console and open the [AWS WAF & Shield console](https://console.aws.amazon.com/wafv2/)
1. In the AWS Shield navigation bar, choose Overview or Events

### Global Threat Environment Dashboard
The Global Threat Environment Dashboard provides summary information about all DDoS attacks that have been detected by AWS.
1. Sign in to the AWS Management Console and open the [AWS WAF & Shield console](https://console.aws.amazon.com/wafv2/)
1. In the AWS Shield navigation bar, choose Global threat dashboard.
1. Choose a time period.

### AWS Shield Advanced
1. Sign in to the AWS Management Console and open the [AWS WAF & Shield console](https://console.aws.amazon.com/wafv2/)
1. In the AWS Shield navigation bar, choose Overview or Events

You have access to a number of CloudWatch metrics that can indicate that your application is being targeted. 
1. You can configure the DDoSDetected metric to tell you if an attack has been detected. 
1. If you want to be alerted based on the attack volume, you can also use the DDoSAttackBitsPerSecond, DDoSAttackPacketsPerSecond, or DDoSAttackRequestsPerSecond metrics.

A full list of available metrics is available within the table in the [DDoS Visibility](https://docs.aws.amazon.com/whitepapers/latest/aws-best-practices-ddos-resiliency/visibility.html)

## Escalation Procedures

### AWS Shield
- `Who is monitoring the logs/alerts, receiving them and acting upon each?`
- `Who gets notified when an alert is discovered?`
- `When do public relations and legal get involved in the process?`
- `When would you reach out to AWS Support for help?`

### AWS Shield Advanced
- `Who is monitoring the logs/alerts, receiving them and acting upon each?`
- `Who gets notified when an alert is discovered?`
- `When do public relations and legal get involved in the process?`
- `Have you specified the AWS resources that you want to protect using Shield Advanced?`
    1. Sign in to the AWS Management Console and open the [AWS WAF & Shield console](https://console.aws.amazon.com/wafv2/)
    1. In the AWS Shield navigation bar, choose Protected Resources
- `Have you authorized the DDoS Response Team (DRT) to access your WAF rules and logs? This helps them mitigate attacks when you request help.`
    1. Sign in to the AWS Management Console and open the [AWS WAF & Shield console](https://console.aws.amazon.com/wafv2/)
    1. In the AWS Shield navigation bar, choose Overview
    1. Configure AWS DRT support `Edit DRT access`
- `Do you need support form the AWS DDoS Response Team?`
    1. You can open a case under AWS Shield in the AWS Support Center
        * To get DDoS Response Team (DRT) support, contact the AWS Support Center and Select the following options:
            1. Case type: Technical Support
            1. Service: Distributed Denial of Service (DDoS)
            1. Category: Inbound to AWS
        * With AWS Shield Advanced proactive engagement, the DRT contacts you directly if the Amazon Route 53 health check associated with your protected resource becomes unhealthy during an event that is detected by Shield Advanced. 

## Analysis
1. Sign in to the AWS Management Console and open the [AWS WAF & Shield console](https://console.aws.amazon.com/wafv2/)
1. In the AWS Shield navigation bar, choose Events

### Detection and Mitigation
The Detection and mitigation tab displays graphs that show the traffic that AWS Shield evaluated prior to reporting this event and the effect of any mitigation that was placed. It also provides metrics for infrastructure-layer Distributed Denial of Service (DDoS) events for resources in your own Amazon Virtual Private Cloud VPC and AWS Global Accelerator accelerators. Detection and mitigation metrics are not included for Amazon CloudFront or Amazon Route 53 resources. 

### Top Contributers
The Top contributors tab provides insight into where traffic is coming from during a detected event. You can view the highest volume contributors, sorted by aspects such as protocol, source port, and TCP flags. You can download the information and you can adjust the units used and the number of contributors to display. 

## Containment

### [AWS Best Practices for DDoS Resiliency](https://d0.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf)
In this whitepaper, AWS provides you with prescriptive DDoS guidance to improve the resiliency of applications running on AWS. This includes a DDoS-resilient reference architecture that can be used as a guide to help protect application availability. This whitepaper also describes different attack types, such as infrastructure layer attacks and application layer attacks. AWS explains which best practices are most effective to manage each attack type. In addition, the services and features that fit into a DDoS mitigation strategy are outlined and how each one can be used to help protect your applications is explained.
Containment procedures assume the use of AWS Web Application Firewalls. If a third party WAF or firewall is being used, customize those procedures here to match your use case. 

### AWS WAF
1. Create conditions in AWS WAF that match the unusual behavior.
1. Add those conditions to one or more AWS WAF rules.
1. Add those rules to a web ACL and configure the web ACL to count the requests that match the rules.
    * **NOTE** You should always test your rules first by initially using Count rather than Block. After you're comfortable that the new rule is identifying the correct requests, you can modify your rule to block those requests.
1. Monitor those counts to determine if the source of the requests should be blocked. If the volume of requests continues to be unusually high, change your web ACL to block those requests. 
 
## Eradication
Not Applicable

## Recovery
### Requesting a credit in AWS Shield Advanced
If you're subscribed to AWS Shield Advanced and you experience a DDoS attack that increases utilization of an Shield Advanced protected resource, you can request a credit for charges related to the increased utilization to the extent that it is not mitigated by Shield Advanced. 

Additional information can be found in AWS documents at [Requesting a credit in AWS Shield Advanced](https://docs.aws.amazon.com/waf/latest/developerguide/request-refund.html)

## Preventative Actions
### Mitigation Techniques
On AWS, DDoS mitigation capabilities are automatically provided
1. AWS Shield Standard defends against most common, frequently occurring network and transport layer DDoS attacks that target your web site or applications. This is offered on all AWS services and in every AWS Region at no additional cost.
1. You can improve your readiness to respond to and mitigate DDoS attacks is by subscribing to AWS Shield Advanced. This optional DDoS mitigation service helps you protect an application hosted on any AWS Region. The service is available globally for Amazon CloudFront and Amazon Route 53. It’s also available in select AWS Regions for Classic Load Balancer (CLB), Application Load Balancer (ALB), and Elastic IP Addresses (EIPs). Using AWS Shield Advanced with EIPs allows you to protect Network Load Balancer (NLBs) or Amazon EC2 instances.
1. Key considerations to help mitigate volumetric DDoS attacks include ensuring that enough transit capacity and diversity is available, and protecting your AWS resources, like Amazon EC2 instances, against attack traffic
1. Large DDoS attacks can overwhelm the capacity of a single Amazon EC2 instance, so adding load balancing can help your resiliency.
1. Amazon CloudFront allows you to cache static content and serve it from AWS edge locations, which can help reduce the load on your origin.
1. By using AWS WAF, you can configure web access control lists(Web ACLs) on your CloudFront distributions or Application Load Balancers to filter and block requests based on request signatures.
### Attack Surface Reduction
1. You can specify security groups when you launch an instance, or you can associate the instance with a security group ata later time. All internet traffic to a security group is implicitly denied unless you create an allow rule to permit the traffic. 
    * If you are subscribed to AWS Shield Advanced, you can register Elastic IPs (EIPs) as Protected Resources.
1. If you’re using Amazon CloudFront with an origin that is inside of your VPC, you should use an AWS Lambda function to automatically update your security group rules to allowonly Amazon CloudFront traffic.
1. Typically, when you must expose an API to the public, there is a risk that the API frontend could be targeted by a DDoS attack. To help reduce the risk, you can use Amazon API Gateway as an entryway to applications running on Amazon EC2, AWS Lambda, or elsewhere.
    * When you use Amazon CloudFront and AWS WAF with Amazon API Gateway, configure the following options:
        1. Configure the cache behavior for your distributions to forward all headers to the API Gateway regional endpoint. By doing this, CloudFront will treat the content as dynamic and skip caching thecontent.
        1. Protect your API Gateway against direct access by configuring the distribution to include the origin custom header x-api-key, by setting theAPI keyvalue in API Gateway.
        1. Protect your backend from excess traffic by configuring standard or burst rate limits for each method in your RESTAPIs.

### Operational Techniques
When a key operational metric deviates substantially from the expectedvalue, an attacker may be attempting to target your application’s availability. If you’re familiar with the normal behavior of your application, you can more quickly take action when you detect an anomaly.
1. If you have architected your application by following the DDoS-resilient reference architecture, common infrastructure layer attacks will be blocked before reaching your application.
1. If you’re using AWS WAF, you can use CloudWatch to monitor and alarm on increases in requests that you’ve set in WAF to be allowed, counted, or blocked. This allows you to receive a notification if the level of traffic exceeds what your application can handle.

For a description of Amazon CloudWatch metrics that are commonly used to detect and react to DDoS attacks, see the table in the [DDoS Visibility](https://docs.aws.amazon.com/whitepapers/latest/aws-best-practices-ddos-resiliency/visibility.html)
### Overall Security Posture
Execute a [Self-Service Security Assessment](https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) against the environment to further identify other risks and potentially other public exposure not identified throughout this playbook.

## Lessons Learned
`This is a place to add items specific to your company that do not need "fixing", but are important to know when executing this playbook in tandem with operational and business requirements.`

## Addressed Backlog Items
- As an Incident Responder I need a documented escalation path both internally and externally to AWS

## Current Backlog Items
