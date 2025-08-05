
# Bedrock API Security Monitoring

A CloudFormation solution that monitors Amazon Bedrock API activity for security events, specifically tracking service-specific credential creation and bearer token usage through CloudTrail logs.

## Overview

This solution creates two EventBridge rules to monitor CloudTrail logs for specific Bedrock API security events. When events are detected, notifications are sent via SNS to subscribed email addresses.

## Features

- **CreateServiceSpecificCredential Monitoring**: Detects IAM API calls for credential creation
- **Bearer Token Usage Detection**: Monitors API calls using bearer tokens


## Architecture

The solution consists of:

- **EventBridge Rules**: Two rules that monitor CloudTrail for credential creation and bearer token usage
- **SNS Topic**: Delivers encrypted security alerts via email
- **KMS Key**: Encrypts SNS messages
- **IAM Roles**: Provide necessary access for service operations


## Deployment

[Download the template](./cfn/bedrock-eventbridge-monitoring.yaml)

Deploy using AWS CLI:

```bash
aws cloudformation create-stack \
  --stack-name bedrock--api-security-monitoring \
  --template-body file://bedrock-eventbridge-monitoring.yaml \
  --capabilities CAPABILITY_NAMED_IAM
```

Or deploy via AWS Console by uploading the template file.


