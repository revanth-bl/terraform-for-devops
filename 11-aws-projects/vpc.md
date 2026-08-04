# 🌐 Deploying an Amazon VPC with Terraform

## 📖 Introduction

An **Amazon Virtual Private Cloud (Amazon VPC)** is the foundation of almost every AWS infrastructure. It provides an isolated virtual network where you can launch AWS resources such as EC2 instances, RDS databases, EKS clusters, and Lambda functions.

Using Terraform, you can define the entire network as code, making deployments:

- Automated
- Repeatable
- Version controlled
- Easy to modify
- Easy to destroy

A well-designed VPC is the first step toward building secure and scalable cloud infrastructure.

---

# What is a VPC?

A VPC is a logically isolated virtual network inside AWS.

It allows you to control:

- IP address ranges
- Subnets
- Route tables
- Internet access
- NAT Gateways
- Security Groups
- Network ACLs

```
AWS Cloud

↓

Amazon VPC

↓

Your Resources
```

---

# Project Architecture

```text
                 Internet
                     │
                     ▼
             Internet Gateway
                     │
                     ▼
               Amazon VPC
         ┌──────────┴──────────┐
         ▼                     ▼
 Public Subnet          Private Subnet
         │                     │
         ▼                     ▼
      EC2 Instance          RDS / EKS
```

---

# Project Structure

```text
vpc-project/

├── provider.tf
├── versions.tf
├── variables.tf
├── terraform.tfvars
├── vpc.tf
├── subnet.tf
├── route-table.tf
├── internet-gateway.tf
├── security-group.tf
└── outputs.tf
```

---

# Prerequisites

Before starting, ensure you have:

- AWS Account
- Terraform installed
- AWS CLI installed
- AWS credentials configured
- Basic understanding of networking

Verify Terraform:

```bash
terraform version
```

Verify AWS CLI:

```bash
aws --version
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

# Create the VPC

Example:

```hcl
resource "aws_vpc" "main" {

  cidr_block = "10.0.0.0/16"

  tags = {

    Name = "Terraform-VPC"

  }

}
```

This creates the main virtual network.

---

# Create Public and Private Subnets

Example:

```hcl
resource "aws_subnet" "public" {

  vpc_id = aws_vpc.main.id

  cidr_block = "10.0.1.0/24"

}

resource "aws_subnet" "private" {

  vpc_id = aws_vpc.main.id

  cidr_block = "10.0.2.0/24"

}
```

Public subnets allow internet-facing resources.

Private subnets host internal resources like databases and application servers.

---

# Create an Internet Gateway

Example:

```hcl
resource "aws_internet_gateway" "igw" {

  vpc_id = aws_vpc.main.id

}
```

The Internet Gateway enables communication between the VPC and the internet.

---

# Create a Route Table

Example:

```hcl
resource "aws_route_table" "public" {

  vpc_id = aws_vpc.main.id

}
```

Add a default route:

```hcl
route {

  cidr_block = "0.0.0.0/0"

  gateway_id = aws_internet_gateway.igw.id

}
```

This routes internet-bound traffic through the Internet Gateway.

---

# Create a Security Group

Example:

```hcl
resource "aws_security_group" "web" {

  name = "web-sg"

  vpc_id = aws_vpc.main.id

}
```

Security Groups act as virtual firewalls for AWS resources.

---

# Outputs

Example:

```hcl
output "vpc_id" {

  value = aws_vpc.main.id

}
```

Useful outputs include:

- VPC ID
- Public subnet ID
- Private subnet ID
- Internet Gateway ID

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

# Preview Infrastructure

```bash
terraform plan
```

Example:

```text
Plan: 8 to add, 0 to change, 0 to destroy.
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

Terraform provisions the VPC and networking resources.

---

# Verify Deployment

Verify using AWS CLI:

```bash
aws ec2 describe-vpcs
```

Or inspect the VPC using the AWS Management Console.

---

# Destroy Infrastructure

```bash
terraform destroy
```

Confirm:

```text
yes
```

Terraform removes the VPC and associated resources.

---

# Deployment Workflow

```text
Write Terraform
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
Amazon VPC Created
```

---

# Amazon VPC Architecture

```text
Internet
    │
    ▼
Internet Gateway
    │
    ▼
Amazon VPC
 ┌────┴────┐
 ▼         ▼
Public   Private
Subnet    Subnet
 │          │
 ▼          ▼
EC2      RDS / EKS
```

---

# Best Practices

- Use separate public and private subnets.
- Use private subnets for databases and production workloads.
- Use meaningful CIDR blocks.
- Tag all AWS resources consistently.
- Follow the principle of least privilege for Security Groups.
- Store Terraform state remotely for team projects.
- Review `terraform plan` before deployment.

---

# Common Mistakes

❌ Using overlapping CIDR blocks.

❌ Placing databases in public subnets.

❌ Opening Security Groups to `0.0.0.0/0` unnecessarily.

❌ Forgetting to associate route tables with subnets.

❌ Not attaching an Internet Gateway for internet-facing resources.

❌ Leaving unused networking resources running.

---

# Interview Questions

### What is an Amazon VPC?

An Amazon VPC is a logically isolated virtual network in AWS where cloud resources are deployed.

---

### Which Terraform resource creates a VPC?

```hcl
resource "aws_vpc"
```

---

### Which Terraform resource creates an Internet Gateway?

```hcl
resource "aws_internet_gateway"
```

---

### Why are private subnets used?

Private subnets host internal resources that should not be directly accessible from the internet.

---

### What is the purpose of a route table?

A route table determines how network traffic is routed within the VPC and to external networks.

---

### Which command removes the VPC infrastructure?

```bash
terraform destroy
```

---

# Summary

Amazon VPC is the networking foundation of AWS, and Terraform makes building secure, scalable, and repeatable network architectures simple through Infrastructure as Code.

Key concepts include:

- Amazon VPC
- `aws_vpc`
- Public and Private Subnets
- Internet Gateway
- Route Tables
- Security Groups
- CIDR Blocks
- `terraform init`
- `terraform plan`
- `terraform apply`
- Infrastructure as Code

A well-designed VPC is the backbone of AWS deployments and serves as the foundation for services such as EC2, RDS, EKS, Lambda, and many other AWS resources.