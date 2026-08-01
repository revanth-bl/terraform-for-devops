# 🤖 Terraform with Jenkins

## 📖 Introduction

**Jenkins** is one of the most popular open-source **Continuous Integration and Continuous Deployment (CI/CD)** automation servers.

When integrated with Terraform, Jenkins can automate the complete Infrastructure as Code (IaC) workflow, including:

- Formatting Terraform code
- Validating configurations
- Running linting
- Performing security scans
- Generating execution plans
- Deploying infrastructure
- Destroying infrastructure (when required)

Jenkins is widely used in enterprise environments because of its flexibility, extensive plugin ecosystem, and support for self-hosted pipelines.

---

# Why Use Jenkins with Terraform?

Without Jenkins:

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
- Time-consuming processes
- Inconsistent workflows

---

With Jenkins:

```
Developer Pushes Code

↓

Jenkins Pipeline

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
Git Repository
          │
          ▼
Jenkins
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

├── Jenkinsfile
├── main.tf
├── variables.tf
├── outputs.tf
└── versions.tf
```

The pipeline configuration is stored in:

```text
Jenkinsfile
```

---

# Basic Jenkins Pipeline

```groovy
pipeline {

    agent any

    stages {

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'terraform validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan'
            }
        }
    }
}
```

This pipeline initializes Terraform, validates the configuration, and generates an execution plan.

---

# Formatting Check

```groovy
stage('Terraform Format') {

    steps {

        sh 'terraform fmt -check -recursive'

    }

}
```

Ensures Terraform files follow the official formatting style.

---

# Validation

```groovy
stage('Terraform Validate') {

    steps {

        sh 'terraform validate'

    }

}
```

Checks that the Terraform configuration is valid.

---

# Linting

Example using TFLint:

```groovy
stage('TFLint') {

    steps {

        sh 'tflint --init'
        sh 'tflint'

    }

}
```

Detects provider-specific issues and best practice violations.

---

# Security Scanning

Example using Checkov:

```groovy
stage('Checkov') {

    steps {

        sh 'checkov -d .'

    }

}
```

Scans Terraform code for security and compliance issues.

---

# Execution Plan

```groovy
stage('Terraform Plan') {

    steps {

        sh 'terraform plan'

    }

}
```

Displays the infrastructure changes Terraform intends to make.

---

# Deployment

```groovy
stage('Terraform Apply') {

    steps {

        sh 'terraform apply -auto-approve'

    }

}
```

> **Warning:** Automatically applying Terraform changes to production is generally **not recommended**. Production pipelines should include manual approval stages or protected deployment processes.

---

# Authentication

Terraform requires credentials to communicate with cloud providers.

Common approaches:

### AWS

Store credentials in **Jenkins Credentials** or use IAM Roles when Jenkins runs on AWS.

---

### Azure

Use:

- Service Principal
- OpenID Connect (OIDC) where supported
- Jenkins Credentials

---

### Google Cloud

Use:

- Service Account
- Workload Identity Federation
- Jenkins Credentials

Never store credentials directly inside the `Jenkinsfile`.

---

# Jenkins Credentials

Sensitive values should be stored securely.

```
Jenkins Dashboard

↓

Manage Jenkins

↓

Credentials
```

Typical credentials:

- AWS Access Keys
- Azure Service Principal secrets
- GCP Service Account keys
- API tokens
- SSH keys

---

# Pipeline Architecture

```text
Developer
      │
      ▼
Git Push
      │
      ▼
Git Repository
      │
      ▼
Jenkins
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

# Jenkins vs GitHub Actions

| Jenkins | GitHub Actions |
|----------|----------------|
| Self-hosted automation server | Cloud-based CI/CD integrated with GitHub |
| Uses `Jenkinsfile` | Uses workflow YAML files |
| Extensive plugin ecosystem | Native GitHub integration |
| Full infrastructure control | Simpler setup for GitHub repositories |

---

# Jenkins vs GitLab CI/CD

| Jenkins | GitLab CI/CD |
|----------|--------------|
| Separate CI/CD server | Built into GitLab |
| Uses Jenkins agents | Uses GitLab Runners |
| Highly customizable | Fully integrated DevOps platform |
| Large plugin ecosystem | Native GitLab features |

---

# Easy Way to Remember

Think of an automated factory manager.

```
Receive Order

↓

Manage Production

↓

Deliver Product
```

Jenkins works in a similar way.

```
Push Code

↓

Jenkins

↓

Automatic Infrastructure Deployment
```

---

# Best Practices

- Store secrets using Jenkins Credentials.
- Run `terraform fmt`, `terraform validate`, TFLint, and Checkov/tfsec before deployment.
- Review `terraform plan` before applying changes.
- Use remote Terraform backends.
- Require manual approval for production deployments.
- Keep Jenkins plugins updated.
- Use pipeline as code with a `Jenkinsfile`.

---

# Common Mistakes

❌ Hardcoding cloud credentials in the `Jenkinsfile`.

❌ Automatically deploying production after every commit.

❌ Skipping validation and security scans.

❌ Ignoring failed pipeline stages.

❌ Using local Terraform state for team projects.

---

# Interview Questions

### What is Jenkins?

Jenkins is an open-source automation server used to build CI/CD pipelines for software delivery and infrastructure automation.

---

### Which file defines a Jenkins Pipeline?

```text
Jenkinsfile
```

---

### Why use Jenkins with Terraform?

It automates formatting, validation, testing, security scanning, planning, and infrastructure deployment.

---

### Where should cloud credentials be stored?

In **Jenkins Credentials** or through secure identity mechanisms such as IAM Roles or OIDC—not inside the `Jenkinsfile`.

---

### Should production deployments always use `terraform apply -auto-approve`?

No. Production deployments should generally require manual approval or protected deployment workflows.

---

### Can Jenkins integrate with GitHub and GitLab?

Yes. Jenkins can trigger pipelines from GitHub, GitLab, Bitbucket, and many other version control systems.

---

# Summary

Jenkins is a powerful and flexible CI/CD platform that automates Terraform workflows from code commit to infrastructure deployment.

Key concepts include:

- Jenkins
- Jenkinsfile
- CI/CD
- Jenkins Credentials
- Terraform automation
- `terraform fmt`
- `terraform validate`
- TFLint
- Checkov
- tfsec
- `terraform plan`
- `terraform apply`

Combining Jenkins with Terraform enables reliable, repeatable, and production-ready Infrastructure as Code pipelines for teams of all sizes.