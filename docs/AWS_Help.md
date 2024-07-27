# Security Playbook for Engaging AWS Support
This document is provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2024 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

## Points of Contact

Author: `Author Name` \
Approver: `Approver Name` \
Last Date Approved:  

## Executive Summary

As part of our ongoing commitment to customers, AWS is providing this
security incident response playbook that describes the steps needed to request assistance from AWS Support for security events.

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
    -   Basic and Developer Support Customers: *General Question*.
    -   For more information you can [compare AWS Support plans](https://aws.amazon.com/premiumsupport/plans/).
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
