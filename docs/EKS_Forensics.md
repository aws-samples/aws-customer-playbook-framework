# Incident Response Playbook: EKS Forensics
This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2024 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

## Points of Contact

Author: `Author Name`
Approver: `Approver Name`
Last Date Approved:

## Executive Summary
This playbook outlines the process to perform forensics on EKS resources that have been identified as malicious or have indicators of compromise that warrant additional investigation.

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
[AWS Cloud Adoption Framework Security Perspective](https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* **Directive**
* **Detective**
* **Responsive**
* **Preventative**

![Image](/images/aws_caf.png)
* * *

### Response Steps
1. **[PREPARATION]** Enable Logging
2. **[PREPARATION]** Establish a forensic environment
3. **[PREPARATION]** Ensure there is a capture mechanism for forensic artifacts
4. **[PREPARATION]** Implement a training program
5. **[DETECTION AND ANALYSIS]** Metadata Acquisition
6. **[DETECTION AND ANALYSIS]** Memory Acquisition
7. **[DETECTION AND ANALYSIS]** Memory Analysis
8. **[DETECTION AND ANALYSIS]** Disk Acquisition
9.  **[CONTAINMENT]** Isolate affected resources immediately
10. **[RECOVERY]** Perform recovery processes as appropriate

***The response steps follow the Incident Response Life Cycle from [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

![Image](/images/nist_life_cycle.png)***

### Incident Classification & Handling
* **Tactics, techniques, and procedures**: Tools: Forensics
* **Category**: Forensics
* **Resource**: EKS, EC2, VPC
* **Indicators**: Cyber Threat Intelligence, Third Party Notice, Cloudwatch Metrics, GuardDuty Alerts
* **Log Sources**: Endpoint, CloudTrail, CloudWatch
* **Teams**: Security Operations Center (SOC), Forensic Investigators, Cloud Engineering

## Incident Handling Process
### The incident response process has the following stages:
* Preparation
* Detection & Analysis
* Containment & Eradication
* Recovery
* Post-Incident Activity

## Escalation Procedures
- `I need a business decision on when forensics should be conducted`
- `Who is monitoring the logs/alerts, receiving them and acting upon each?`
- `Who gets notified when an alert is discovered?`
- `When do public relations and legal get involved in the process?`
- `When would you reach out to AWS Support for help?`

## Preparation

This playbook references and integrates, where possible, with [Prowler](https://github.com/toniblyx/prowler) which is a command line tool that helps you with AWS security assessment, auditing, hardening and incident response.

It follows guidelines of the CIS Amazon Web Services Foundations Benchmark (49 checks) and has more than 100 additional checks including related to GDPR, HIPAA, PCI-DSS, ISO-27001, FFIEC, SOC2 and others.

This tool provides a rapid snapshot of the current state of security within a customer environment. Additionally, [AWS Security Hub](https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) provides for automated compliance scanning and can [integrate with Prowler](https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Enable Logging
- GuardDuty Logs
  - GuardDuty automatically logs its findings, but you need to ensure it's enabled and configured correctly
- EKS Audit Log Monitoring
  - Enable control plane logging
    - Go to the EKS console
    - Select your cluster
    - Under the "Logging" tab, enable "audit" logs
  - Use fluentd for node-level logging
    - Deploy fluentd as a DaemonSet in your EKS cluster
    - Configure fluentd to collect container logs and send them to CloudWatch Logs or another log aggregation service

### Establish a forensic environment
- Considerations for a forensic environment are documented in the AWS Well-Architected framework [SEC10-BP03](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/sec_incident_response_prepare_forensic.html)
- **NOTE**: It is recommended to have a forensics VPC setup within each region where forensics may need to be conducted against production resources.

### Ensure there is a capture mechanism for forensic artifacts
- AWS Systems Manager (AWS SSM) can be leveraged to run commands or scripts across your EKS nodes.
  - This can be used to trigger memory captures, run other forensic tools, or initiate EBS snapshots.

### Training
- `What is the expectation of knowledge for personnel with forensics responsiblities? (knowledge/skills/abilities)`
- `What forensics training and procedures do my personnel receive?`
- `What regulatory requirements for forensics apply to my company?`
- `What training is in place for analysts within the company to become familiar with AWS API (command-line environment), EKS, EC2, and other AWS services?`

## Detection and Analysis

### Potential Sources of Alerts
- GuardDuty Alerts
- EKS Audit Logs

### Potential AWS GuardDuty Findings
- CredentialAccess:Kubernetes/SuccessfulAnonymousAccess
- CredentialAccess:Kubernetes/AnomalousBehavior.SecretsAccessed
- Discovery:Kubernetes/SuccessfulAnonymousAccess
- Execution:Kubernetes/AnomalousBehavior.ExecInPod
- Execution:Kubernetes/AnomalousBehavior.WorkloadDeployed
- Persistence:Kubernetes/AnomalousBehavior.WorkloadDeployed!ContainerWithSensitiveMount
- PrivilegeEscalation:Kubernetes/AnomalousBehavior.RoleBindingCreated
- PrivilegeEscalation:Kubernetes/AnomalousBehavior.RoleCreated
- PrivilegeEscalation:Kubernetes/AnomalousBehavior.WorkloadDeployed!PrivilegedContainer

### Metadata Acquisition
#### Identify all Pods associated with a Workload/Deployment:
    ```
    kubectl get pods -l app=<deployment-name>
    -OR-
    selector=$(kubectl get deployment <deployment-name> -o jsonpath='{.spec.selector.matchLabels}' | jq -r 'to_entries | map("\(.key)=\(.value)") | join(",")')
    kubectl get pods -l $selector
#### Identify all Pods using a specific Container Image:
    ```
    IMAGE=“<name_of_image>”
    kubectl get pods --all-namespaces -o json | jq -r --arg image "$IMAGE" '.items[] | select(.spec.containers[] | .image == $image) | "\(.metadata.name) \(.metadata.namespace) \(.spec.nodeName)"'
#### Identify all Pods associated with a specific Service Account:
    ```
    kubectl get pods -n <namespace> -o json | jq -r '.items[] | select(.spec.serviceAccount == "<service account name>") | "\(.metadata.name) \(.spec.nodeName)"'
#### Identify a Service’s IP:
    ```
    kubectl get service [--all-namespaces, -n <namespace>]
#### List all Pods in a specific Namespace with Cluster IP and Worker Node names:
    ```
    kubectl get pods -n <namespace> --show-labels -o [wide, json]
#### List specific Pod information in a specific Namespace with Cluster IP and Worker Node names:
    ```
    kubectl get pods <pod-name> -n <namespace> --show-labels -o [wide, json]
    kubectl get pods <pod-name> -n <namespace> -o=jsonpath='{.spec.nodeName}{"\n"}'
#### Label the affected Pod:
    ```
    kubectl label pods -n <namespace> <pod-name> status=PRIVILEGED-POD
#### Acquire Node Information (Name and Instance ID):
    ```
    kubectl get nodes <node-name> -n <namespace> --show-labels -o custom-columns=NAME:.metadata.name,INSTANCEID:.spec.providerID
    -OR-
    kubectl get nodes <node-name> -n <namespace> --show-labels  -o custom-columns=NAME:.metadata.name,INSTANCEID:.spec.providerID | sed -e 's/aws:.*\///g'
#### Enable Termination Protection on the Worker Node:
  - Identify the EC2 instance and enable Termination Protection
  - https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html#Using_ChangingDisableAPITermination
#### Label the affected Worker Node:
  - https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#label
    ```
    kubectl label node <node-name> status=quarantine
    ```

### Memory Acquisition
It is best practice to collect memory for the entire node.

#### Save container-level state before evidence is altered
  - Example for docker
    - docker top CONTAINER for processes running
    - docker logs CONTAINER for daemon level held logs
    - docker inspect CONTAINER for various information about the container

#### Acquire Memory from Worker Node:
  - https://aws-quickstart.github.io/cdk-eks-blueprints/addons/ssm-agent/
  - https://github.com/aws-samples/eks-security-compromised-cluster-remediation/tree/main/Containment/forensics
```
# Run the following commands on the impacted node
sudo curl -LO --output-dir /usr/local/bin https://github.com/microsoft/avml/releases/download/v0.3.0/avml
sudo chmod +x /usr/local/bin/avml
sudo /usr/local/bin/avml ${local_dest_dir}/memory.dmp
```

### Memory Analysis
#### Get process tree of node
```
python3 vol.py -f {MEM_DUMP} linux.pstree
```
#### Identify relevant PIDs
When examining a process list (pslist) in Kubernetes and trying to map Process IDs (PIDs) to specific pods, you'll need to combine information from the node and the container runtime. Here are several approaches to accomplish this:
```
# Use the container id
cat /proc/<PID>/cgroup | grep docker

# Use crictl to map the container ID to a pod
crictl pods --container <container-id>


# Use Kubernetets Node API
kubectl get --raw /api/v1/nodes/<node-name>/proxy/containerLogs/

# Use nsenter
nsenter -t $PID -u hostname
```
#### Examine specific PIDs memory space for linked/referenced/open files
```
python3 vol.py -f {MEM_DUMP} linux.proc --pid {PID}
```
#### Dump specific memory space for pod
```
python3 vol.py -o {OUTPUT_DIR} -f {MEM_DUMP} linux.proc --pid {PID} --dump
```

### Disk Acquisition

Create an EBS snapshot of the target node. For additional information and steps to take an EBS snapshot, see [Create Amazon EBS snapshots](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-creating-snapshot.html).

## Containment

### Isolate affected resources immediately
Once you suspect that a pod has been compromised, you want to isolate the pod without alerting the attacker. Cordoning the node that the compromised pod is running on will prevent new pods from being scheduled onto that node. For additional information on cordoning, see [Manual Node Administration](https://kubernetes.io/docs/concepts/architecture/nodes/#manual-node-administration).

To cordon a node, execute the following commands:
```
kubectl cordon <NODE_NAME>
```

## Eradication
There are no steps for this process applicable to this runbook.

## Recovery
Upon conclusion of forensics activities, and following issuance of the final report you should terminate the forensics EC2 instance and delete the EBS snapshot **UNLESS** there is a legal or regulatory requirement to maintain the data.

## Lessons Learned
`This is a place to add items specific to your company that do not need "fixing", but are important to know when executing this playbook in tandem with operational and business requirements.`

## Addressed Backlog Items
- As an Incident Responder I need a runbook to conduct EKS Forensics
- As an Incident Responder I need a business decision on when EKS forensics should be conducted
- As an Incident Responder I need to have logging enabled in all regions that are enabled regardless of intention of use

## Current Backlog Items
