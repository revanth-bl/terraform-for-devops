# ⚡ Deploying AWS Lambda with Terraform

## 📖 Introduction

**AWS Lambda** is a serverless compute service that lets you run code without provisioning or managing servers.

With Terraform, you can automate the creation and management of Lambda functions, IAM roles, permissions, triggers, CloudWatch logs, and related AWS resources.

Using Infrastructure as Code (IaC) makes Lambda deployments:

- Automated
- Repeatable
- Version controlled
- Easy to update
- Easy to destroy when no longer needed

---

# What is AWS Lambda?

AWS Lambda executes your code in response to events such as:

- API requests
- File uploads to Amazon S3
- Amazon EventBridge schedules
- Amazon SNS messages
- Amazon SQS messages
- DynamoDB streams
- CloudWatch Events

```
Event

↓

AWS Lambda

↓

Execute Code

↓

Return Response
```

You only pay for the compute time your function uses.

---

# Project Architecture

```text
Terraform
      │
      ▼
AWS Provider
      │
      ▼
IAM Role
      │
      ▼
AWS Lambda
      │
      ▼
CloudWatch Logs
```

---

# Project Structure

```text
lambda-project/

├── provider.tf
├── versions.tf
├── variables.tf
├── terraform.tfvars
├── iam.tf
├── lambda.tf
├── outputs.tf
├── lambda_function.py
└── requirements.txt
```

---

# Prerequisites

Before starting, ensure you have:

- AWS Account
- Terraform installed
- AWS CLI installed
- AWS credentials configured
- Python (or another supported Lambda runtime)
- Basic knowledge of serverless computing

Verify installations:

```bash
terraform version
```

```bash
aws --version
```

```bash
python --version
```

---

# Configure AWS Credentials

```bash
aws configure
```

Provide:

```text
AWS Access Key ID

AWS Secret Access Key

Region

Output Format
```

---

# Configure the AWS Provider

**provider.tf**

```hcl
provider "aws" {

  region = var.aws_region

}
```

---

# Specify Terraform Version

**versions.tf**

```hcl
terraform {

  required_version = ">= 1.5.0"

  required_providers {

    aws = {

      source  = "hashicorp/aws"

      version = "~> 5.0"

    }

  }

}
```

---

# Create an IAM Role

Every Lambda function requires an execution role.

Example:

```hcl
resource "aws_iam_role" "lambda_role" {

  name = "terraform-lambda-role"

}
```

Attach the required IAM policies to allow the function to access AWS services such as CloudWatch Logs, S3, DynamoDB, or SNS.

---

# Package the Lambda Code

Example Python function:

**lambda_function.py**

```python
def lambda_handler(event, context):

    return {

        "statusCode": 200,

        "body": "Hello from Terraform!"

    }
```

Create a ZIP package:

```bash
zip function.zip lambda_function.py
```

Terraform uploads this package to AWS Lambda.

---

# Create the Lambda Function

Example:

```hcl
resource "aws_lambda_function" "hello" {

  function_name = "terraform-lambda"

  filename      = "function.zip"

  handler        = "lambda_function.lambda_handler"

  runtime        = "python3.12"

  role           = aws_iam_role.lambda_role.arn

}
```

Terraform creates the Lambda function using the uploaded deployment package.

---

# Outputs

Example:

```hcl
output "lambda_name" {

  value = aws_lambda_function.hello.function_name

}
```

Useful outputs include:

- Function name
- Function ARN
- Invoke ARN

---

# Initialize Terraform

```bash
terraform init
```

---

# Format Configuration

```bash
terraform fmt
```

---

# Validate Configuration

```bash
terraform validate
```

---

# Preview Changes

```bash
terraform plan
```

Example output:

```text
Plan: 3 to add, 0 to change, 0 to destroy.
```

---

# Deploy Infrastructure

```bash
terraform apply
```

Confirm:

```text
yes
```

Terraform creates the Lambda function and related resources.

---

# Invoke the Function

Using the AWS CLI:

```bash
aws lambda invoke \
  --function-name terraform-lambda \
  output.json
```

View the response:

```bash
cat output.json
```

Example:

```json
{
  "statusCode": 200,
  "body": "Hello from Terraform!"
}
```

---

# Monitor Logs

Lambda automatically writes logs to Amazon CloudWatch Logs.

View logs using:

```bash
aws logs describe-log-groups
```

Or use the AWS Management Console to inspect execution logs.

---

# Add Event Triggers

Lambda can be triggered by many AWS services, including:

- Amazon S3
- Amazon API Gateway
- Amazon EventBridge
- Amazon SNS
- Amazon SQS
- DynamoDB Streams
- CloudWatch Events

Terraform can provision these trigger resources and permissions alongside the Lambda function.

---

# Destroy Infrastructure

```bash
terraform destroy
```

Confirm:

```text
yes
```

Terraform removes the Lambda function and associated AWS resources.

---

# Deployment Workflow

```text
Write Lambda Code
        │
        ▼
Package Function
        │
        ▼
terraform init
        │
        ▼
terraform fmt
        │
        ▼
terraform validate
        │
        ▼
terraform plan
        │
        ▼
terraform apply
        │
        ▼
AWS Lambda
```

---

# Lambda Architecture

```text
Event
      │
      ▼
AWS Lambda
      │
      ▼
IAM Role
      │
      ▼
CloudWatch Logs
```

---

# Best Practices

- Follow the principle of least privilege for IAM roles.
- Keep deployment packages small.
- Store Terraform state remotely for team projects.
- Use environment variables instead of hardcoding configuration values.
- Enable CloudWatch logging and monitoring.
- Version and test Lambda functions before production deployments.
- Review `terraform plan` before applying changes.

---

# Common Mistakes

❌ Forgetting to package the Lambda code before deployment.

❌ Using incorrect handler names.

❌ Granting excessive IAM permissions.

❌ Hardcoding secrets inside the function code.

❌ Ignoring CloudWatch Logs when troubleshooting.

❌ Leaving unused Lambda functions and related resources deployed.

---

# Interview Questions

### What is AWS Lambda?

AWS Lambda is a serverless compute service that runs code in response to events without requiring server management.

---

### Which Terraform resource creates a Lambda function?

```hcl
resource "aws_lambda_function"
```

---

### Why does a Lambda function need an IAM role?

The IAM role grants the Lambda function permission to access AWS services such as CloudWatch Logs, S3, DynamoDB, and SNS.

---

### Which command packages a Python Lambda function?

```bash
zip function.zip lambda_function.py
```

---

### Which command deploys the Lambda function?

```bash
terraform apply
```

---

### Where are Lambda execution logs stored?

Amazon CloudWatch Logs.

---

# Summary

AWS Lambda is one of the core serverless services on AWS, and Terraform makes deploying and managing Lambda functions simple, repeatable, and version-controlled.

Key concepts include:

- AWS Lambda
- Serverless computing
- `aws_lambda_function`
- IAM Role
- CloudWatch Logs
- Event triggers
- `terraform init`
- `terraform plan`
- `terraform apply`
- Infrastructure as Code

Learning to deploy AWS Lambda with Terraform is an essential skill for modern DevOps and Cloud Engineers building scalable, event-driven applications.