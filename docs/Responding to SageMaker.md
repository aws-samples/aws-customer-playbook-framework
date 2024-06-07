# Incident Response Playbook: Responding to SageMaker Security Events
This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2024 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.


## Points of Contact

Author: `Author Name` \
Approver: `Approver Name` \
Last Date Approved:

## Executive Summary
As part of our ongoing commitment to customers, AWS is providing this security incident response playbook that describes the steps needed to investigate security events where Amazon SageMaker is either the source or a target of unauthorized use within your AWS account(s). The purpose of this document is to provide prescriptive guidance on the actions to take once you suspect a security event has taken place.

![Image](/images/nist_life_cycle.png)

*Aspects of AWS incident response*


### Amazon SageMaker

Amazon SageMaker is a fully managed machine learning (ML) service. With SageMaker, data scientists and developers can quickly and confidently build, train, and deploy ML models into a production-ready hosted environment. It provides a UI experience for running ML workflows that makes SageMaker ML tools available across multiple integrated development environments (IDEs).

With SageMaker, you can store and share your data without having to build and manage your own servers. With built-in support for bring-your-own-algorithms and frameworks, SageMaker offers flexible distributed training options that adjust to specific workflows. For additional information, review the developer guide for Amazon SageMaker here: https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html



## Preparation
Proactively prepare your environment by implementing preventative ([Service Control Policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)) and detective controls (See the Detection section for Config Rules).


**Preventative (SCP)**

* VPC Deployment
    * For example, the following SCP will prevent users from launching any notebooks, training, or processing jobs unless a VPC is specified.

```
 {
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "VPCDeployment",
      "Effect": "Deny",
      "Action": [
        "sagemaker:CreateHyperParameterTuningJob",
        "sagemaker:CreateModel",
        "sagemaker:CreateNotebookInstance",
        "sagemaker:CreateProcessingJob",
        "sagemaker:CreateTrainingJob"
      ],
      "Resource": [
        "*"
      ],
      "Condition": {
        "Null": {
          "sagemaker:VpcSecurityGroupIds": "true",
          "sagemaker:VpcSubnets": "true"
        }
      }
    }
  ]
}
```


* Enforce job encryption

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyUnencryptedVolumes",
      "Effect": "Deny",
      "Action": [
        "sagemaker:CreateHyperParameterTuningJob",
        "sagemaker:CreateTrainingJob",
        "sagemaker:CreateEndpointConfig",
        "sagemaker:CreateTransformJob"
      ],
      "Resource": [
        "*"
      ],
      "Condition": {
        "Null": {
          "sagemaker:VolumeKmsKey": [
            "true"
          ]
        }
      }
    }
  ]
}
```
* Enforce inter-container traffic encryption
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyUnencryptedTraffic",
      "Effect": "Deny",
      "Action": [
        "sagemaker:CreateTrainingJob",
        "sagemaker:CreateHyperParameterTuningJob"
      ],
      "Resource": [
        "*"
      ],
      "Condition": {
        "Bool": {
          "sagemaker:InterContainerTrafficEncryption": "false"
        }
      }
    }
  ]
}
```

* Enforce network isolation
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyNotIsolated",
      "Effect": "Deny",
      "Action": [
        "sagemaker:CreateTrainingJob",
        "sagemaker:CreateHyperParameterTuningJob",
        "sagemaker:CreateModel"
      ],
      "Resource": "*",
      "Condition": {
        "Bool": {
          "sagemaker:NetworkIsolation": "false"
        }
      }
    }
  ]
}
```
* Restricting notebook pre-signed URL to IPs
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "RestrictUrlToIp",
      "Effect": "Deny",
      "Action": "sagemaker:CreatePresignedNotebookInstanceUrl",
      "Resource": "*",
      "Condition": {
        "ForAllValues:NotIpAddress": {
          "aws:SourceIp": [
            "[ENTER_PUBLIC_IP_ADDRESS]"
          ]
        }
      }
    }
  ]
}
```
* Disable Internet Access
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyDirectInternet",
      "Effect": "Deny",
      "Action": "sagemaker:CreateNotebookInstance",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "sagemaker:DirectInternetAccess": [
            "Enabled"
          ]
        }
      }
    }
  ]
}
```

* Disable Root access in SageMaker Notebooks
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "SageMakerDenyRootAccess",
      "Effect": "Deny",
      "Action": [
        "sagemaker:CreateNotebookInstance",
        "sagemaker:UpdateNotebookInstance"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "sagemaker:RootAccess": [
            "Enabled"
          ]
        }
      }
    }
  ]
}
```
* Restrict the instance types that can be started by users
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "SageMakerLimitInstanceTypes",
      "Effect": "Deny",
      "Action": "sagemaker:CreateNotebookInstance",
      "Resource": "*",
      "Condition": {
        "ForAnyValue:StringNotLike": {
          "sagemaker:InstanceTypes": [
            "[EXAMPLE_INSTANCE_TYPES]",
            "ml.c5.xlarge",
            "ml.m5.xlarge",
            "ml.t3.medium"
          ]
        }
      }
    }
  ]
}
```

* Similarly, for Studio, see the following sample policy. Note that administrators need to allow the system instance for the default Jupyter Server apps.
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "SageMakerAllowedInstanceTypes",
      "Effect": "Deny",
      "Action": [
        "sagemaker:CreateApp"
      ],
      "Resource": "*",
      "Condition": {
        "ForAnyValue:StringNotLike": {
          "sagemaker:InstanceTypes": [
            "ml.c5.large",
            "ml.m5.large",
            "ml.t3.medium",
            "system"
          ]
        }
      }
    }
  ]
}
```


## Detection

### SageMaker Checks

### AWS Config

AWS Config has several [managed rules to evaluate SageMaker](https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
* sagemaker-endpoint-configuration-kms-key-configured
* sagemaker-endpoint-config-prod-instance-count
* sagemaker-notebook-instance-inside-vpc
* sagemaker-notebook-instance-kms-key-configured
* sagemaker-notebook-instance-root-access-check
* sagemaker-notebook-no-direct-internet-access

### CloudTrail Events

In the event of unauthorized access to your environment, see below for possible scenarios tied to relevant SageMaker API calls as a quick reference (please note that this is not a complete list of SageMaker API calls):


**Data/Model Exfiltration:**
- Copying or downloading sensitive data from SageMaker data stores or model artifacts.
- Extracting model parameters or training data, potentially exposing intellectual property or personal information.

    <ins>Example API calls:</ins>
    - `DescribeModelPackage` to retrieve information about model packages.
    - `DescribeTrainingJob` to access details of training jobs and their output data.
    - `GetModelPackageModelMetrics` to retrieve model metrics and potentially sensitive data.



**Data Poisoning:**
* Modifying or poisoning trained models by injecting malicious data or adversarial examples.
* Deploying compromised models to SageMaker endpoints, leading to incorrect predictions or malicious outputs.

    <ins>Example API calls:</ins>
    - `CreateModelPackage` or `UpdateModelPackage` to deploy a compromised model package.
    - `CreateTransformJob` to execute a transform job with a poisoned model.
    - `CreateEndpointConfig` and `CreateEndpoint` to deploy a malicious model to an endpoint.
    - `CreateTrainingJob` or `UpdateTrainingJob` to inject malicious data into training jobs.



**Resource Misuse:**
* Launching unauthorized SageMaker notebooks or instances for cryptomining or other malicious activities.
* Using SageMaker resources as entry points or pivot points for lateral movement within the AWS environment.

    <ins>Example API calls:</ins>
    - `CreateNotebookInstance` or `UpdateNotebookInstance` to launch unauthorized notebook instances.
    - `CreateTrainingJob` or `CreateHyperParameterTuningJob` to initiate excessive training jobs.
    - `CreateEndpoint` or `UpdateEndpoint` to create or modify endpoints for unauthorized purposes.

**Denial of Service (DoS):**
* Exhausting SageMaker resources (e.g., compute instances, storage) by launching excessive training jobs or endpoints.
* Overwhelming SageMaker APIs or services with a high volume of requests, leading to service disruptions.

    <ins>Example API calls:</ins>
    - `CreateTrainingJob` or `CreateHyperParameterTuningJob` to launch numerous training jobs and exhaust resources.
    - `CreateEndpoint` or `UpdateEndpoint` to create multiple endpoints and consume excessive compute resources.

**Configuration Changes:**
* Modifying SageMaker roles, policies, or permissions to escalate privileges or grant unauthorized access.
* Altering SageMaker VPC configurations, security groups, or network settings to bypass security controls.

    <ins>Example API calls:</ins>
    - `CreateRole` or `UpdateRole` to modify SageMaker roles and permissions.
    - `CreateNotebookInstanceLifecycleConfig` or `UpdateNotebookInstanceLifecycleConfig` to alter notebook instance configurations.
    - `CreateEndpointConfig` or `UpdateEndpointConfig` to change endpoint configurations or security settings.

**Log Tampering:**
* Modifying or deleting SageMaker logs or audit trails to cover tracks and hinder incident investigation.
* Injecting false log entries to mislead security analysts or hide malicious activities.

    <ins>Example API calls:</ins>
    - `PutModelPackageModelMetrics` to inject false model metrics into logs.
    - `StopTrainingJob` or `StopTransformJob` to potentially modify or delete log data.

**Malware Deployment:**
* Deploying malware or backdoors within SageMaker notebooks or instances for persistent access or data theft.
* Using SageMaker resources to distribute malware or launch attacks against other systems or networks.

    <ins>Example API calls:</ins>
    - `CreateNotebookInstance` or `UpdateNotebookInstance` to launch notebook instances with malware.
    - `CreateModelPackage` or `UpdateModelPackage` to deploy model packages containing malicious code.

**Credential Theft:**
* Stealing AWS credentials or SageMaker API keys stored within notebooks or instances.
* Using stolen credentials to gain further unauthorized access to other AWS resources or services.

    <ins>Example API calls:</ins>
    - `DescribeNotebookInstance` or `DescribeTrainingJob` to potentially access stored credentials or API keys.
    - `GetModelPackageModelMetrics` or `DescribeModelPackage` to retrieve sensitive information or credentials.

**Cryptojacking:**
* Hijacking SageMaker compute resources (e.g., instances, endpoints) for unauthorized cryptocurrency mining activities.
* Consuming excessive compute resources and potentially leading to service disruptions or increased costs.

    <ins>Example API calls:</ins>
    - `CreateNotebookInstance` or `UpdateNotebookInstance` to launch instances for cryptocurrency mining.
    - `CreateTrainingJob` or `CreateHyperParameterTuningJob` to initiate compute-intensive jobs for mining purposes.


**Note: It's important to note that these API calls can also be used for legitimate purposes, but in the context of unauthorized access, they could be misused to perform risky actions. Implementing robust access controls, monitoring, and auditing mechanisms is crucial to detect and prevent such misuse of SageMaker APIs and resources.**

### Understanding SageMaker Log Entries

The screenshots below provide a visual aid for an Incident Responder to assist in the interpretation of events found during an investigation. Each image below represents the action/s taken that match with the event name that is logged

---

**CreateDomain**
<details>
  <summary>Expand Screenshot</summary>
-
Creates a Domain. A domain consists of an associated Amazon Elastic File System volume, a list of authorized users, and a variety of security, application, policy, and Amazon Virtual Private Cloud (VPC) configurations. Users within a domain can share notebook files and other artifacts with each other.

![CreateDomain](/images/sagemaker-01.png)
</details>

---

**SageMaker Domain details**
<details>
  <summary>Expand Screenshot</summary>

Amazon SageMaker domain supports SageMaker machine learning (ML) environments. A SageMaker domain is composed of the following entities: Domain, User Profile, Shared Space, App


![DomainDetails](/images/sagemaker-02.png)
![DomainDetails2](/images/sagemaker-03.png)

</details>

---


**CloudTrail event for CreateDomain**

<details>
  <summary>Expand Screenshot</summary>

Note that the `CreateDomain` event in Cloudtrail has all of the following information: VPC, subnets, execution role, apps, etc.

![SageMaker Domain with associated VPC, execution role, apps, etc](/images/sagemaker-04.png)


</details>

---

**CloudTrail event for CreateEndpoint**

<details>
  <summary>Expand Screenshots</summary>

Note that the  `CreateEndpoint` event for SageMaker in CloudTrail is called by `SageMaker-ExecutionRole` service role

![Create endpoint example](/images/sagemaker-05.png)

</details>

---



## Analysis

In the event of an incident, in addition to investigating the indicators of compromise, threat actor, timeframe, etc., here are some additional questions to consider once it has been confirmed that this is an incident relating to SageMaker resources:

1. Which SageMaker resources were accessed without authorization? (e.g., notebooks, models, endpoints, data stores)
2. How was the unauthorized access gained? (e.g., compromised credentials, misconfigured permissions, exploited vulnerabilities)
3. What actions were performed on the affected SageMaker resources during the unauthorized access?
4. Were any models or data exfiltrated or tampered with?
5. If models were accessed, is there a risk of model poisoning or adversarial attacks?
6. Were any new resources (e.g., notebooks, endpoints) created or modified during the unauthorized access?
7. Were any SageMaker APIs or SDKs used during the unauthorized access, and what actions were performed through them?
8. Were any SageMaker logs or audit trails modified or deleted to cover tracks?
9. Were any SageMaker roles or IAM policies modified or misused during the incident?
10. Were any SageMaker VPC configurations or network settings altered?
11. Were any SageMaker notebooks or instances used as entry points or pivot points for further unauthorized access?
12. Were any SageMaker resources used to launch attacks or malicious activities against other AWS resources or external systems?
13. What is the potential impact of the unauthorized access on the confidentiality, integrity, and availability of SageMaker resources and associated data?
14. How can the affected SageMaker resources be securely isolated, backed up, and potentially recovered or rebuilt?
15. What specific SageMaker security best practices or configurations were not followed, leading to the unauthorized access?

## Containment and Eradication

If any resources are created by an unauthorized user or resource were not authorized to be created, follow the following instructions on how to delete/modify created resources or permissions:

- [How to delete a SageMaker domain (console)](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-delete-domain.html#gs-studio-delete-domain-studio)
- [How to delete a SageMaker domain (CLI)](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-delete-domain.html#gs-studio-delete-domain-cli)
- [How to delete a SageMaker Endpoint](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-endpoint)
- [How to delete a SageMaker Endpoint Configuration](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-endpoint-config)
- [How to delete a SageMaker Model](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-model)
- [Remove Root Access from SageMaker Notebooks](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-root-access.html) (Go to desired notebook instance/Stop Instance/Once completed, click Edit/Under Permissions, select Disable/Update Notebook Instance/Run Instance)


## Addressed Backlog Items
- As an Incident Responder, I need to be able to monitor all critical SageMaker events
- As an Incident Responder, I need a playbook on querying SageMaker Cloudtrail events at scale

## Current Backlog Items

## Appendix - Best Practices

**Strongly Recommended**

- [Deploy in an isolated VPC](https://docs.aws.amazon.com/sagemaker/latest/dg/infrastructure-connect-to-resources.html)
- Use VPC endpoint to access resources
  - [Sagemaker notebooks should restrict access to connections from within the VPC](https://docs.aws.amazon.com/sagemaker/latest/dg/infrastructure-connect-to-resources.html)
- Use security groups and NACLs to control traffic into and out of your environment
- [Inter-container traffic encryption for training jobs with several compute instances](https://docs.aws.amazon.com/sagemaker/latest/dg/train-encrypt.html)
- [Enable encryption at rest using KMS](https://docs.aws.amazon.com/sagemaker/latest/dg/encryption-at-rest.html)
- [Disable root access to notebooks if it is not needed](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-root-access.html)
- [Lifecycle Configuration Best Practices](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-lifecycle-config-install.html)
  - Create an allow list of packages that team can use to reduce risk of malicious code running
- [Least privilege using IAM roles and resource based policies (ex. bucket policies for accessing S3 bucket data), leverage ML Governance](https://docs.aws.amazon.com/sagemaker/latest/dg/governance.html)
- [Use IAM Identity Center](https://docs.aws.amazon.com/sagemaker/latest/dg/domain-user-profile-add-remove.html)
- [Store and rotate credentials in Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/integrating-sagemaker.html)
- [Monitor model input and output using SageMaker Model Monitor](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-faqs.html)
- [Enable CloudTrail S3 data events logging for S3 data and model artifacts auditing](https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html)
- [Enable SageMaker Experiments and leverage version control for model artifacts](https://docs.aws.amazon.com/sagemaker/latest/dg/experiments.html)
- [Enable VPC Flow Logs to monitor network traffic in your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)
- [Use CodeArtifact to download libraries/packages needed from the internet](https://aws.amazon.com/blogs/machine-learning/secure-aws-codeartifact-access-for-isolated-amazon-sagemaker-notebook-instances/)
- [CloudWatch can also be used to monitor SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html)

**Encouraged**

- [Use Amazon Macie to protect sensitive S3 data](https://docs.aws.amazon.com/macie/latest/user/data-classification.html)
- [Use Service Catalog to vend pre-vetted SageMaker resources, such as notebooks to restrict instance size and enforce the use of secure configurations](https://aws.amazon.com/blogs/machine-learning/automate-a-centralized-deployment-of-amazon-sagemaker-studio-with-aws-service-catalog/)
