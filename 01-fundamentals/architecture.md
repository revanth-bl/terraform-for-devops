# 🏗️ Terraform Architecture

## 📖 What is Terraform Architecture?

Terraform follows a simple workflow to provision infrastructure.

Instead of manually creating cloud resources, Terraform reads your configuration files, creates an execution plan, communicates with the cloud provider using APIs, and provisions the infrastructure while tracking everything in a state file.

---

# High-Level Architecture

```text
                 Developer
                     │
                     │
          terraform apply
                     │
                     ▼
        ┌────────────────────┐
        │ Terraform CLI      │
        └────────────────────┘
                     │
             Reads .tf files
                     │
                     ▼
         ┌───────────────────┐
         │ Terraform Core    │
         └───────────────────┘
              │         │
              │         │
       State Manager    │
              │         │
              ▼         ▼
        terraform.tfstate
                     │
             Provider Plugin
                     │
             AWS / Azure / GCP
                     │
                     ▼
             Cloud Provider APIs
                     │
                     ▼
     EC2 • VPC • IAM • RDS • S3 • Lambda
```

---

# Main Components

Terraform architecture consists of four major components.

---

## 1️⃣ Terraform CLI

The CLI is the command-line interface that users interact with.

Examples

```bash
terraform init
terraform plan
terraform apply
terraform destroy
```

Responsibilities

- Reads Terraform configuration files
- Executes commands
- Displays output
- Interacts with Terraform Core

---

## 2️⃣ Terraform Core

Terraform Core is the brain of Terraform.

It performs tasks such as

- Reading configuration files
- Building the dependency graph
- Comparing infrastructure with the state file
- Creating an execution plan
- Managing state
- Calling providers

Terraform Core never talks directly to AWS or Azure.

Instead, it communicates through providers.

---

## 3️⃣ Provider Plugins

Providers act as translators between Terraform and cloud platforms.

Examples

- AWS Provider
- Azure Provider
- Google Provider
- Kubernetes Provider
- Docker Provider

Example

```hcl
provider "aws" {
  region = "us-east-1"
}
```

The AWS Provider converts Terraform instructions into AWS API requests.

---

## 4️⃣ Cloud APIs

Every cloud provider exposes APIs.

Examples

AWS

```
Create EC2 Instance
Create VPC
Create S3 Bucket
Delete IAM Role
```

Azure

```
Create Virtual Machine
Create Resource Group
```

Terraform providers call these APIs automatically.

---

# How Terraform Works

Step 1

You write configuration files.

```text
main.tf
variables.tf
outputs.tf
```

↓

Step 2

Run

```bash
terraform init
```

Terraform downloads provider plugins.

↓

Step 3

Run

```bash
terraform plan
```

Terraform compares

- Configuration
- Current Infrastructure
- State File

and creates an execution plan.

↓

Step 4

Run

```bash
terraform apply
```

Terraform

- Creates resources
- Updates resources
- Deletes resources

↓

Step 5

Terraform updates

```
terraform.tfstate
```

This file stores information about your infrastructure.

---

# Dependency Graph

Terraform automatically understands dependencies.

Example

```hcl
resource "aws_vpc" "main" {}

resource "aws_subnet" "public" {
  vpc_id = aws_vpc.main.id
}
```

Terraform knows

```
Create VPC
      ↓
Create Subnet
```

No manual ordering is required.

---

# State File

Terraform stores infrastructure information inside

```
terraform.tfstate
```

The state file contains

- Resource IDs
- Attributes
- Dependencies
- Current infrastructure

Terraform uses this file to determine what needs to change.

---

# Workflow Diagram

```text
Developer
     │
     ▼
Write .tf Files
     │
     ▼
terraform init
     │
     ▼
Download Providers
     │
     ▼
terraform plan
     │
     ▼
Execution Plan
     │
     ▼
terraform apply
     │
     ▼
Cloud APIs
     │
     ▼
Infrastructure Created
     │
     ▼
terraform.tfstate Updated
```

---

# Easy Way to Remember

Think of Terraform like ordering food online.

```
You
 │
 ▼
Terraform CLI
 │
 ▼
Terraform Core
 │
 ▼
Provider
 │
 ▼
Restaurant (AWS/Azure)
 │
 ▼
Food Delivered (Infrastructure)
```

- CLI → Takes your order
- Core → Processes the order
- Provider → Calls the restaurant
- Cloud API → Cooks the food
- State File → Saves your order history

---

# Advantages

- Infrastructure as Code
- Repeatable deployments
- Version controlled
- Automatic dependency management
- Multi-cloud support
- Scalable
- Easy collaboration

---

# Common Mistakes

❌ Editing the state file manually

❌ Forgetting to run `terraform init`

❌ Deleting cloud resources outside Terraform

❌ Committing sensitive state files to GitHub

❌ Ignoring execution plans

---

# Interview Questions

### What are the main components of Terraform architecture?

Terraform CLI, Terraform Core, Providers, and Cloud Provider APIs.

---

### Does Terraform communicate directly with AWS?

No.

Terraform Core communicates with provider plugins, and providers communicate with AWS APIs.

---

### What is the role of Terraform Core?

It reads configurations, builds dependency graphs, manages state, creates execution plans, and communicates with providers.

---

### Why are providers required?

Providers translate Terraform configurations into API calls understood by cloud platforms.

---

### What is the purpose of the state file?

It stores the current infrastructure state, allowing Terraform to determine what changes are needed.

---

# Summary

Terraform Architecture consists of:

- Terraform CLI
- Terraform Core
- Provider Plugins
- Cloud Provider APIs
- State File

Understanding this architecture helps explain how Terraform plans, provisions, updates, and manages infrastructure efficiently.