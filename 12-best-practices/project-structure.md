# 📁 Terraform Project Structure

## 📖 Introduction

A well-organized Terraform project is easier to understand, maintain, and scale. As infrastructure grows, placing everything in a single `main.tf` file quickly becomes difficult to manage.

A good project structure separates resources into logical files and directories, making collaboration and troubleshooting much simpler.

Whether you're working on a small personal project or a large enterprise deployment, following a consistent project structure is considered a Terraform best practice.

---

# Why Project Structure Matters

Poor project structure:

```text
main.tf

(1000+ lines)
```

Problems:

- Difficult to navigate
- Hard to debug
- Poor collaboration
- Code duplication
- Difficult maintenance

---

Good project structure:

```text
provider.tf

variables.tf

outputs.tf

network.tf

compute.tf

database.tf
```

Benefits:

- Better organization
- Easier debugging
- Cleaner code
- Improved collaboration
- Better scalability

---

# Basic Terraform Project Structure

A simple project might look like this:

```text
terraform-project/

├── main.tf
├── provider.tf
├── versions.tf
├── variables.tf
├── terraform.tfvars
├── outputs.tf
└── README.md
```

Suitable for:

- Small projects
- Learning Terraform
- Simple infrastructure

---

# Recommended Project Structure

For medium to large projects:

```text
terraform-project/

├── provider.tf
├── versions.tf
├── variables.tf
├── terraform.tfvars
├── locals.tf
├── outputs.tf
├── network.tf
├── compute.tf
├── database.tf
├── security.tf
├── storage.tf
├── monitoring.tf
├── modules/
│   ├── vpc/
│   ├── ec2/
│   ├── rds/
│   ├── eks/
│   └── iam/
└── README.md
```

Each file focuses on a specific part of the infrastructure.

---

# Purpose of Common Files

### provider.tf

Defines cloud providers.

Example:

```hcl
provider "aws" {

  region = var.aws_region

}
```

---

### versions.tf

Specifies Terraform and provider versions.

Example:

```hcl
terraform {

  required_version = ">= 1.5.0"

}
```

---

### variables.tf

Defines input variables.

Example:

```hcl
variable "aws_region" {}
```

---

### terraform.tfvars

Stores variable values.

Example:

```hcl
aws_region = "us-east-1"
```

---

### locals.tf

Stores reusable local values.

Example:

```hcl
locals {

  project = "terraform"

}
```

---

### outputs.tf

Displays useful values after deployment.

Example:

```hcl
output "vpc_id" {

  value = aws_vpc.main.id

}
```

---

# Organizing Resources

Instead of placing everything in `main.tf`:

```text
main.tf

↓

Everything
```

Split resources into logical files:

```text
network.tf

↓

VPC

Subnets

Route Tables

Internet Gateway
```

```text
compute.tf

↓

EC2

Auto Scaling

Launch Templates
```

```text
database.tf

↓

RDS

DB Subnet Groups
```

```text
security.tf

↓

IAM

Security Groups

Policies
```

This organization improves readability and maintenance.

---

# Module Structure

Reusable modules should have their own directory.

Example:

```text
modules/

├── vpc/

│   ├── main.tf

│   ├── variables.tf

│   ├── outputs.tf

│   └── README.md

├── ec2/

├── rds/

└── eks/
```

Modules help avoid code duplication and encourage reuse.

---

# Environment Structure

For multiple environments:

```text
environments/

├── dev/

├── test/

├── staging/

└── production/
```

Each environment can have its own variable values or backend configuration while sharing common modules.

---

# State Files

Do **not** organize state files manually.

Use remote backends such as:

- Amazon S3
- Azure Storage
- Google Cloud Storage
- Terraform Cloud

Avoid committing:

```text
terraform.tfstate
```

to Git repositories.

---

# Project Workflow

```text
Write Terraform
        │
        ▼
Organize Files
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
```

---

# Small vs Large Projects

| Small Project | Large Project |
|---------------|---------------|
| Few files | Multiple logical files |
| Minimal modules | Extensive reusable modules |
| Simple structure | Organized by components and environments |
| Suitable for learning | Suitable for production |

---

# Example Enterprise Structure

```text
terraform-project/

├── modules/
│   ├── networking/
│   ├── compute/
│   ├── storage/
│   ├── database/
│   ├── monitoring/
│   └── security/
│
├── environments/
│   ├── dev/
│   ├── test/
│   ├── staging/
│   └── production/
│
├── provider.tf
├── versions.tf
├── variables.tf
├── locals.tf
├── outputs.tf
└── README.md
```

This structure scales well for enterprise deployments.

---

# Easy Way to Remember

Think of organizing files in a house.

Bad:

```text
Everything

↓

One Room
```

Good:

```text
Kitchen

Bedroom

Living Room

Bathroom
```

Terraform projects should follow the same principle—group similar resources together.

---

# Best Practices

- Separate resources into logical files.
- Keep `main.tf` small or eliminate it if other files clearly organize the project.
- Use reusable modules.
- Create separate environments for development, testing, and production.
- Store Terraform state remotely.
- Document the project with a README.
- Keep file names descriptive and consistent.

---

# Common Mistakes

❌ Putting every resource into one file.

❌ Copying and pasting infrastructure instead of using modules.

❌ Mixing networking, compute, and database resources together.

❌ Committing `terraform.tfstate` to Git.

❌ Creating inconsistent directory structures across projects.

❌ Ignoring documentation.

---

# Interview Questions

### Why should Terraform projects be organized into multiple files?

It improves readability, maintainability, collaboration, and scalability.

---

### What is the purpose of `variables.tf`?

It defines input variables used by the Terraform configuration.

---

### What is the purpose of `outputs.tf`?

It displays useful values after infrastructure is deployed.

---

### Why use modules?

Modules eliminate code duplication and promote reusable infrastructure components.

---

### Should Terraform state files be committed to Git?

No. State files often contain sensitive information and should be stored in secure remote backends.

---

### Why separate development and production environments?

It allows independent infrastructure management, testing, and safer deployments.

---

# Summary

A well-structured Terraform project is easier to develop, maintain, and scale as infrastructure grows.

Key concepts include:

- Logical file organization
- Modules
- Environment separation
- Remote state
- Reusable infrastructure
- `provider.tf`
- `variables.tf`
- `outputs.tf`
- `locals.tf`
- `terraform.tfvars`
- Infrastructure as Code

Following a consistent project structure helps teams build clean, maintainable, and production-ready Terraform projects.