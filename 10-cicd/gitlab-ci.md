# 🦊 Terraform with GitLab CI/CD

## 📖 Introduction

**GitLab CI/CD** is GitLab's built-in Continuous Integration and Continuous Deployment platform.

It allows developers to automate the entire Terraform workflow, including:

- Formatting Terraform code
- Validating configurations
- Running linting
- Performing security scans
- Generating execution plans
- Deploying infrastructure
- Destroying infrastructure (when required)

GitLab CI/CD is widely used in enterprise environments because it integrates source control, CI/CD, issue tracking, and DevOps workflows into a single platform.

---

# Why Use GitLab CI/CD with Terraform?

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

- Manual deployments
- Human errors
- Inconsistent infrastructure
- Difficult collaboration

---

With GitLab CI/CD:

```
Developer Pushes Code

↓

GitLab Pipeline

↓

Terraform Workflow

↓

Deploy Infrastructure
```

Everything is automated and repeatable.

---

# CI/CD Workflow

```text
Developer Pushes Code
          │
          ▼
GitLab Repository
          │
          ▼
GitLab Runner
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
Manual Approval (Optional)
          │
          ▼
terraform apply
```

---

# Project Structure

```text
terraform-project/

├── .gitlab-ci.yml
├── main.tf
├── variables.tf
├── outputs.tf
└── versions.tf
```

The GitLab pipeline configuration is stored in:

```text
.gitlab-ci.yml
```

---

# Basic Pipeline Example

```yaml
image: hashicorp/terraform:latest

stages:
  - validate
  - plan

validate:
  stage: validate
  script:
    - terraform init
    - terraform validate

plan:
  stage: plan
  script:
    - terraform plan
```

This pipeline initializes Terraform, validates the configuration, and generates an execution plan.

---

# Formatting Check

```yaml
fmt:
  stage: validate
  script:
    - terraform fmt -check -recursive
```

Ensures Terraform code follows the official formatting standard.

---

# Validation

```yaml
validate:
  stage: validate
  script:
    - terraform init
    - terraform validate
```

Checks that the Terraform configuration is valid.

---

# Linting

Example using TFLint:

```yaml
lint:
  stage: validate
  script:
    - tflint --init
    - tflint
```

Detects provider-specific issues and best practice violations.

---

# Security Scanning

Example using Checkov:

```yaml
security:
  stage: validate
  script:
    - checkov -d .
```

This scans Terraform code for security and compliance issues.

---

# Execution Plan

```yaml
plan:
  stage: plan
  script:
    - terraform plan
```

Displays the infrastructure changes Terraform intends to make.

---

# Deployment

```yaml
apply:
  stage: deploy
  script:
    - terraform apply -auto-approve
```

> **Warning:** Automatically applying changes to production is generally **not recommended**. Production deployments typically require manual approval.

---

# Authentication

Terraform needs credentials to communicate with cloud providers.

Common approaches:

### AWS

Use GitLab CI/CD Variables:

```
AWS_ACCESS_KEY_ID

AWS_SECRET_ACCESS_KEY
```

---

### Azure

Use:

- Service Principal
- OpenID Connect (OIDC) federation
- GitLab CI/CD Variables

---

### Google Cloud

Use:

- Service Account
- Workload Identity Federation
- GitLab CI/CD Variables

Never hardcode credentials inside `.gitlab-ci.yml`.

---

# GitLab CI/CD Variables

Sensitive values should be stored securely.

```
GitLab Project

↓

Settings

↓

CI/CD

↓

Variables
```

Common variables:

- Cloud credentials
- API tokens
- Secrets
- Terraform variables

---

# Pipeline Architecture

```text
Developer
      │
      ▼
Git Push
      │
      ▼
GitLab Repository
      │
      ▼
GitLab Runner
      │
      ▼
Terraform Pipeline
      │
      ▼
Cloud Provider
      │
      ▼
Infrastructure
```

---

# Complete Terraform Pipeline

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
Manual Approval
    │
    ▼
terraform apply
```

---

# GitLab CI/CD vs Manual Deployment

| Manual | GitLab CI/CD |
|---------|--------------|
| Manual execution | Automated pipeline |
| Human errors | Consistent deployments |
| Slow | Faster delivery |
| Difficult collaboration | Team-friendly |
| Limited visibility | Pipeline history and logs |

---

# GitLab CI/CD vs GitHub Actions

| GitLab CI/CD | GitHub Actions |
|---------------|----------------|
| Uses `.gitlab-ci.yml` | Uses workflow files in `.github/workflows/` |
| Built into GitLab | Built into GitHub |
| Uses GitLab Runners | Uses GitHub-hosted or self-hosted runners |
| Strong DevOps integration | Strong GitHub ecosystem integration |

Both platforms support automated Terraform deployments.

---

# Easy Way to Remember

Think of an automated factory.

```
Raw Materials

↓

Assembly Line

↓

Finished Product
```

Terraform follows a similar process.

```
Push Code

↓

GitLab CI/CD

↓

Automatic Infrastructure Deployment
```

---

# Best Practices

- Store credentials using GitLab CI/CD Variables or OIDC federation.
- Run `terraform fmt`, `terraform validate`, TFLint, and Checkov/tfsec before deployment.
- Review `terraform plan` before applying changes.
- Use remote backends for Terraform state.
- Protect production branches.
- Require manual approval for production deployments.
- Pin Docker images and tool versions where practical for reproducible builds.

---

# Common Mistakes

❌ Hardcoding credentials in `.gitlab-ci.yml`.

❌ Automatically deploying production on every commit.

❌ Skipping validation and security scans.

❌ Ignoring failed pipeline jobs.

❌ Using local Terraform state in collaborative environments.

---

# Interview Questions

### What is GitLab CI/CD?

GitLab CI/CD is GitLab's built-in platform for automating software build, test, and deployment workflows.

---

### Where is the GitLab pipeline configuration stored?

```text
.gitlab-ci.yml
```

---

### Why use GitLab CI/CD with Terraform?

It automates formatting, validation, security scanning, planning, and infrastructure deployment.

---

### What executes GitLab pipelines?

**GitLab Runners** execute the pipeline jobs.

---

### Should production deployments always use `terraform apply -auto-approve`?

No. Production deployments usually require manual approval or protected deployment workflows.

---

### Where should cloud credentials be stored?

In **GitLab CI/CD Variables** or by using secure identity federation methods such as OIDC, not inside pipeline files.

---

# Summary

GitLab CI/CD provides a powerful platform for automating Terraform workflows from code commit to infrastructure deployment.

Key concepts include:

- GitLab CI/CD
- GitLab Runners
- `.gitlab-ci.yml`
- CI/CD Variables
- OIDC
- `terraform fmt`
- `terraform validate`
- TFLint
- Checkov
- tfsec
- `terraform plan`
- `terraform apply`

Integrating Terraform with GitLab CI/CD helps teams deliver secure, consistent, and production-ready Infrastructure as Code through automated pipelines.