# 🗄️ Deploying Amazon RDS with Terraform

## 📖 Introduction

**Amazon Relational Database Service (Amazon RDS)** is AWS's managed relational database service that simplifies the deployment, operation, and scaling of databases in the cloud.

With Terraform, you can automate the creation and management of RDS instances, subnet groups, parameter groups, security groups, and backups using Infrastructure as Code (IaC).

Using Terraform for RDS provides:

- Automated database provisioning
- Version-controlled infrastructure
- Repeatable deployments
- Consistent environments
- Easy infrastructure management

---

# What is Amazon RDS?

Amazon RDS is a managed database service supporting multiple database engines, including:

- MySQL
- PostgreSQL
- MariaDB
- Oracle
- Microsoft SQL Server

AWS manages:

- Hardware
- Operating system
- Automatic backups
- Software patching
- High availability (optional)
- Monitoring

```
Application

↓

Amazon RDS

↓

Managed Database
```

---

# Project Architecture

```text
Terraform
      │
      ▼
AWS Provider
      │
      ▼
VPC
      │
      ▼
DB Subnet Group
      │
      ▼
Security Group
      │
      ▼
Amazon RDS
```

---

# Project Structure

```text
rds-project/

├── provider.tf
├── versions.tf
├── variables.tf
├── terraform.tfvars
├── network.tf
├── security.tf
├── rds.tf
├── outputs.tf
└── README.md
```

---

# Prerequisites

Before deploying an RDS instance, ensure you have:

- AWS Account
- Terraform installed
- AWS CLI installed
- AWS credentials configured
- A VPC with at least two subnets in different Availability Zones
- Basic understanding of relational databases

Verify installations:

```bash
terraform version
```

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

# Create a DB Subnet Group

Amazon RDS must be deployed inside a DB subnet group.

Example:

```hcl
resource "aws_db_subnet_group" "main" {

  name       = "terraform-db-subnet"

  subnet_ids = var.private_subnets

}
```

The subnet group allows RDS to use multiple Availability Zones.

---

# Create a Security Group

The security group controls database access.

Example:

```hcl
resource "aws_security_group" "rds" {

  name = "terraform-rds-sg"

}
```

Typically, inbound rules allow only trusted application servers or specific IP addresses to connect to the database.

---

# Create the RDS Instance

Example:

```hcl
resource "aws_db_instance" "mysql" {

  identifier        = "terraform-db"

  engine            = "mysql"

  instance_class    = "db.t3.micro"

  allocated_storage = 20

  username          = "admin"

  password          = var.db_password

  skip_final_snapshot = true

}
```

Terraform provisions the managed database instance.

> **Note:** Store database passwords securely using variables, secrets management, or a service such as AWS Secrets Manager. Avoid hardcoding credentials in Terraform files.

---

# Outputs

Example:

```hcl
output "db_endpoint" {

  value = aws_db_instance.mysql.endpoint

}
```

Useful outputs include:

- Database endpoint
- Database identifier
- Database ARN

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

Example output:

```text
Plan: 5 to add, 0 to change, 0 to destroy.
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

Provisioning an RDS instance may take several minutes.

---

# Connect to the Database

Example (MySQL):

```bash
mysql -h DATABASE_ENDPOINT \
-u admin \
-p
```

Replace `DATABASE_ENDPOINT` with the endpoint displayed in the Terraform outputs.

---

# Monitor the Database

Use:

- Amazon CloudWatch Metrics
- Amazon CloudWatch Logs (where supported)
- AWS Management Console
- Enhanced Monitoring (optional)

These services help monitor database performance and health.

---

# Destroy Infrastructure

```bash
terraform destroy
```

Confirm:

```text
yes
```

Terraform removes the RDS instance and associated infrastructure.

---

# Deployment Workflow

```text
Terraform
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
Amazon RDS
```

---

# Amazon RDS Architecture

```text
Application
      │
      ▼
Security Group
      │
      ▼
Amazon RDS
      │
      ▼
Database Storage
```

---

# Best Practices

- Use private subnets for production databases.
- Restrict database access using security groups.
- Store passwords securely using secrets management.
- Enable automated backups.
- Enable Multi-AZ deployments for production workloads.
- Use remote Terraform state for team projects.
- Review `terraform plan` before deployment.
- Tag database resources consistently.

---

# Common Mistakes

❌ Placing production databases in public subnets.

❌ Hardcoding database passwords.

❌ Opening database ports to the entire internet (`0.0.0.0/0`).

❌ Disabling backups without a valid reason.

❌ Ignoring security group rules.

❌ Leaving unused RDS instances running and incurring unnecessary AWS costs.

---

# Interview Questions

### What is Amazon RDS?

Amazon RDS is AWS's managed relational database service.

---

### Which Terraform resource creates an RDS instance?

```hcl
resource "aws_db_instance"
```

---

### Which Terraform resource creates a DB subnet group?

```hcl
resource "aws_db_subnet_group"
```

---

### Why is a DB subnet group required?

It specifies the subnets where Amazon RDS can deploy database instances, typically across multiple Availability Zones.

---

### Should database passwords be hardcoded in Terraform?

No. Passwords should be stored securely using variables marked as sensitive, secrets management solutions, or AWS Secrets Manager.

---

### Which command removes the RDS infrastructure?

```bash
terraform destroy
```

---

# Summary

Amazon RDS is a fully managed relational database service, and Terraform enables automated, repeatable, and version-controlled database deployments.

Key concepts include:

- Amazon RDS
- `aws_db_instance`
- `aws_db_subnet_group`
- Security Groups
- Database endpoints
- Automated backups
- Multi-AZ deployments
- `terraform init`
- `terraform plan`
- `terraform apply`
- Infrastructure as Code

Learning to deploy Amazon RDS with Terraform is an important step toward building production-ready cloud architectures that include databases, applications, and networking resources.