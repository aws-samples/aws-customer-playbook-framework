# Rationalization of security incident alerting
This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2021 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

> “If you protect your paper clips and diamonds with equal vigor, you’ll soon have more paper clips and fewer diamonds.” attributed to former US Secretary of State Dean Rusk

Alerts are determined by the threat modeling of a workload during the development of security controls. Alert usage is defined by Responsive controls, however, its definition relies on the whole security perspective: Directive, Preventative, Detective, and Responsive. 

## Definitions

### Threat modeling:

* **Workload**: what you are trying to protect
* **Threat**: what you fear happening
* **Impact**: how business is affected when threat meets workload
* **Vulnerability**: what can facilitate the threat
* **Mitigation**: what controls are in place to compensate for vulnerability
* **Probability**: how likely is for the threat to happen with mitigations in place

**Prioritization of threats is a factor of impact and probability.**

### Security controls:

* **Directive** controls establish the governance, risk, and compliance models the environment will operate within.
* **Preventive** controls protect your workloads and mitigate threats and vulnerabilities. 
* **Detective** controls provide full visibility and transparency over the operation of your deployments in AWS. 
* **Responsive** controls drive remediation of potential deviations from your security baselines.


![Image](/images/image-caf-sec.png)

## To minimize alert fatigue and better handling of security events, the threat modeling context should take into account:

* Workload relevance (*) to the business.
* Data classification (*) of each workload component.
* Methodology consistently used such as STRIDE (Spoofing, Tampering, Info Disclosure, Repudiation, Denial of Service and Elevation of Privilege) 
* Cloud security readiness and maturity.
* Incident Response and Threat Hunting capacity.
* Auto-remediation capabilities.
* Incident handling automation maturity.


(*) ***Workload relevance and data classification*** are defined by the corporation’s risk framework initiative. Examples:

### Workload relevance: 

**High**: major monetary loss and image perception damage, long term business impact, low recovery success  
**Medium**: sustainable monetary loss and image perception damage, short term business impact, high recovery success  
**Low**: no measurable monetary loss and image perception damage, no business impact, recovery is not applicable  

### Data classification:

**Secret**: major monetary loss and image perception damage, long term business impact, low recovery success  
**Confidential**: sustainable monetary loss and image perception damage, short term business impact, high recovery success  
**Unclassified**: no measurable monetary loss and image perception damage, no business impact, recovery is not applicable  


## The goal of alert prioritization is to send them to the appropriate queue:

* Alert triage queue
* Threat hunting queue
* Archive queue


The process to define where the alert will end up is dependent on all the factors enumerated previously. A basic queuing decision is as follows:

### Alert triage queue:

1. Specific risk framework mandate, including but not limited to, industry regulation, statutory compliance, business requirement. In this scenario, the workload has **high** relevance or data is classified as **secret**.
2. Specific threat model requirement to start formal incident response process.
3. Auto-remediation failed for alerts where workload has **medium** or **high** relevance or data classification is **secret** or **confidential**.

### **Threat hunting queue**:

1. Auto-remediation successful for workload with **medium** or **high** relevance or data classification is **secret** or **confidential**.
2. Auto-remediation failed for workload with **low** relevance or data classification is **unclassified**.

### Archive queue:

1. Auto-remediation succeeded for workload with **low** relevance or data classification is **unclassified**.
