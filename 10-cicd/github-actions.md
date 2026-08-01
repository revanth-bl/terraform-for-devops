# 🚀 Terraform with GitHub Actions

## 📖 Introduction

**GitHub Actions** is GitHub's built-in Continuous Integration and Continuous Deployment (CI/CD) platform.

When combined with Terraform, GitHub Actions can automatically:

- Format Terraform code
- Validate configurations
- Run security scans
- Generate execution plans
- Deploy infrastructure
- Destroy infrastructure (when required)

This enables fully automated Infrastructure as Code (IaC) workflows whenever code is pushed to GitHub.

---

# Why Use GitHub Actions with Terraform?

Without automation:

```
Developer

↓

Open Terminal

↓

terraform init

↓

terraform plan

↓

terraform apply
```

Problems:

- Manual process
- Human errors
- Inconsistent deployments
- Difficult collaboration

---

With GitHub Actions:

```
Developer Pushes Code

↓

GitHub Actions

↓

Terraform Workflow

↓

Deploy Infrastructure
```

Everything runs automatically.

---

# CI/CD Workflow

```text
Developer Pushes Code
          │
          ▼
GitHub Repository
          │
          ▼
GitHub Actions
          │
          ▼
terraform fmt
          │
          ▼
terraform validate
          │
          ▼
TFLint
          │
          ▼
Checkov / tfsec
          │
          ▼
terraform plan
          │
          ▼
Approval (Optional)
          │
          ▼
terraform apply
```

---

# Project Structure

```text
terraform-project/

├── .github/
│   └── workflows/
│       └── terraform.yml
│
├── main.tf
├── variables.tf
├── outputs.tf
└── versions.tf
```

GitHub Actions workflows are stored inside:

```text
.github/workflows/
```

---

# Basic Workflow Example

```yaml
name: Terraform

on:
  push:
    branches:
      - main

jobs:
  terraform:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: hashicorp/setup-terraform@v3

      - run: terraform init

      - run: terraform validate

      - run: terraform plan
```

This workflow initializes Terraform, validates the configuration, and generates an execution plan.

---

# Formatting Check

```yaml
- name: Terraform Format

  run: terraform fmt -check -recursive
```

Ensures all Terraform files follow the standard formatting style.

---

# Validation

```yaml
- name: Terraform Validate

  run: terraform validate
```

Checks whether the configuration is valid.

---

# Linting

Example using TFLint:

```yaml
- name: Setup TFLint

  uses: terraform-linters/setup-tflint@v4

- name: Initialize TFLint

  run: tflint --init

- name: Run TFLint

  run: tflint
```

Detects best practice and provider-specific issues.

---

# Security Scanning

Example using Checkov:

```yaml
- name: Run Checkov

  uses: bridgecrewio/checkov-action@master

  with:

    directory: .
```

This scans Terraform code for security and compliance issues.

---

# Execution Plan

```yaml
- name: Terraform Plan

  run: terraform plan
```

The plan shows the infrastructure changes Terraform intends to make.

---

# Deployment

```yaml
- name: Terraform Apply

  run: terraform apply -auto-approve
```

> **Warning:** Automatically applying changes on every push is generally **not recommended for production**. Production deployments often require approvals or protected environments.

---

# Authentication

Terraform needs credentials to access cloud providers.

Common approaches:

### AWS

Use GitHub Secrets:

```
AWS_ACCESS_KEY_ID

AWS_SECRET_ACCESS_KEY
```

---

### Azure

Use:

- Service Principal
- OpenID Connect (OIDC) federation (recommended)
- GitHub Secrets (if OIDC is not available)

---

### Google Cloud

Use:

- Service Account
- Workload Identity Federation (recommended)
- GitHub Secrets (if required)

Never store credentials directly inside workflow files.

---

# GitHub Secrets

Store sensitive information securely.

Example:

```
Repository

↓

Settings

↓

Secrets and Variables

↓

Actions

↓

Secrets
```

Typical secrets:

- Cloud credentials
- API keys
- Tokens

---

# Workflow Architecture

```text
Developer
      │
      ▼
Git Push
      │
      ▼
GitHub Repository
      │
      ▼
GitHub Actions
      │
      ▼
Terraform Workflow
      │
      ▼
Cloud Provider
      │
      ▼
Infrastructure
```

---

# Complete CI/CD Pipeline

```text
Git Push
    │
    ▼
Checkout Code
    │
    ▼
terraform fmt
    │
    ▼
terraform validate
    │
    ▼
TFLint
    │
    ▼
Checkov / tfsec
    │
    ▼
terraform plan
    │
    ▼
Manual Approval (Production)
    │
    ▼
terraform apply
```

---

# GitHub Actions vs Manual Deployment

| Manual | GitHub Actions |
|---------|----------------|
| Manual execution | Automatic execution |
| Human errors | Consistent workflow |
| Slow | Fast |
| Difficult collaboration | Team-friendly |
| Hard to audit | Workflow history and logs |

---

# Easy Way to Remember

Think of an automatic car wash.

```
Drive In

↓

Automatic Cleaning

↓

Drive Out
```

Terraform CI/CD works similarly.

```
Push Code

↓

GitHub Actions

↓

Automatic Infrastructure Deployment
```

---

# Best Practices

- Store credentials using GitHub Secrets or OIDC federation.
- Run `terraform fmt`, `terraform validate`, TFLint, and Checkov/tfsec before deployment.
- Review `terraform plan` before applying changes.
- Use remote state backends.
- Protect production branches.
- Require manual approvals for production deployments.
- Pin GitHub Action versions instead of using floating versions when appropriate.

---

# Common Mistakes

❌ Storing cloud credentials in workflow files.

❌ Automatically deploying production on every commit.

❌ Skipping validation and security scans.

❌ Ignoring failed CI/CD checks.

❌ Using local state in team environments.

---

# Interview Questions

### What is GitHub Actions?

GitHub Actions is GitHub's built-in CI/CD platform for automating software development workflows.

---

### Why use GitHub Actions with Terraform?

It automates formatting, validation, testing, security scanning, planning, and infrastructure deployment.

---

### Where are GitHub Actions workflows stored?

```text
.github/workflows/
```

---

### Which command generates the Terraform execution plan?

```bash
terraform plan
```

---

### Should production deployments always use `terraform apply -auto-approve`?

No. Production environments usually require manual approvals or protected deployment workflows.

---

### Where should cloud credentials be stored?

In GitHub Secrets or by using secure identity federation methods such as OpenID Connect (OIDC), not directly in workflow files.

---

# Summary

GitHub Actions enables automated, reliable, and repeatable Terraform deployments by integrating Infrastructure as Code into modern CI/CD pipelines.

Key concepts include:

- GitHub Actions
- CI/CD
- Workflow files
- GitHub Secrets
- OIDC
- `terraform fmt`
- `terraform validate`
- TFLint
- Checkov
- tfsec
- `terraform plan`
- `terraform apply`

Combining GitHub Actions with Terraform helps teams build secure, scalable, and production-ready Infrastructure as Code workflows.