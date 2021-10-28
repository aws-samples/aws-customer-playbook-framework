# Resource Glossary
This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2021 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

[[_TOC_]]

## Overall References
### [AWS Cloud Adoption Framework](https://aws.amazon.com/professional-services/CAF/)
AWS Professional Services created the AWS Cloud Adoption Framework (AWS CAF) to help organizations develop and execute efficient and effective plans for their cloud adoption journey. The guidance and best practices provided by the framework help you build a comprehensive approach to cloud computing across your organization, and throughout your IT lifecycle. Using the AWS CAF helps you realize measurable business benefits from cloud adoption faster and with less risk.
### [AWS Security Incident Response Guide Wiki](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)
- [AWS Security Incident Response Guide Downloadable](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/aws-security-incident-response-guide.pdf)
This guide presents an overview of the fundamentals of responding to security incidents within a customer’s AWS Cloud environment. It focuses on an overview of cloud security and incident response concepts, and identifies cloud capabilities, services, and mechanisms that are available to customers who are responding to security issues.

This paper is intended for those in technical roles and assumes that you are familiar with the general principles of information security, have a basic understanding of incident response in your current on-premises environments, and have some familiarity with cloud services. 

## Amazon Dashboards
### [Amazon Billing Dashboard](https://console.aws.amazon.com/billing/home#)
### [Amazon CloudTrail Dashboard](https://console.aws.amazon.com/cloudtrail)
### [Amazon CloudWatch console](https://console.aws.amazon.com/cloudwatch/)
### [Amazon EC2 console](https://console.aws.amazon.com/ec2/)
### [Amazon IAM console](https://console.aws.amazon.com/iam/)
### [Amazon Network Manager Dashboard](https://console.aws.amazon.com/networkmanager/home)
### [Amazon Orders and invoices](https://console.aws.amazon.com/billing/home?/paymenthistory/)
### [Amazon S3 console](https://console.aws.amazon.com/s3/)
### [Amazon RDS console](https://console.aws.amazon.com/rds/)
### [Amazon VPC Dashboard](https://console.aws.amazon.com/vpc/home)
### [Amazon WAF & Shield console](https://console.aws.amazon.com/wafv2/shieldv2)

## Documentation and Guides
### [Amazon Athena and VPC Flow Logs](https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)
### [Amazon CloudWatch Events](https://docs.aws.amazon.com/codepipeline/latest/userguide/create-cloudtrail-S3-source-console.html)
### [Amazon Config](https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
### [AWS API describe-db-instances](https://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-instances.html)
### [AWS API list-buckets](https://docs.aws.amazon.com/cli/latest/reference/s3api/list-buckets.html)
### [AWS Documentation: Avoiding unexpected charges](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/checklistforunwantedcharges.html)
### [AWS Documentation: Blocking public access to your Amazon S3 storage](https://aws.amazon.com/s3/features/block-public-access/)
### [AWS Documentation: DDoS Visibility](https://docs.aws.amazon.com/whitepapers/latest/aws-best-practices-ddos-resiliency/visibility.html)
### [AWS Documentation: Managing S3 access with ACLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/acls.html)
### [AWS Documentation: MFA for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html)
### [AWS Documentation: RDS enhanced monitoring](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Monitoring.OS.html)
### [AWS Documentation: Requesting a credit in AWS Shield Advanced](https://docs.aws.amazon.com/waf/latest/developerguide/request-refund.html)
### [AWS Documentation: AWS Shield Documentation](https://docs.aws.amazon.com/waf/latest/developerguide/shield-chapter.html)
### [AWS Documentation: AWS Shield Advanced guide](https://docs.aws.amazon.com/waf/latest/developerguide/getting-started-ddos.html)
### [AWS Documentation: Terminate all my resources](https://aws.amazon.com/premiumsupport/knowledge-center/terminate-resources-account-closure/)
### [AWS Documentation: Using versioning in S3 buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html)
### [AWS Documentation: VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html)
### [Enforce Mandatory RDS Encryption](https://medium.com/@cbchhaya/aws-scp-to-mandate-rds-encryption-6b4dc8b036a)
### [Prevent Users from Modifying S3 Block Public Access Settings](https://asecure.cloud/a/scp_s3_block_public_access/)

## Tools
### [AWS Cost Anomaly Detection](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-ad.html)
AWS Cost Anomaly Detection is an AWS Cost Management feature that uses machine learning to continuously monitor your cost and usage to detect unusual spends. Using AWS Cost Anomaly Detection includes the following benefits:
- Receive alerts individually in aggregated reports. You can receive alerts in an email or an Amazon SNS topic.
- Evaluate your spend patterns using machine learning methods to minimize false positive alerts. For example, you can evaluate weekly or monthly seasonality and organic growth.
- Analyze and determine the root cause of the anomaly, such as account, service, Region, or usage type that is driving the cost increase.
- Configure how you need to evaluate your costs. You can choose whether you want to analyze all of your AWS services independently, or by member accounts, cost allocation tags, or cost categories.
### [Amazon Git Secrets](https://github.com/awslabs/git-secrets)
Git Secrets can scan merges, commits, and commit messages for secret information (that is, access keys). If Git Secrets detects prohibited regular expressions, it can reject those commits from being posted to public repositories.
### [Amazon Inspector](https://aws.amazon.com/inspector/)
Amazon Inspector is an automated security assessment service that helps improve the security and compliance of applications deployed on AWS. Amazon Inspector automatically assesses applications for exposure, vulnerabilities, and deviations from best practices.
### [Amazon Macie](https://aws.amazon.com/macie/)
Amazon Macie is a fully managed data security and data privacy service that uses machine learning and pattern matching to discover and protect your sensitive data in AWS. Macie’s alerts, or findings, can be searched and filtered in the AWS Management Console and sent to Amazon EventBridge, formerly called Amazon CloudWatch Events, for easy integration with existing workflow or event management systems, or to be used in combination with AWS services, such as AWS Step Functions to take automated remediation actions.
### [Amazon Security Hub](https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc)
AWS Security Hub gives you a comprehensive view of your security alerts and security posture across your AWS accounts. With Security Hub, you now have a single place that aggregates, organizes, and prioritizes your security alerts, or findings, from multiple AWS services, such as Amazon GuardDuty, Amazon Inspector, Amazon Macie, AWS Identity and Access Management (IAM) Access Analyzer, AWS Systems Manager, and AWS Firewall Manager, as well as from AWS Partner Network (APN) solutions.
### [Prowler](https://github.com/toniblyx/prowler)
Prowler is a command line tool that helps you with AWS security assessment, auditing, hardening and incident response. It follows guidelines of the CIS Amazon Web Services Foundations Benchmark (49 checks) and has more than 100 additional checks including related to GDPR, HIPAA, PCI-DSS, ISO-27001, FFIEC, SOC2 and others.
### [Self-Service Security Assessment](https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
The Self-Service Security Assessment is deployed using a simple AWS CloudFormation template that includes a dedicated Amazon Virtual Private Cloud (Amazon VPC) with two subnets, one NAT Gateway, one Amazon Elastic Compute Cloud (Amazon EC2) instance, and one Amazon Simple Storage Service (Amazon S3) bucket. Once deployed, open source projects Prowler and ScoutSuite are downloaded and installed within the Amazon EC2 instance and begin locally scanning AWS accounts using AWS APIs to run more than 256 point-in-time checks. The checks look at current AWS settings across services like AWS CloudTrail, Amazon CloudWatch, Amazon EC2, Amazon GuardDuty, AWS Identity and Access Management (AWS IAM), Amazon Relational Database Service (Amazon RDS), Amazon Route 53, and Amazon S3 and assesses them against security best practices.
## Training
### [AWS RE:INFORCE](https://reinforce.awsevents.com/faq/)
AWS re:Inforce is a cloud security conference designed to help you improve your security awareness and best practices. Join us for technical content focused on AWS products and services, a keynote featuring AWS Security leadership, and direct access to experts who can help expand your knowledge of cloud security and compliance. 
