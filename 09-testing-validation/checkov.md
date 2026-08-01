# 🛡️ Checkov

## 📖 Introduction

**Checkov** is an **Infrastructure as Code (IaC) security and compliance scanner** developed by **Bridgecrew (now part of Palo Alto Networks)**.

It scans Terraform, CloudFormation, Kubernetes, Docker, GitHub Actions, Azure Resource Manager (ARM), Bicep, Helm, and many other Infrastructure as Code files to identify:

- Security vulnerabilities
- Misconfigurations
- Compliance violations
- Best practice issues

Checkov helps teams detect problems **before infrastructure is deployed**, making it an essential tool in modern DevSecOps workflows.

---

# What is Checkov?

Think of Checkov as a **security auditor** for your Infrastructure as Code.

```
Terraform Code

↓

Checkov Scan

↓

Security Report

↓

Fix Issues

↓

Deploy
```

Instead of discovering security issues after deployment, Checkov finds them during development or CI/CD.

---

# Why Use Checkov?

Without Checkov:

```
Write Terraform

↓

Deploy

↓

Security Issue Found
```

Problems:

- Security risks
- Compliance failures
- Costly fixes
- Potential data exposure

---

With Checkov:

```
Write Terraform

↓

Run Checkov

↓

Fix Problems

↓

Deploy Secure Infrastructure
```

Benefits:

- Early security detection
- Compliance validation
- Automated scanning
- CI/CD integration
- Better infrastructure quality

---

# Installation

Using pip:

```bash
pip install checkov
```

Using Homebrew (macOS):

```bash
brew install checkov
```

Using Docker:

```bash
docker run --rm -v $(pwd):/tf bridgecrew/checkov -d /tf
```

Verify installation:

```bash
checkov --version
```

---

# Basic Scan

Scan the current directory:

```bash
checkov -d .
```

Scan a specific folder:

```bash
checkov -d terraform/
```

Scan a single file:

```bash
checkov -f main.tf
```

---

# Example

Terraform code:

```hcl
resource "aws_s3_bucket" "data" {

  bucket = "my-bucket"

}
```

Run:

```bash
checkov -f main.tf
```

Example output:

```text
Check: CKV_AWS_21

FAILED

Ensure S3 bucket has versioning enabled
```

Checkov identifies missing security configurations before deployment.

---

# Supported Technologies

Checkov supports scanning:

- Terraform
- Terraform Plan
- Kubernetes
- Helm
- Dockerfiles
- CloudFormation
- Azure ARM Templates
- Azure Bicep
- GitHub Actions
- GitLab CI
- CircleCI
- Kubernetes YAML
- Serverless Framework
- And many more

---

# Common Checks

Checkov validates many security best practices, such as:

AWS

- S3 encryption
- S3 versioning
- IAM permissions
- Security Groups
- EC2 configuration
- EBS encryption
- RDS encryption

Azure

- Storage encryption
- Network Security Groups
- Managed Identity
- Key Vault configuration

Google Cloud

- IAM policies
- Storage bucket security
- Firewall rules
- Compute Engine configuration

Kubernetes

- Privileged containers
- Security contexts
- Resource limits
- Network policies

---

# Running Specific Checks

Run only selected checks:

```bash
checkov -d . --check CKV_AWS_21
```

Skip specific checks:

```bash
checkov -d . --skip-check CKV_AWS_21
```

---

# Output Formats

Default output:

```bash
checkov -d .
```

JSON output:

```bash
checkov -d . -o json
```

JUnit XML:

```bash
checkov -d . -o junitxml
```

SARIF (useful for GitHub code scanning):

```bash
checkov -d . -o sarif
```

---

# CI/CD Integration

Checkov is commonly integrated into CI/CD pipelines.

Example workflow:

```text
Developer Pushes Code
          │
          ▼
GitHub Actions
          │
          ▼
Run Checkov
          │
          ▼
Security Report
          │
          ▼
Pass / Fail Pipeline
```

This prevents insecure infrastructure from being deployed automatically.

---

# Checkov Workflow

```text
Write Terraform
        │
        ▼
Run Checkov
        │
        ▼
Security Scan
        │
        ▼
Fix Issues
        │
        ▼
Deploy Infrastructure
```

---

# Example GitHub Actions Step

```yaml
- name: Run Checkov

  uses: bridgecrewio/checkov-action@master

  with:

    directory: .
```

This scans your Terraform code during every GitHub Actions workflow run.

---

# Checkov vs Terraform Validate

| Terraform Validate | Checkov |
|---------------------|----------|
| Checks Terraform syntax | Checks security and compliance |
| Detects configuration errors | Detects security risks |
| No security analysis | Security-focused scanning |
| Built into Terraform | Separate security tool |

They complement each other and are often used together.

---

# Checkov vs tfsec

| Checkov | tfsec |
|----------|--------|
| Supports many IaC technologies | Primarily focused on Terraform |
| Security + compliance checks | Terraform security scanning |
| Broad DevSecOps platform | Lightweight Terraform scanner |

Both are excellent tools and are commonly used in DevSecOps workflows.

---

# Easy Way to Remember

Think of airport security.

```
Passenger

↓

Security Check

↓

Safe Flight
```

Terraform works similarly.

```
Terraform Code

↓

Checkov

↓

Secure Deployment
```

Checkov ensures your infrastructure passes security checks before deployment.

---

# Best Practices

- Run Checkov before every deployment.
- Integrate Checkov into CI/CD pipelines.
- Review every failed check instead of ignoring them.
- Keep Checkov updated to receive new security rules.
- Combine Checkov with `terraform validate`, `terraform fmt`, and tools like TFLint for comprehensive quality checks.
- Apply the principle of least privilege to cloud resources.

---

# Common Mistakes

❌ Ignoring failed security checks.

❌ Disabling important checks without understanding the impact.

❌ Running Checkov only before production releases instead of throughout development.

❌ Assuming `terraform validate` performs security analysis.

❌ Not updating Checkov to the latest version.

---

# Interview Questions

### What is Checkov?

Checkov is an Infrastructure as Code security and compliance scanner that detects misconfigurations before infrastructure is deployed.

---

### Who develops Checkov?

Bridgecrew, now part of **Palo Alto Networks**.

---

### Which command scans the current directory?

```bash
checkov -d .
```

---

### Does Checkov only support Terraform?

No. It supports Terraform, Kubernetes, Docker, CloudFormation, Azure ARM, Bicep, GitHub Actions, Helm, and many other Infrastructure as Code technologies.

---

### Can Checkov be integrated into CI/CD pipelines?

Yes. It is commonly integrated into GitHub Actions, GitLab CI, Jenkins, Azure DevOps, and other CI/CD platforms.

---

### What is the difference between `terraform validate` and Checkov?

`terraform validate` checks configuration syntax and structure, while Checkov scans for security vulnerabilities, compliance issues, and infrastructure best practice violations.

---

# Summary

Checkov is one of the most widely used Infrastructure as Code security scanners in DevSecOps.

Key concepts include:

- Infrastructure as Code scanning
- Security analysis
- Compliance validation
- Misconfiguration detection
- Multi-platform support
- CI/CD integration
- GitHub Actions
- DevSecOps

Using Checkov alongside Terraform's built-in validation tools helps create secure, compliant, and production-ready cloud infrastructure.