# 🔍 Terraform Linting (TFLint)

## 📖 Introduction

**Linting** is the process of analyzing source code to detect potential problems, bad practices, and style issues **before the code is executed**.

For Terraform, the most popular linting tool is **TFLint**.

TFLint goes beyond Terraform's built-in validation by checking for:

- Best practice violations
- Invalid resource configurations
- Deprecated syntax
- Cloud provider-specific issues
- Naming and configuration mistakes

Linting helps improve the quality, consistency, and reliability of Infrastructure as Code (IaC).

---

# What is TFLint?

**TFLint (Terraform Linter)** is an open-source static analysis tool for Terraform.

It scans Terraform configuration files and reports potential issues before deployment.

```
Terraform Code

↓

TFLint

↓

Warnings & Suggestions

↓

Fix Issues

↓

Deploy
```

Unlike `terraform validate`, TFLint focuses on **code quality and best practices**, not just configuration validity.

---

# Why Use TFLint?

Without linting:

```
Write Terraform

↓

Deploy

↓

Unexpected Problems
```

Possible issues:

- Wrong instance types
- Deprecated resources
- Unused variables
- Incorrect naming
- Provider-specific mistakes

---

With TFLint:

```
Write Terraform

↓

Run TFLint

↓

Fix Issues

↓

Deploy
```

Benefits:

- Better code quality
- Early error detection
- Consistent standards
- Fewer deployment issues
- Easier maintenance

---

# Installation

### macOS

```bash
brew install tflint
```

---

### Windows (Chocolatey)

```powershell
choco install tflint
```

---

### Linux

Download the latest release from the official GitHub repository or use your package manager if available.

---

### Verify Installation

```bash
tflint --version
```

---

# Initialize Plugins

Many provider-specific rules require plugin initialization.

```bash
tflint --init
```

Run this before using TFLint for the first time in a project.

---

# Basic Scan

Scan the current directory:

```bash
tflint
```

Scan a specific directory:

```bash
tflint terraform/
```

---

# Example

Terraform code:

```hcl
resource "aws_instance" "web" {

  ami           = "ami-123456"

  instance_type = "t2.nano"

}
```

Run:

```bash
tflint
```

Example output:

```text
Warning:

AWS instance type may not be appropriate

Reference: aws_instance.web
```

TFLint reports potential problems before deployment.

---

# Configuration File

TFLint can be configured using:

```text
.tflint.hcl
```

Example:

```hcl
plugin "aws" {

  enabled = true

  version = "0.34.0"

  source = "github.com/terraform-linters/tflint-ruleset-aws"

}
```

This enables AWS-specific linting rules.

---

# Supported Providers

TFLint supports provider-specific rule sets, including:

- AWS
- Azure
- Google Cloud
- Kubernetes
- Terraform language rules

Additional community rule sets are also available.

---

# CI/CD Integration

TFLint is commonly integrated into CI/CD pipelines.

Workflow:

```text
Developer Pushes Code
          │
          ▼
GitHub Actions
          │
          ▼
Run TFLint
          │
          ▼
Fix Issues
          │
          ▼
Deploy
```

This ensures Terraform code meets quality standards before deployment.

---

# Example GitHub Actions Step

```yaml
- name: Run TFLint

  run: |

    tflint --init
    tflint
```

---

# TFLint Workflow

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
terraform plan
        │
        ▼
terraform apply
```

---

# terraform validate vs TFLint

| terraform validate | TFLint |
|--------------------|---------|
| Checks syntax and configuration | Checks quality and best practices |
| Built into Terraform | Separate linting tool |
| Detects configuration errors | Detects warnings and provider-specific issues |
| No style analysis | Static code analysis |

Both tools complement each other.

---

# TFLint vs Checkov

| TFLint | Checkov |
|---------|----------|
| Code quality and best practices | Security and compliance |
| Provider-specific rules | Security-focused rules |
| Static analysis | Security scanning |
| Helps prevent configuration mistakes | Helps prevent security misconfigurations |

Many teams run **both** TFLint and Checkov in their pipelines.

---

# Easy Way to Remember

Think of a grammar checker for writing.

```
Essay

↓

Grammar Checker

↓

Correct Mistakes
```

Terraform works similarly.

```
Terraform Code

↓

TFLint

↓

Better Code
```

TFLint improves code quality before deployment.

---

# Best Practices

- Run TFLint before every deployment.
- Initialize provider plugins with `tflint --init`.
- Use provider-specific rule sets.
- Integrate TFLint into CI/CD pipelines.
- Review and fix warnings instead of ignoring them.
- Combine TFLint with `terraform fmt`, `terraform validate`, and Checkov.

---

# Common Mistakes

❌ Assuming TFLint replaces `terraform validate`.

❌ Forgetting to run `tflint --init`.

❌ Ignoring warnings without understanding their impact.

❌ Running linting only before production releases.

❌ Using outdated rule sets or plugins.

---

# Interview Questions

### What is TFLint?

TFLint is an open-source linter for Terraform that performs static analysis and detects best practice violations and provider-specific issues.

---

### Does TFLint replace `terraform validate`?

No. `terraform validate` checks configuration validity, while TFLint focuses on code quality and best practices.

---

### Which command initializes provider plugins?

```bash
tflint --init
```

---

### Which command scans the current Terraform project?

```bash
tflint
```

---

### Can TFLint be integrated into CI/CD?

Yes. It is commonly used with GitHub Actions, GitLab CI, Jenkins, Azure DevOps, and other CI/CD platforms.

---

### Why is TFLint important?

It improves Terraform code quality, detects potential problems early, and encourages consistent Infrastructure as Code practices.

---

# Summary

TFLint is the most widely used linting tool for Terraform, helping developers identify best practice violations and provider-specific issues before deployment.

Key concepts include:

- Static code analysis
- Terraform linting
- TFLint
- Provider rule sets
- `.tflint.hcl`
- CI/CD integration
- Code quality
- Best practices

Using TFLint alongside `terraform fmt`, `terraform validate`, and Checkov helps build clean, secure, and production-ready Terraform infrastructure.