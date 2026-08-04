# ☸️ Deploying Amazon EKS with Terraform

## 📖 Introduction

**Amazon Elastic Kubernetes Service (Amazon EKS)** is AWS's managed Kubernetes service.

Instead of manually creating VPCs, IAM roles, security groups, node groups, and the EKS cluster through the AWS Console, Terraform allows you to provision the entire Kubernetes infrastructure as code.

Using Terraform with Amazon EKS provides:

- Automated cluster provisioning
- Version-controlled infrastructure
- Repeatable deployments
- Easier scaling and maintenance
- Consistent environments

---

# What is Amazon EKS?

Amazon EKS is a managed Kubernetes service where AWS manages the Kubernetes control plane while you manage your applications and worker nodes (or use AWS Fargate).

```
Terraform

↓

AWS Provider

↓

Amazon EKS

↓

Kubernetes Cluster
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
Amazon EKS
      │
      ▼
Managed Node Group
      │
      ▼
Kubernetes Pods
```

---

# Project Structure

```text
eks-project/

├── provider.tf
├── versions.tf
├── variables.tf
├── terraform.tfvars
├── vpc.tf
├── iam.tf
├── eks.tf
├── nodegroup.tf
├── outputs.tf
└── README.md
```

---

# Prerequisites

Before deploying an EKS cluster, ensure you have:

- AWS Account
- Terraform installed
- AWS CLI installed
- kubectl installed
- AWS credentials configured
- Basic understanding of Kubernetes

Verify installations:

```bash
terraform version
```

```bash
aws --version
```

```bash
kubectl version --client
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

# Define Terraform Version

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

The EKS cluster requires networking resources.

Terraform typically creates:

- VPC
- Public Subnets
- Private Subnets
- Internet Gateway
- Route Tables
- NAT Gateway (recommended for production)

```
Internet

↓

Internet Gateway

↓

VPC

↓

Public & Private Subnets

↓

Amazon EKS
```

---

# Create IAM Roles

Terraform provisions IAM roles for:

- EKS Cluster
- Worker Nodes
- AWS services used by Kubernetes

These roles grant the permissions required for cluster management.

---

# Create the EKS Cluster

Example:

```hcl
resource "aws_eks_cluster" "main" {

  name     = "terraform-eks"

  role_arn = aws_iam_role.eks_cluster.arn

}
```

Terraform creates the managed Kubernetes control plane.

---

# Create a Managed Node Group

Example:

```hcl
resource "aws_eks_node_group" "workers" {

  cluster_name = aws_eks_cluster.main.name

  node_role_arn = aws_iam_role.worker.arn

}
```

Managed node groups provide EC2 instances that run Kubernetes workloads.

---

# Outputs

Example:

```hcl
output "cluster_name" {

  value = aws_eks_cluster.main.name

}
```

Useful outputs include:

- Cluster name
- Cluster endpoint
- Cluster ARN

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
Plan: 25 to add, 0 to change, 0 to destroy.
```

---

# Deploy the Cluster

```bash
terraform apply
```

Confirm:

```text
yes
```

Provisioning an EKS cluster may take 15–30 minutes depending on the configuration.

---

# Configure kubectl

After deployment:

```bash
aws eks update-kubeconfig --region us-east-1 --name terraform-eks
```

Verify connectivity:

```bash
kubectl get nodes
```

Example:

```text
NAME                 STATUS

ip-10-0-1-25         Ready
```

---

# Deploy an Application

Example:

```bash
kubectl create deployment nginx --image=nginx
```

Expose the deployment:

```bash
kubectl expose deployment nginx --port=80 --type=LoadBalancer
```

Verify:

```bash
kubectl get services
```

---

# Destroy Infrastructure

```bash
terraform destroy
```

Confirm:

```text
yes
```

Terraform removes the cluster and associated AWS resources.

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
Amazon EKS Cluster
      │
      ▼
kubectl
      │
      ▼
Deploy Applications
```

---

# Amazon EKS Architecture

```text
Internet
      │
      ▼
Load Balancer
      │
      ▼
Amazon EKS Cluster
      │
      ▼
Worker Nodes
      │
      ▼
Pods
```

---

# Best Practices

- Use private subnets for worker nodes in production.
- Enable IAM Roles for Service Accounts (IRSA).
- Store Terraform state remotely.
- Enable Kubernetes logging and monitoring.
- Use managed node groups whenever possible.
- Tag AWS resources consistently.
- Upgrade Kubernetes versions regularly.
- Review `terraform plan` before deployment.

---

# Common Mistakes

❌ Deploying worker nodes only in public subnets for production workloads.

❌ Granting excessive IAM permissions.

❌ Forgetting to update the kubeconfig file.

❌ Ignoring Terraform validation.

❌ Using local Terraform state in team environments.

❌ Leaving test clusters running and incurring unnecessary AWS costs.

---

# Interview Questions

### What is Amazon EKS?

Amazon EKS is AWS's managed Kubernetes service.

---

### Which Terraform resource creates an EKS cluster?

```hcl
resource "aws_eks_cluster"
```

---

### Which Terraform resource creates managed worker nodes?

```hcl
resource "aws_eks_node_group"
```

---

### Which command configures `kubectl` for an EKS cluster?

```bash
aws eks update-kubeconfig --region us-east-1 --name terraform-eks
```

---

### Which command lists Kubernetes nodes?

```bash
kubectl get nodes
```

---

### Why is a VPC required for EKS?

The VPC provides networking for the Kubernetes control plane, worker nodes, and communication with other AWS resources.

---

# Summary

Amazon EKS is one of the most popular production Kubernetes platforms, and Terraform makes provisioning and managing EKS infrastructure simple, repeatable, and version-controlled.

Key concepts include:

- Amazon EKS
- Kubernetes
- `aws_eks_cluster`
- `aws_eks_node_group`
- VPC
- IAM
- `kubectl`
- `terraform init`
- `terraform plan`
- `terraform apply`
- Infrastructure as Code

Learning to deploy Amazon EKS with Terraform is an essential skill for DevOps and Cloud Engineers working with Kubernetes on AWS.