# 🔒 Terraform Security Best Practices

## 📖 Introduction

Security is one of the most important aspects of Infrastructure as Code (IaC). A misconfigured Terraform project can expose sensitive data, create vulnerable cloud resources, or grant excessive permissions.

Terraform itself is not a security tool—it is an automation tool. It is your responsibility to design secure infrastructure and follow cloud security best practices.

This guide covers the most important Terraform security practices for production environments.

---

# Why Security Matters

Poor security practices can lead to:

- Data breaches
- Unauthorized access
- Infrastructure compromise
- Credential leaks
- Compliance violations
- Financial loss

A secure Terraform workflow reduces these risks and helps protect cloud resources.

---

# Security Workflow

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
Security Scan
        │
        ▼
terraform plan
        │
        ▼
terraform apply
```

---

# 1. Never Hardcode Secrets

### ❌ Bad

```hcl
password = "MyPassword123"

access_key = "AKIAxxxxxxxx"
```

### ✅ Good

```hcl
password = var.db_password
```

Better options include:

- AWS Secrets Manager
- Azure Key Vault
- Google Secret Manager
- Environment variables
- CI/CD secret stores

Never commit secrets to Git.

---

# 2. Protect Terraform State

The state file may contain:

- Passwords
- Access keys
- Database endpoints
- Infrastructure metadata

Do **not** commit:

```text
terraform.tfstate
```

Use secure remote backends such as:

- Amazon S3 with encryption
- Azure Storage
- Google Cloud Storage
- Terraform Cloud

Enable encryption and access controls on the backend.

---

# 3. Use the Principle of Least Privilege

Grant only the permissions required.

### ❌ Bad

```text
AdministratorAccess
```

### ✅ Good

```text
EC2 Read Only

S3 Read/Write

CloudWatch Logs
```

Limit IAM permissions for users, roles, and service accounts.

---

# 4. Restrict Network Access

Avoid exposing resources unnecessarily.

### ❌ Bad

```text
SSH

0.0.0.0/0
```

### ✅ Good

```text
SSH

203.0.113.10/32
```

Allow only trusted IP addresses or security groups.

---

# 5. Encrypt Data

Enable encryption wherever possible.

Examples:

- Amazon EBS encryption
- Amazon S3 server-side encryption
- Amazon RDS encryption
- Azure Disk Encryption
- Google Cloud encryption

Encryption protects data at rest.

---

# 6. Enable Secure Communication

Always use encrypted protocols such as:

- HTTPS
- TLS
- SSH

Avoid unsecured protocols such as:

- HTTP (where HTTPS is appropriate)
- Telnet
- FTP

---

# 7. Mark Sensitive Variables

Example:

```hcl
variable "db_password" {

  sensitive = true

}
```

Sensitive variables reduce accidental exposure in Terraform output.

---

# 8. Scan Terraform Code

Run security scanners before deployment.

Examples:

```bash
tfsec .
```

```bash
checkov -d .
```

These tools detect common security misconfigurations.

---

# 9. Keep Providers Updated

Example:

```hcl
required_providers {

  aws = {

    source = "hashicorp/aws"

    version = "~> 5.0"

  }

}
```

Use supported provider versions to receive bug fixes and security updates.

---

# 10. Enable Logging and Monitoring

Use cloud monitoring services such as:

- Amazon CloudWatch
- AWS CloudTrail
- Azure Monitor
- Google Cloud Monitoring

Logging helps detect suspicious activity and troubleshoot issues.

---

# 11. Protect Remote State Access

Restrict who can access Terraform state.

Recommended controls:

- IAM policies
- Multi-Factor Authentication (MFA)
- Encryption
- State locking
- Versioning

Only authorized users should access the state backend.

---

# 12. Review Infrastructure Before Deployment

Always review planned changes.

```bash
terraform plan
```

This helps identify accidental or insecure modifications before they are applied.

---

# 13. Use Version Control Securely

Store Terraform code in Git.

Protect repositories by:

- Restricting access
- Enabling branch protection
- Reviewing pull requests
- Preventing secret commits
- Using CI/CD security checks

---

# 14. Avoid Manual Infrastructure Changes

Manual changes create infrastructure drift.

```
Terraform

↓

Cloud Console

↓

Manual Changes

↓

State Drift
```

Manage infrastructure through Terraform whenever possible.

---

# Security Checklist

| Security Area | Best Practice |
|--------------|---------------|
| Secrets | Use secret managers or CI/CD secrets |
| State | Encrypt and store remotely |
| IAM | Apply least privilege |
| Network | Restrict inbound access |
| Encryption | Enable encryption at rest |
| Providers | Keep versions updated |
| Validation | Run `terraform validate` |
| Security Scans | Run Checkov and tfsec |
| Monitoring | Enable logging and alerts |
| Version Control | Protect repositories |

---

# Common Security Mistakes

❌ Hardcoding credentials.

❌ Committing `terraform.tfstate` to Git.

❌ Using overly permissive IAM policies.

❌ Allowing unrestricted SSH access.

❌ Ignoring security scan results.

❌ Using outdated provider versions.

❌ Making manual cloud console changes.

❌ Disabling encryption.

---

# Easy Way to Remember

Think of protecting a house.

```
Lock Doors

↓

Install Cameras

↓

Give Keys Only to Trusted People

↓

Monitor Activity
```

Terraform security follows the same idea:

```
Protect Secrets

↓

Restrict Access

↓

Encrypt Data

↓

Monitor Infrastructure
```

---

# Interview Questions

### Why shouldn't secrets be hardcoded in Terraform?

Because they can be exposed through source control, logs, or state files. Secrets should be stored securely using secret management solutions or sensitive variables.

---

### Why is Terraform state considered sensitive?

It may contain passwords, access keys, infrastructure metadata, and other confidential information.

---

### Which Terraform tools perform security scanning?

- Checkov
- tfsec

---

### Why is the principle of least privilege important?

It minimizes the impact of compromised accounts by granting only the permissions required to perform specific tasks.

---

### Which command previews infrastructure changes?

```bash
terraform plan
```

---

### Which command validates Terraform configuration?

```bash
terraform validate
```

---

# Summary

Security should be integrated into every stage of the Terraform workflow. By following security best practices, you can build infrastructure that is resilient, maintainable, and aligned with modern cloud security standards.

Key concepts include:

- Secret management
- Remote state security
- IAM least privilege
- Encryption
- Secure networking
- Provider updates
- Security scanning
- Monitoring
- `terraform validate`
- `terraform plan`
- Checkov
- tfsec

Applying these practices helps create secure, production-ready Infrastructure as Code that protects both your cloud resources and your organization.