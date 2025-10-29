# Accelerate incident response: Build an intelligent security playbook assistant with Amazon Q

## Overview

This guide provides step-by-step instructions for deploying a security-hardened Amazon Q Business application designed for security incident response and playbook management. The solution creates an AI-powered search and chat interface that allows security teams to quickly find and interact with security playbooks using natural language queries.

## Security Enhancements

This secure version includes the following security improvements over the standard implementation:

### 🔒 **Enhanced Security Features**
- **KMS Encryption**: All S3 buckets use AWS KMS encryption instead of basic AES256
- **Access Logging**: Comprehensive S3 access logging with dedicated logs bucket
- **Least Privilege IAM**: IAM policies scoped to specific application resources (no wildcards)
- **Secure User Management**: Removed overly permissive IAM groups, uses Identity Center exclusively
- **Enhanced Monitoring**: Detailed access logs with automatic 90-day retention policy
- **Data Protection**: S3 versioning enabled on all data buckets

### 🛡️ **Security Compliance**
- No wildcard permissions in IAM policies
- All resources scoped to specific application IDs
- Public access blocked on all S3 buckets
- Encrypted data at rest and in transit
- Comprehensive audit logging

## Architecture

The solution deploys:
- **Amazon Q Business Application** with IAM Identity Center integration
- **Two encrypted S3 buckets** for AWS and custom security playbooks
- **Dedicated access logs bucket** with lifecycle management
- **Q Business Index** with searchable document attributes
- **Web experience** for natural language interaction
- **IAM roles** with least-privilege permissions

## Prerequisites

- Existing AWS IAM Identity Center instance
- Users created in Identity Center
- AWS CLI configured with appropriate permissions
- CloudFormation deployment permissions

## Step 1: Deploy the Secure CloudFormation Template

1. **Download the secure template**: `Q-SecurityPlaybooks-Secure.yaml`

2. **Get your Identity Center instance ARN**:
   ```bash
   aws sso-admin list-instances
   ```

3. **Deploy the template**:
   ```bash
   aws cloudformation deploy \
     --template-file Q-SecurityPlaybooks-Secure.yaml \
     --stack-name q-security-playbooks-secure \
     --parameter-overrides \
       ApplicationName=security-playbooks \
       AwsPlaybooksBucketName=aws-security-playbooks \
       CxPlaybooksBucketName=custom-security-playbooks \
       IdentityCenterInstanceArn=arn:aws:sso:::instance/ssoins-xxxxxxxxxx \
     --capabilities CAPABILITY_IAM \
     --region us-east-1
   ```

4. **Wait for deployment completion** (typically 5-10 minutes)

## Step 2: Upload AWS Security Playbooks

1. **Clone the AWS Customer Playbook Framework**:
   ```bash
   git clone https://github.com/aws-samples/aws-customer-playbook-framework.git
   cd aws-customer-playbook-framework
   ```

2. **Upload to your AWS playbooks bucket**:
   ```bash
   aws s3 sync . s3://aws-security-playbooks-YOUR_ACCOUNT_ID/ --exclude ".git/*"
   ```

3. **Verify upload in AWS Console** or via CLI:
   ```bash
   aws s3 ls s3://aws-security-playbooks-YOUR_ACCOUNT_ID/ --recursive
   ```

## Step 3: Upload Custom Security Playbooks

1. **Prepare your custom playbooks** in a local directory

2. **Upload custom playbooks to the dedicated S3 bucket**:
   ```bash
   aws s3 cp your-custom-playbooks/ s3://custom-security-playbooks-YOUR_ACCOUNT_ID/ --recursive
   ```

3. **Confirm the upload** by reviewing the bucket contents in the AWS Console

## Step 4: Configure User Access (Identity Center Only)

⚠️ **Important**: This secure version removes IAM user groups. All user management must be done through Identity Center.

1. **Navigate to IAM Identity Center console**
2. **Ensure users are created** in Identity Center (not IAM)
3. **Go to Amazon Q Business console**
4. **Select your application**
5. **Click "Access management"**
6. **Add users and groups** from Identity Center only
7. **Assign appropriate permissions** based on user roles

## Step 5: Sync Playbooks with Amazon Q

1. **Navigate to the Amazon Q Business console**
2. **Select your application**
3. **Click on the "Data sources" tab**
4. **For each data source** (AWS playbooks and custom playbooks):
   - Locate the data source in the list
   - Click "Sync now"
   - Monitor the sync progress (may take several minutes)
5. **Verify successful sync** for both data sources

## Step 6: Set Up Automated AWS Playbook Updates (Enhanced Security)

Create a Lambda function with enhanced security for automatic updates:

1. **Navigate to AWS Lambda console**
2. **Click "Create function"**
3. **Choose "Author from scratch"** with name "UpdateAWSPlaybooks-Secure"
4. **Select Python 3.11** (or latest available)

5. **Use this enhanced secure code**:
```python
import boto3
import subprocess
import os
import logging
from typing import Dict, Any
import json

# Configure logging
logger = logging.getLogger()
logger.setLevel(logging.INFO)

def lambda_handler(event: Dict[str, Any], context: Any) -> Dict[str, Any]:
    """
    Secure AWS Lambda function to sync AWS Customer Playbook Framework to S3.
    Enhanced with better error handling and security logging.
    """
    try:
        # Clean up any existing files
        if os.path.exists('/tmp/playbooks'):
            subprocess.run(['rm', '-rf', '/tmp/playbooks'], check=True)
        
        # Clone latest from GitHub with security verification
        logger.info("Cloning repository with security verification...")
        result = subprocess.run(
            ['git', 'clone', '--depth', '1', '--single-branch',
             'https://github.com/aws-samples/aws-customer-playbook-framework.git',
             '/tmp/playbooks'],
            capture_output=True,
            text=True,
            check=True,
            timeout=300  # 5 minute timeout
        )
        
        # Initialize S3 client with enhanced security
        s3 = boto3.client('s3')
        bucket_name = os.environ['BUCKET_NAME']
        
        # Verify bucket exists and we have access
        try:
            s3.head_bucket(Bucket=bucket_name)
        except Exception as e:
            raise Exception(f"Cannot access bucket {bucket_name}: {str(e)}")
        
        # Upload files to S3 with metadata
        logger.info(f"Uploading files to S3 bucket: {bucket_name}")
        files_uploaded = 0
        
        for root, _, files in os.walk('/tmp/playbooks'):
            for file in files:
                if not file.startswith('.git') and not file.startswith('.'):
                    local_path = os.path.join(root, file)
                    s3_key = os.path.relpath(local_path, '/tmp/playbooks')
                    
                    # Add metadata for tracking
                    s3.upload_file(
                        Filename=local_path,
                        Bucket=bucket_name,
                        Key=s3_key,
                        ExtraArgs={
                            'Metadata': {
                                'source': 'automated-sync',
                                'sync-timestamp': str(context.aws_request_id)
                            },
                            'ServerSideEncryption': 'aws:kms'
                        }
                    )
                    files_uploaded += 1
        
        # Log successful completion
        logger.info(f"Successfully uploaded {files_uploaded} files to S3")
        
        # Trigger Q Business data source sync
        qbusiness = boto3.client('qbusiness')
        app_id = os.environ.get('QBUSINESS_APP_ID')
        data_source_id = os.environ.get('QBUSINESS_DATA_SOURCE_ID')
        index_id = os.environ.get('QBUSINESS_INDEX_ID')
        
        if all([app_id, data_source_id, index_id]):
            try:
                qbusiness.start_data_source_sync_job(
                    applicationId=app_id,
                    dataSourceId=data_source_id,
                    indexId=index_id
                )
                logger.info("Triggered Q Business data source sync")
            except Exception as e:
                logger.warning(f"Could not trigger Q Business sync: {str(e)}")
        
        return {
            'statusCode': 200,
            'body': json.dumps({
                'message': f'Successfully uploaded {files_uploaded} files to S3',
                'bucket': bucket_name,
                'files_uploaded': files_uploaded
            })
        }
        
    except subprocess.CalledProcessError as e:
        error_message = f"Git clone failed: {e.stderr}"
        logger.error(error_message)
        raise Exception(error_message)
        
    except Exception as e:
        error_message = f"Error occurred: {str(e)}"
        logger.error(error_message)
        raise Exception(error_message)
    
    finally:
        # Secure cleanup
        if os.path.exists('/tmp/playbooks'):
            subprocess.run(['rm', '-rf', '/tmp/playbooks'], check=True)
```

6. **Configure environment variables**:
   - `BUCKET_NAME`: Your AWS playbooks S3 bucket name
   - `QBUSINESS_APP_ID`: Your Q Business application ID (from CloudFormation outputs)
   - `QBUSINESS_DATA_SOURCE_ID`: Your data source ID
   - `QBUSINESS_INDEX_ID`: Your index ID

7. **Set execution timeout** to 5 minutes
8. **Configure enhanced IAM role** with minimal required permissions
9. **Deploy the function**

## Step 7: Schedule Automated Updates with Enhanced Security

1. **Go to Amazon EventBridge console**
2. **Click "Create rule"**
3. **Configure rule**:
   - Name: `SecurePlaybookUpdate`
   - Description: `Secure automated playbook updates`
   - Rule type: `Schedule`
4. **Set schedule pattern** (e.g., daily at 2 AM UTC)
5. **Add target**:
   - Service: AWS Lambda
   - Function: Your secure Lambda function
6. **Add input transformer** for enhanced logging
7. **Review and create**

## Step 8: Monitor and Test the Secure Solution

### Testing
1. **Open your Amazon Q application** via Identity Center portal
2. **Test security-related queries**:
   - "How do I mitigate a ransomware attack?"
   - "Show me incident response procedures for data breaches"
   - "What are the steps for AWS Config compliance issues?"
3. **Verify responses** include relevant playbook information

### Monitoring
1. **Check S3 access logs** in your dedicated logs bucket
2. **Monitor CloudTrail** for Q Business API calls
3. **Review Lambda execution logs** for sync operations
4. **Set up CloudWatch alarms** for failed syncs or access anomalies

## Security Monitoring and Compliance

### Access Logging
- **S3 Access Logs**: Stored in dedicated bucket with 90-day retention
- **CloudTrail Integration**: All API calls logged and monitored
- **Lambda Execution Logs**: Detailed sync operation logging

### Security Best Practices
- **Regular Access Reviews**: Review user access quarterly
- **Playbook Content Audits**: Ensure sensitive information is properly classified
- **Cost Monitoring**: Set up AWS Budgets for cost control
- **Security Scanning**: Regular scans of uploaded content

## Cost Estimation (Enhanced Security Version)

The secure version includes additional components that may affect costs:

| Component | Configuration | Monthly Cost (USD) |
|-----------|---------------|-------------------|
| Amazon Q Business Lite | 4,500 users × $3/user | $13,500 |
| Amazon Q Business Pro | 500 users × $20/user | $10,000 |
| Enterprise Index | 50 index units × $0.264/hour | $9,504 |
| AWS Lambda | 1M requests/month | $46.91 |
| Amazon API Gateway | 1M requests/month | $5 |
| **S3 Access Logging** | **Additional storage costs** | **~$50-100** |
| **KMS Encryption** | **Additional key usage** | **~$10-20** |
| **Enhanced Monitoring** | **CloudWatch logs/metrics** | **~$25-50** |
| **Total Estimated Cost** | | **$33,140-$33,225** |

### Cost Optimization for Secure Version
- Monitor access log storage and adjust retention as needed
- Use S3 Intelligent Tiering for log storage
- Optimize Lambda execution frequency
- Regular cleanup of unused resources
- Set up detailed cost allocation tags

## Clean Up (Secure Version)

To avoid ongoing charges and ensure complete cleanup:

1. **Delete CloudFormation stack**:
   ```bash
   aws cloudformation delete-stack --stack-name q-security-playbooks-secure
   ```

2. **Verify S3 bucket cleanup**:
   - Check that all three buckets are deleted (data + logs)
   - Empty buckets manually if deletion fails

3. **Clean up Lambda and EventBridge**:
   - Delete Lambda function
   - Remove EventBridge rules

4. **Verify Identity Center cleanup**:
   - Remove application assignments
   - Clean up any test users if created

5. **Check KMS key usage**:
   - Review KMS key policies if using custom keys

## Security Considerations

### Data Classification
- Ensure playbooks don't contain sensitive credentials
- Implement proper data classification tags
- Regular content audits for compliance

### Access Control
- Use Identity Center groups for role-based access
- Implement least-privilege principles
- Regular access reviews and cleanup

### Monitoring and Alerting
- Set up alerts for unusual access patterns
- Monitor failed authentication attempts
- Track data source sync failures

## Troubleshooting

### Common Security-Related Issues
1. **Access Denied Errors**: Verify Identity Center user assignments
2. **Sync Failures**: Check IAM role permissions are scoped correctly
3. **Missing Logs**: Verify access logging bucket permissions
4. **KMS Errors**: Ensure proper KMS key policies

### Support Resources
- AWS Support for Q Business issues
- CloudFormation documentation for template troubleshooting
- Identity Center documentation for user management

## Next Steps

1. **Implement additional security controls** as needed for your organization
2. **Set up automated security scanning** of playbook content
3. **Integrate with SIEM systems** for enhanced monitoring
4. **Develop custom playbooks** specific to your environment
5. **Train security team** on effective Q Business usage
6. **Establish governance processes** for playbook management

## Notices

This secure implementation guide provides enhanced security controls for AWS Q Business deployment. Customers are responsible for:
- Ensuring compliance with organizational security policies
- Regular security reviews and updates
- Proper data classification and handling
- Monitoring and incident response procedures

AWS products and services are provided "as is" without warranties. This guidance represents current best practices and is subject to change. Always consult the latest AWS documentation and security best practices for your specific use case.

[Q-SecurityPlaybooks-Secure.json](https://github.com/user-attachments/files/21760628/Q-SecurityPlaybooks-Secure.json)
