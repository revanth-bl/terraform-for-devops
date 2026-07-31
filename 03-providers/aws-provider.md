# ☁️ Terraform AWS Provider

## 📖 Introduction

The **AWS Provider** enables Terraform to provision and manage **Amazon Web Services (AWS)** infrastructure using Infrastructure as Code (IaC).

Instead of manually creating resources through the AWS Management Console, you define your infrastructure in Terraform configuration files and let Terraform create, update, and destroy resources automatically.

The AWS Provider is officially maintained by **HashiCorp** and communicates with AWS services through the AWS APIs.

Provider Name:

```text
aws
```

---

# Why Use the AWS Provider?

Without Terraform:

```
Login to AWS Console

↓

Create VPC

↓

Create Subnet

↓

Launch EC2

↓

Configure Security Groups

↓

Repeat for every environment
```

With Terraform:

```bash
terraform apply
```

Terraform creates the complete infrastructure automatically.

---

# Prerequisites

Before using the AWS Provider, you should have:

- AWS Account
- IAM User or IAM Role
- AWS CLI Installed
- Terraform Installed
- AWS Access Key and Secret Key (if using IAM user)
- Required IAM permissions

Verify the AWS CLI:

```bash
aws --version
```

Verify Terraform:

```bash
terraform version
```

---

# Configure AWS Credentials

### Method 1: AWS CLI (Recommended)

Configure your credentials:

```bash
aws configure
```

Terraform automatically uses the credentials stored by the AWS CLI.

Example:

```
AWS Access Key ID:

AWS Secret Access Key:

Default region:

Default output format:
```

Verify the identity:

```bash
aws sts get-caller-identity
```

---

### Method 2: Environment Variables

Linux/macOS:

```bash
export AWS_ACCESS_KEY_ID="YOUR_ACCESS_KEY"

export AWS_SECRET_ACCESS_KEY="YOUR_SECRET_KEY"

export AWS_DEFAULT_REGION="us-east-1"
```

Windows PowerShell:

```powershell
$env:AWS_ACCESS_KEY_ID="YOUR_ACCESS_KEY"

$env:AWS_SECRET_ACCESS_KEY="YOUR_SECRET_KEY"

$env:AWS_DEFAULT_REGION="us-east-1"
```

---

### Method 3: IAM Role

When running Terraform on an AWS EC2 instance, ECS task, or Lambda function, attach an IAM Role to the compute resource. Terraform automatically uses the temporary credentials provided by AWS.

This is the recommended authentication method for production workloads on AWS.

---

# Configure the AWS Provider

```hcl
terraform {

  required_providers {

    aws = {

      source  = "hashicorp/aws"

      version = "~> 5.0"

    }

  }

}

provider "aws" {

  region = "us-east-1"

}
```

---

# Provider Block Explained

```hcl
provider "aws" {

  region = "us-east-1"

}
```

- `provider` specifies the cloud provider.
- `aws` is the provider name.
- `region` sets the default AWS Region for resources.

---

# Common AWS Regions

Some commonly used AWS Regions include:

- us-east-1 (N. Virginia)
- us-east-2 (Ohio)
- us-west-1 (N. California)
- us-west-2 (Oregon)
- eu-west-1 (Ireland)
- eu-central-1 (Frankfurt)
- ap-south-1 (Mumbai)
- ap-southeast-1 (Singapore)
- ap-northeast-1 (Tokyo)

---

# Create an EC2 Instance

```hcl
resource "aws_instance" "web" {

  ami           = "ami-1234567890abcdef0"

  instance_type = "t2.micro"

}
```

Terraform creates one EC2 instance.

---

# Create an S3 Bucket

```hcl
resource "aws_s3_bucket" "logs" {

  bucket = "my-terraform-demo-bucket"

}
```

> **Note:** S3 bucket names must be globally unique across AWS.

---

# Create a VPC

```hcl
resource "aws_vpc" "main" {

  cidr_block = "10.0.0.0/16"

  tags = {

    Name = "terraform-vpc"

  }

}
```

---

# Create a Security Group

```hcl
resource "aws_security_group" "web" {

  name = "web-sg"

  description = "Allow SSH"

  vpc_id = aws_vpc.main.id

}
```

Ingress and egress rules can be added to control network access.

---

# Common AWS Resources

Terraform can manage:

- EC2
- VPC
- Subnets
- Route Tables
- Internet Gateways
- NAT Gateways
- Security Groups
- IAM Users, Roles, and Policies
- S3 Buckets
- EBS Volumes
- Elastic Load Balancers
- Auto Scaling Groups
- RDS
- Lambda
- CloudWatch
- SNS
- SQS
- EKS
- ECS
- DynamoDB
- Route 53

---

# AWS Provider Workflow

```text
Write Terraform Code
          │
          ▼
terraform init
          │
          ▼
Download AWS Provider
          │
          ▼
terraform plan
          │
          ▼
Review Changes
          │
          ▼
terraform apply
          │
          ▼
AWS Resources Created
```

---

# Authentication Methods

Terraform supports multiple authentication methods for AWS.

### AWS CLI

```bash
aws configure
```

Best for learning and development.

---

### Environment Variables

```bash
AWS_ACCESS_KEY_ID

AWS_SECRET_ACCESS_KEY

AWS_DEFAULT_REGION
```

Common in local development and automation.

---

### IAM Role

Recommended for:

- EC2
- ECS
- Lambda
- CodeBuild

Avoids storing long-term credentials.

---

### AWS IAM Identity Center (AWS SSO)

Supported through the AWS CLI for organizations using centralized authentication.

---

# Best Practices

- Pin the AWS provider version.
- Use IAM Roles instead of long-term access keys whenever possible.
- Store Terraform state remotely (for example, in Amazon S3 with state locking).
- Follow the principle of least privilege.
- Use modules for reusable infrastructure.
- Never hardcode AWS credentials in Terraform files.

---

# Common Mistakes

❌ Forgetting to run `aws configure`.

❌ Hardcoding AWS credentials.

❌ Using overly permissive IAM policies.

❌ Not specifying the provider version.

❌ Creating S3 buckets with non-unique names.

❌ Deploying resources to the wrong AWS Region.

---

# Interview Questions

### What is the AWS Provider?

The AWS Provider enables Terraform to provision and manage Amazon Web Services resources.

---

### What is the provider name?

```text
aws
```

---

### Which command configures AWS CLI credentials?

```bash
aws configure
```

---

### How do you verify your AWS identity?

```bash
aws sts get-caller-identity
```

---

### What is the recommended authentication method for production on AWS?

**IAM Roles**, because they provide temporary credentials and eliminate the need to store access keys.

---

### Can Terraform manage Amazon EKS?

Yes. Terraform can provision and manage Amazon Elastic Kubernetes Service (EKS), along with its supporting networking and IAM resources.

---

# Summary

The AWS Provider enables Terraform to automate the provisioning and management of AWS infrastructure.

Key concepts include:

- AWS authentication
- Provider configuration
- EC2
- VPC
- Security Groups
- S3
- IAM
- AWS Regions
- Best practices for authentication and security

Mastering the AWS Provider is an essential skill for building scalable, secure, and production-ready AWS infrastructure using Terraform.