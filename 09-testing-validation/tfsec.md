# 🔒 tfsec

## 📖 Introduction

**tfsec** is an **open-source security scanner for Terraform** that helps identify security vulnerabilities and misconfigurations **before infrastructure is deployed**.

It analyzes Terraform configuration files and checks them against hundreds of built-in security rules covering AWS, Azure, Google Cloud, Kubernetes, and more.

By running tfsec during development or in a CI/CD pipeline, teams can detect security issues early and build more secure Infrastructure as Code (IaC).

> **Note:** tfsec is maintained by Aqua Security. Many of its rules have also been incorporated into **Trivy**, but tfsec is still widely recognized and commonly encountered in existing DevOps projects and interviews.

---

# What is tfsec?

tfsec is a static analysis tool that scans Terraform code for security problems.

```
Terraform Code

↓

tfsec Scan

↓

Security Report

↓

Fix Issues

↓

Deploy
```

It checks your Terraform configuration without creating or modifying any cloud resources.

---

# Why Use tfsec?

Without tfsec:

```
Write Terraform

↓

Deploy

↓

Security Vulnerability Found
```

Problems:

- Open security groups
- Unencrypted storage
- Weak IAM permissions
- Publicly exposed resources
- Compliance failures

---

With tfsec:

```
Write Terraform

↓

Run tfsec

↓

Fix Problems

↓

Deploy Secure Infrastructure
```

Benefits:

- Early security detection
- Faster remediation
- Better compliance
- Reduced security risks
- DevSecOps integration

---

# Installation

### macOS

```bash
brew install tfsec
```

---

### Windows (Chocolatey)

```powershell
choco install tfsec
```

---

### Linux

Download the latest release from the official GitHub repository or install using your preferred package manager.

---

### Verify Installation

```bash
tfsec --version
```

---

# Basic Scan

Scan the current directory:

```bash
tfsec .
```

Scan a specific folder:

```bash
tfsec terraform/
```

---

# Example

Terraform code:

```hcl
resource "aws_security_group" "web" {

  ingress {

    from_port = 22

    to_port = 22

    protocol = "tcp"

    cidr_blocks = ["0.0.0.0/0"]

  }

}
```

Run:

```bash
tfsec .
```

Example output:

```text
HIGH

Security group allows unrestricted SSH access
```

tfsec detects insecure configurations before deployment.

---

# Common Security Checks

### AWS

- Public S3 buckets
- Missing encryption
- Open security groups
- Weak IAM policies
- Unencrypted EBS volumes
- RDS encryption

---

### Azure

- Storage account encryption
- Network Security Groups
- Managed identities
- Key Vault configuration

---

### Google Cloud

- Public storage buckets
- Firewall rules
- IAM permissions
- Compute Engine security

---

### Kubernetes

- Privileged containers
- Security contexts
- Resource limits
- Network policies

---

# Output Formats

Default output:

```bash
tfsec .
```

JSON output:

```bash
tfsec . --format json
```

SARIF output:

```bash
tfsec . --format sarif
```

JUnit output:

```bash
tfsec . --format junit
```

These formats are useful for CI/CD pipelines and security reporting.

---

# Excluding Checks

Skip specific rules:

```bash
tfsec . --exclude AWS006
```

Only exclude checks when you fully understand the security implications.

---

# CI/CD Integration

tfsec is commonly integrated into automated pipelines.

Workflow:

```text
Developer Pushes Code
          │
          ▼
GitHub Actions
          │
          ▼
Run tfsec
          │
          ▼
Security Report
          │
          ▼
Pass / Fail Pipeline
```

This prevents insecure infrastructure from being deployed automatically.

---

# Example GitHub Actions Step

```yaml
- name: Run tfsec

  run: tfsec .
```

---

# tfsec Workflow

```text
Write Terraform
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
tfsec
        │
        ▼
terraform plan
        │
        ▼
terraform apply
```

---

# tfsec vs terraform validate

| terraform validate | tfsec |
|--------------------|--------|
| Checks syntax and configuration | Checks security |
| Built into Terraform | External security tool |
| Detects configuration errors | Detects security vulnerabilities |
| No security analysis | Security-focused analysis |

Both tools should be used together.

---

# tfsec vs Checkov

| tfsec | Checkov |
|--------|----------|
| Primarily focused on Terraform security | Supports Terraform and many other IaC technologies |
| Lightweight security scanner | Security and compliance platform |
| Terraform-specific rules | Multi-platform scanning |
| Fast and simple | Broader feature set |

Both tools are widely used in DevSecOps environments.

---

# Easy Way to Remember

Think of airport security.

```
Passenger

↓

Security Screening

↓

Safe Flight
```

Terraform works the same way.

```
Terraform Code

↓

tfsec

↓

Secure Deployment
```

tfsec ensures security issues are detected before infrastructure is deployed.

---

# Best Practices

- Run tfsec before every deployment.
- Integrate tfsec into CI/CD pipelines.
- Investigate every high and critical finding.
- Keep tfsec updated to receive the latest security rules.
- Combine tfsec with `terraform fmt`, `terraform validate`, TFLint, and Checkov.
- Follow the principle of least privilege when designing infrastructure.

---

# Common Mistakes

❌ Ignoring high-severity security findings.

❌ Excluding security checks without proper justification.

❌ Running tfsec only before production deployments.

❌ Assuming `terraform validate` performs security analysis.

❌ Using outdated versions of tfsec.

---

# Interview Questions

### What is tfsec?

tfsec is an open-source security scanner that analyzes Terraform code for security vulnerabilities and misconfigurations.

---

### Does tfsec modify infrastructure?

No. It only analyzes Terraform configuration files and reports security issues.

---

### Which command scans the current Terraform project?

```bash
tfsec .
```

---

### Can tfsec be integrated into CI/CD pipelines?

Yes. It is commonly integrated with GitHub Actions, GitLab CI, Jenkins, Azure DevOps, and other CI/CD platforms.

---

### What is the difference between `terraform validate` and tfsec?

`terraform validate` checks configuration syntax and validity, while tfsec focuses on identifying security vulnerabilities and insecure configurations.

---

### Why is tfsec important?

It helps detect security issues before deployment, reducing risk and supporting secure Infrastructure as Code practices.

---

# Summary

tfsec is one of the most popular Terraform security scanners, helping teams identify vulnerabilities before infrastructure reaches production.

Key concepts include:

- Terraform security scanning
- Static analysis
- Security vulnerabilities
- Misconfiguration detection
- DevSecOps
- CI/CD integration
- Security best practices

Using tfsec together with `terraform fmt`, `terraform validate`, TFLint, and Checkov creates a strong, secure, and production-ready Terraform workflow.