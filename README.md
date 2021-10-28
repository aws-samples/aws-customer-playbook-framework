# Playbooks
The contents of this library are provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2021 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

## Contributing to the Incident Response Playbooks

### Impostor Syndrome Disclaimer
Before we get into the details: We want your help. No, really.

There may be a little voice inside your head that is telling you that you're not ready to contribute to playbooks; that your skills aren't nearly good enough to contribute. What could you possibly offer a project like this one?
We assure you -- the little voice in your head is wrong. If you can write code at all or have experience with incident response, then we need your contributions! Writing perfect playbooks isn't the measure of a good responder (that would disqualify all of us!); it's trying to create something, making mistakes, and learning from those mistakes. That's how we all improve.

We've provided a clear [playbook creation guide](./docs/Playbook_Creation_Process.md). This outlines the process that you'll need to follow to get a playbook developed and approved for use with Ziplines. By making expectations and process explicit, we hope it will make it easier for you to contribute.
And you don't just have to write code. You can help out by writing documentation, tests, or even by giving feedback about this work. (And yes, that includes giving feedback about everything in this README)

## Playbook Index
* [Playbook Project Disclaimer](./Disclaimer.md)

### Preparation
* [Rationalization of Security Incident Handling](./docs/rationalization_of_security_incident_handling.md)
* [Playbook Development Guide](./docs/Playbook_Development_Guide.md)

#### Administrative
* [Playbook Creation Process](./docs/Playbook_Creation_Process.md)
* [Resource Glossary](./docs/Resource_Glossary.md)

#### Communications 

### Response Scenarios
#### 100-200 Level Scenarios
* [Compromised IAM Credential(s)](./docs/Compromised_IAM_Credentials.md)
* [Denial of Service / Distributed Denial of Service](./docs/Denial_of_Service.md)
* [Inappropriate Public Resources: S3)](./docs/S3_Public_Access.md)
* [Inappropriate Public Resources: RDS)](./docs/RDS_Public_Access.md)
* [Unauthorized Network Changes](./docs/Unauthorized_Network_Changes.md)

#### 300-400 Level Scenarios
* [Bitcoin and Cryptojacking](./docs/Cryptojacking.md)
* Responding to Ransom in AWS
    * [Amazon Elastic Computing (EC2) Linux/Unix](./docs/Ransom_Response_EC2_Linux.md)
    * [Amazon Elastic Computing (EC2) Microsoft Windows](./docs/Ransom_Response_EC2_Windows.md)
    * [Amazon Relational Database Service (RDS)](./docs/Ransom_Response_RDS.md)
    * [Amazon Simple Storage Service (S3)](./docs/Ransom_Response_S3.md)

### AWS Analysis Tools
* [Analyzing VPC Flow Logs with AWS Athena](./docs/Analyzing_VPC_Flow_Logs.md)
* [EC2 Forensics](./docs/EC2_Forensics.md)

## References
* [AWS Cloud Adoption Framework](https://aws.amazon.com/professional-services/CAF/)
* [AWS Security Incident Response Guide Wiki](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)
* [AWS Security Incident Response Guide Downloadable](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/aws-security-incident-response-guide.pdf)
* [AWS Incident Response Blogs](https://aws.amazon.com/blogs/security/tag/incident-response/)
* [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)
* [AWS Well Architected Resources](https://aws.amazon.com/architecture/well-architected/)


## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License Summary

The documentation is made available under the Creative Commons Attribution-ShareAlike 4.0 International License. See the LICENSE file.

The sample code within this documentation is made available under the MIT-0 license. See the LICENSE-SAMPLECODE file.
