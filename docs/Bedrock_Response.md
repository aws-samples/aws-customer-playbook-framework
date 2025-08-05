# Security Playbook for Responding to Amazon Bedrock Security Events
This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2024 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

## Points of Contact

Author: `Author Name` \
Approver: `Approver Name` \
Last Date Approved:

## Executive Summary

As part of our ongoing commitment to customers, AWS is providing this
security incident response playbook that describes the steps needed to
investigate security events where Amazon Bedrock is either the source or a target of unauthorized use within your AWS account(s). The purpose of this document is to provide prescriptive guidance on the actions to take once you suspect a security event has taken place.

![Image](/images/nist_life_cycle.png)

*Aspects of AWS incident response*

## Preparation

In order to quickly and effectively execute incident response activities, it is crucial to prepare the people, processes, and technology within your organization. Review the [*AWS Security Incident Response Guide*](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/preparation.html), and implement the steps required to help ensure preparedness of all three domains.

## How to Engage AWS Support for Assistance

It is important that you notify AWS as soon as you suspect a compromised credential(s) within your AWS account or Organization. Here are the steps to engage AWS Support:

### Open an AWS Support Case

-   Login to your AWS account:
    -   This is the first AWS account that was impacted by the security event, to validate AWS account ownership.
    -   Note: If you are unable to access account, use [this form](https://support.aws.amazon.com/#/contacts/aws-account-support/) to submit a support request.
-   Select “*Services*”, “*Support Center*”, “*Create Case*”.
-   Select issue type “*Account and Billing”*.
-   Select the impacted Service and Category:
    -   Example:
        -   Service: Account
        -   Category: Security
-   Choose a Severity:
    -   Enterprise Support or On-Ramp Customers: *Critical Business Risk Question*.
    -   Business Support Customers: *Urgent Business Risk Question*.
-   Describe your question or issue:
    -   Provide a detailed description of the security issue you’re experiencing, the impacted resources, and the business impact.
        -   Time the security event was first recognized.
        -   Summary of the event timeline to this point.
        -   Artifacts you’ve collected (screenshots, log files).
        -   ARN of IAM Users or Roles that you suspect are compromised.
        -   Description of affected resources, and the business impact.
        -   Level of engagement in your organization (ex: “This security event has the visibility of the CEO and Board of Directors”).
        -   Optional: Add additional case contacts.
-   Select “*Contact Us*”.
    -   Preferred contact language (may be subject to availability)
    -   Preferred contact method: Web, Phone, or Chat (recommended)
    -   Optional: Additional case contacts
    -   *Click “Submit”*

-   **Escalations**: Please notify your AWS account team as soon as possible, so they may engage the necessary resources and escalate as needed.

**Note:** It is very important to verify your 'Alternate Security Contact' is defined for each AWS account. For more details, please refer to [this
article](https://aws.amazon.com/blogs/security/update-the-alternate-security-contact-across-your-aws-accounts-for-timely-security-notifications/).


## Amazon Bedrock

Amazon Bedrock is compromised of multiple components/services:

* Models - can be either foundation models from leading AI startups or custom models
* Inference - the process of generating an output from an input provided to a model
* Knowledge Base - allows you to amass data sources into a repository of information
* Agents - helps your end-users complete actions based on organization data and user input

When a user in an AWS account (either authorized or unauthorized), accesses Bedrock, they ultimately do so through either a Model, a Knowledge Base, or an Agent.  Any prompts issued and their responses will be issued in a Model Invocation Log provided this was set up previously (https://docs.aws.amazon.com/bedrock/latest/userguide/model-invocation-logging.html).  For additional information, review the user guide for Amazon Bedrock here: https://docs.aws.amazon.com/bedrock/

The image below portrays the relationship between Prompts, [Agents](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-how.html), and Knowledge Bases and can be a good way to visualize the data flow (taken from the [User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html))

![Agent Construction During Build-Time API Operations](/images/bedrock-01.png)


## Detection

### Notes and Considerations

* Bedrock is not available in all regions (2024-06-04).  See [link](https://docs.aws.amazon.com/bedrock/latest/userguide/bedrock-regions.html) for current region availability
* Model Invocation Logging is required to view prompts and responses sent to models.  This is a region level setting and is not default
* For CloudTrail, the eventSource = bedrock.amazonaws.com
* The ***InvokeModel*** API is a **read-only** event

### Event Names

The event names listed in the table below are a quick reference for types of events typically found in CloudTrail during the investigation of a security event involving Bedrock.  Of those listed in the table, the most common event names that are logged during normal model usage are:

* `GetFoundationModelAvailability`
* `InvokeModel`
* `InvokeModelWithResponseStream`

| **Event Names** | **readOnly** | **Description** |
| :-----------------| :-----------------| :-------------------------------- |
| ***Events related to Models*** | --- | --- |
| `GetFoundationModelAvailability` | TRUE | Retrieves the availability of a foundation model |
| `GetUseCaseForModelAccess` | TRUE | Retrieves a use case for a specified model |
| `InvokeModel` | TRUE | Invokes the specified Bedrock model to run inference using the input provided in the request body |
| `InvokeModelWithResponseStream` | TRUE | Invokes the specified Bedrock model to run inference using the input provided in the request body using chunking |
| `ListCustomModels` | TRUE | Lists Bedrock custom models that have been created |
| `ListFoundationModels` | TRUE | Lists the foundation models that can be used |
| `ListProvisionedModelThroughputs` | TRUE | Lists the provisioned capacities for models |
| --- | --- | --- |
| ***Events related to Model Invocation Logging*** | --- | --- |
| `DeleteModelInvocationLoggingConfiguration` | FALSE | Deletes an existing invocation logging configuration |
| `GetModelInvocationLoggingConfiguration` | TRUE | Retrieves the existing invocation logging configuration |
| `PutModelInvocationLoggingConfiguration` | FALSE | Creates an existing invocation logging configuration |
| --- | --- | --- |
| ***Events related to Knowledge Bases*** | --- | --- |
| `AssociateAgentKnowledgeBase` | FALSE | This action logs the association of a knowledge base during agent creation or after an agent has been created |
| `CreateDataSource` | FALSE | Creates a data source |
| `CreateKnowledgeBase` | FALSE | Creates a knowledge base |
| `DeleteKnowledgeBase` | FALSE | Deletes a knowledge base |
| `GetDataSource` | TRUE | Gets information about a data source |
| `GetKnowledgeBase` | TRUE | Retrieves an existing knowledge base |
| `ListDataSources` | TRUE | Lists available data sources |
| `ListKnowledgeBases` | TRUE | Lists existing knowledge bases |
| --- | --- | --- |
| ***Events related to Agents*** | --- | --- |
| `CreateAgent` | FALSE | Creates a new agent and a test agent alias pointing to the DRAFT agent version |
| `CreateAgentActionGroup` | FALSE | Creates an action group for an agent. An action group represents the actions that an agent can carry out for the customer by defining the APIs that an agent can call and the logic for calling them |
| `CreateAgentAlias` | FALSE | Creates a new alias for an agent |
| `DeleteAgent` | FALSE | Deletes an earlier created agent |
| `GetAgent` | TRUE | Retrieves an existing agent |
| `GetAgentAlias` | TRUE | Retrieves an existing alias |
| `InvokeAgent` | FALSE | Sends a prompt for the agent to process and respond to |
| `ListAgentActionGroups` | TRUE | Lists the action groups for an agent and information about each one |
| `ListAgentAliases` | TRUE | Lists the aliases of an agent and information about each one |
| `ListAgentKnowledgeBases` | TRUE | Lists knowledge bases associated with an agent and information about each one |
| `ListAgents` | TRUE | Lists existing agents |
| `ListAgentVersions` | TRUE | Lists the versions of an agent and information about each version |
| `PrepareAgent` | FALSE | Prepares an existing Amazon Bedrock Agent to receive runtime requests |
| --- | --- | --- |
| ***Events related to Job Ingestion*** | --- | --- |
| `GetIngestionJob` | TRUE | Gets information about a ingestion job, in which a data source is added to a knowledge base |
| `ListIngestionJobs` | TRUE | Lists the ingestion jobs for a data source and information about each of them |
| `StartIngestionJob` | FALSE | Begins an ingestion job, in which a data source is added to a knowledge base |
| --- | --- | --- |
| ***Events related to RAG*** | --- | --- |
| `Retrieve` | TRUE | Retrieves ingested data from a knowledge base (considered data events and will not show up in CloudTrail) |
| `RetrieveAndGenerate` | FALSE | Sends user input to perform retrieval and generation (considered data events and will not show up in CloudTrail) |

### Bedrock Usage Screenshots

The screenshots below can provide a visual aid for an Incident Responder to assist in the interpretation of events found during an investigation.  Each image below represents the action/s taken that match with the event name that is logged

---

**GetFoundationModelAvailability**
<details>
  <summary>Expand Screenshot</summary>
-
This event is first logged when browsing to the main Amazon Bedrock page

![GetFoundationModelAvailability](/images/bedrock-02.png)

</details>

---

**ListFoundationModels** and **ListCustomModels**
<details>
  <summary>Expand Screenshot</summary>
-
These events are logged when browsing to the 'Base models' and 'Custom models' pages

![ListFoundationModels and ListCustomModels](/images/bedrock-03.png)

</details>

---

**InvokeModel**
<details>
  <summary>Expand Screenshots</summary>
-
This event is logged upon model invocation.

![Model invocation example](/images/bedrock-04.png)

Sample CloudTrail event record for model invocation:

![CloudTrail for InvokeModel](/images/bedrock-05.png)

</details>

---

**InvokeModel** - additional examples
<details>
  <summary>Expand Screenshots</summary>
-
The images below provide additional examples of how the `InvokeModel` event name is logged.
The first image shows the differences between various methods of model invocation:
(a) Model invocation using a Knowledge Base
(b) Direct model invocation
(c) Model invocation using an Agent

Note that the *User name* displayed is different depending on how the model is invoked

![Agent and Knowledge Base model invocation](/images/bedrock-06.png)

This difference is highlighted using two side-by-side screenshots of the `InvokeModel` event record in CloudTrail
The left image =  Direct model invocation
The right image = Model invocation using a Knowledge Base

![CloudTrail for InvokeModel](/images/bedrock-07.png)


</details>

---

**InvokeModel** or **InvokeModelWithResponseStream** - prompts and response
<details>
  <summary>Expand Screenshot</summary>
-
While the `InvokeModel` and `InvokeModelWithResponseStream` events show up in CloudTrail, model invocation logs (when configured) also display the prompt and response that corresponds to each `InvokeModel` event.  The image below is taken from model invocation logs that have been sent to S3 - these contain the prompt used as well as the output or the response from the model (an Athena DDL is available below):

![S3 Model invocation example](/images/bedrock-09.png)

</details>

---

**PutModelInvocationLoggingConfiguration**
<details>
  <summary>Expand Screenshot</summary>
-
This event is logged when model invocation logging is configured in an AWS account

![Configuring Model Invocation Logging](/images/bedrock-10.png)

</details>

---

**CreateKnowledgeBase**
<details>
  <summary>Expand Screenshot</summary>
-
The following CloudTrail event record is logged when a knowledge base is created.  The event record includes the identity that issued the request and the name of the Knowledge Base.  The name of the knowledge base can be referenced in subsequent logs that capture model invocations using `InvokeModel`

![Knowledge Base Creation](/images/bedrock-11.png)

</details>

---

**CreateAgent**
<details>
  <summary>Expand Screenshot</summary>
-
The image below portrays the creation of an agent using the console.  The resulting event name that is logged is `CreateAgent`

![Agent Creation](/images/bedrock-12.png)

</details>

---

**Retrieve** and **RetrieveAndGenerate**
<details>
  <summary>Expand Screenshot</summary>
-
An example of how the `Retrieve` and `RetrieveAndGenerate` event names are shown in the image below when testing a knowledge base.  Note that `Retrieve` only tests the retrieval of data from the knowledge base, and `RetrieveAndGenerate` tests the retrieval of data from the knowledge base and generation of a response:

![Retrieve and RetrieveAndGenerate](/images/bedrock-13.png)

</details>

---

### Athena DDL

Use the DDL code below to create a table in Athena from the Model Invocation Logs stored in an Amazon S3 bucket.  Ensure that the correct S3 URI is used for the bucket location at the end of the code snippet:

```
CREATE EXTERNAL TABLE bedrock_model_invocation_metadata_unfiltered (
  schemaType string,
  schemaVersion string,
  timestamp string,
  accountId string,
  identity struct <arn: string>,
  region string,
  requestId string,
  operation string,
  modelId string,
  errorCode string,
  input struct <inputBodyJson: string,
                  inputBodyS3Path: string,
                  inputContentType: string,
                  inputTokenCount: int>,
  output struct <outputBodyJson: string,
                  outputBodyS3Path: string,
                  outputContentType: string,
                  outputTokenCount: int,
                  outputImageBucketedStepSize: int,
                  outputImageHeight: int,
                  outputImageWidth: int
                  >
  )
ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
 LOCATION 's3://<insert_S3_bucket>/<insert_prefix>/'
```
---

### Bedrock API Keys
To detect the creation or usage of [Bedrock API Keys](https://aws.amazon.com/blogs/machine-learning/accelerate-ai-development-with-amazon-bedrock-api-keys/), leverage [Bedrock API Key detection using Eventbridge and SNS](../detections/Bedrock_Api_Key_Eventbridge_Detection.md).

## Addressed Backlog Items
- As an Incident Responder I need to be able to monitor all critical Bedrock events
- As an Incident Responder I need a playbook on quering Bedrock Cloudtrail events at scale

## Current Backlog Items