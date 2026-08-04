# 🖥️ Deploying an Amazon EC2 Instance with Terraform

## 📖 Introduction

One of the most common Terraform projects is provisioning an **Amazon EC2 (Elastic Compute Cloud)** instance.

Instead of creating an EC2 instance manually through the AWS Management Console, Terraform allows you to define the infrastructure in code, making deployments:

- Repeatable
- Automated
- Version controlled
- Easy to modify
- Easy to destroy when no longer needed

This project demonstrates how to launch an EC2 instance using Terraform.

---

# Project Architecture

```text
Terraform
      │
      ▼
AWS Provider
      │
      ▼
Amazon EC2
      │
      ▼
Running Virtual Machine
```

---

# Project Structure

```text
ec2-project/

├── main.tf
├── provider.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
└── versions.tf
```

---

# Prerequisites

Before starting, ensure you have:

- AWS Account
- Terraform installed
- AWS CLI installed
- AWS credentials configured
- An existing EC2 Key Pair (for SSH access)
- Basic knowledge of Terraform

Verify Terraform:

```bash
terraform version
```

Verify AWS CLI:

```bash
aws --version
```

Check configured credentials:

```bash
aws sts get-caller-identity
```

---

# Configure AWS Credentials

Configure AWS CLI:

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

Terraform automatically uses these credentials.

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

# Define Variables

**variables.tf**

```hcl
variable "aws_region" {}

variable "ami_id" {}

variable "instance_type" {}

variable "key_name" {}
```

---

# Configure Variable Values

**terraform.tfvars**

```hcl
aws_region   = "us-east-1"

ami_id        = "ami-xxxxxxxx"

instance_type = "t2.micro"

key_name      = "my-keypair"
```

Replace the AMI ID and key pair with values from your AWS account and region.

---

# Create the EC2 Instance

**main.tf**

```hcl
resource "aws_instance" "web" {

  ami           = var.ami_id

  instance_type = var.instance_type

  key_name      = var.key_name

  tags = {

    Name = "Terraform-EC2"

  }

}
```

Terraform will create one EC2 instance with the specified configuration.

---

# Outputs

**outputs.tf**

```hcl
output "instance_id" {

  value = aws_instance.web.id

}

output "public_ip" {

  value = aws_instance.web.public_ip

}
```

After deployment, Terraform displays:

- EC2 Instance ID
- Public IP Address

---

# Initialize Terraform

```bash
terraform init
```

Downloads the AWS provider and initializes the project.

---

# Format the Code

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

Terraform shows what resources will be created.

Example:

```text
Plan: 1 to add, 0 to change, 0 to destroy.
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

Terraform creates the EC2 instance.

---

# Verify Deployment

List EC2 instances:

```bash
aws ec2 describe-instances
```

Or verify through the AWS Management Console.

---

# Connect to the Instance

Linux:

```bash
ssh -i my-keypair.pem ec2-user@PUBLIC_IP
```

Ubuntu:

```bash
ssh -i my-keypair.pem ubuntu@PUBLIC_IP
```

Replace `PUBLIC_IP` with the value shown in the Terraform outputs.

---

# Destroy Infrastructure

```bash
terraform destroy
```

Confirm:

```text
yes
```

Terraform removes the EC2 instance and updates the state.

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
Amazon EC2 Running
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
Amazon EC2
      │
      ▼
Public IP
```

---

# Best Practices

- Use variables instead of hardcoded values.
- Store Terraform state in a remote backend for team projects.
- Tag AWS resources consistently.
- Use IAM Roles instead of long-term credentials whenever possible.
- Use the latest supported Amazon Machine Image (AMI).
- Review `terraform plan` before every deployment.
- Destroy unused infrastructure to avoid unnecessary AWS charges.

---

# Common Mistakes

❌ Using an AMI ID from the wrong AWS Region.

❌ Specifying a key pair that does not exist.

❌ Forgetting to configure AWS credentials.

❌ Skipping `terraform validate`.

❌ Committing `terraform.tfstate` to Git.

❌ Leaving EC2 instances running after testing.

---

# Interview Questions

### What Terraform resource creates an EC2 instance?

```hcl
resource "aws_instance"
```

---

### Which command previews infrastructure changes?

```bash
terraform plan
```

---

### Which command creates the infrastructure?

```bash
terraform apply
```

---

### Which command deletes the infrastructure?

```bash
terraform destroy
```

---

### Which attribute returns the public IP address?

```hcl
aws_instance.web.public_ip
```

---

### Why should variables be used instead of hardcoded values?

Variables improve reusability, flexibility, and make Terraform configurations easier to manage across multiple environments.

---

# Summary

Creating an EC2 instance is one of the best beginner Terraform projects because it introduces the complete Infrastructure as Code workflow from configuration to deployment.

Key concepts include:

- AWS Provider
- EC2
- `aws_instance`
- Variables
- Outputs
- `terraform init`
- `terraform plan`
- `terraform apply`
- `terraform destroy`
- Infrastructure as Code

This project provides a strong foundation for more advanced AWS Terraform projects involving VPCs, RDS, EKS, Lambda, and complete cloud architectures.