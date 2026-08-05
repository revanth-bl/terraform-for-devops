# ⚠️ Common Terraform Mistakes

## 📖 Introduction

Terraform simplifies infrastructure management, but even small mistakes can lead to failed deployments, security vulnerabilities, unexpected costs, or infrastructure downtime.

Understanding common mistakes helps you write cleaner, safer, and more maintainable Infrastructure as Code (IaC).

This guide covers the most frequent Terraform mistakes, why they happen, and how to avoid them.

---

# Why Avoid These Mistakes?

Poor Terraform practices can result in:

- Failed deployments
- Security vulnerabilities
- Infrastructure drift
- Team collaboration issues
- Unexpected cloud costs
- Difficult troubleshooting

Following best practices improves reliability and reduces operational risk.

---

# 1. Hardcoding Values

### ❌ Bad

```hcl
provider "aws" {

  region = "us-east-1"

}
```

### ✅ Good

```hcl
provider "aws" {

  region = var.aws_region

}
```

Use variables instead of hardcoded values to make configurations reusable across environments.

---

# 2. Hardcoding Secrets

### ❌ Bad

```hcl
password = "MyPassword123"
```

### ✅ Good

```hcl
password = var.db_password
```

Store secrets securely using:

- Sensitive variables
- AWS Secrets Manager
- Azure Key Vault
- Google Secret Manager
- Environment variables

Never commit secrets to Git.

---

# 3. Skipping terraform fmt

Messy code is harder to read and maintain.

Always run:

```bash
terraform fmt
```

before committing code.

---

# 4. Skipping terraform validate

Many deployment failures occur because configurations were never validated.

Always run:

```bash
terraform validate
```

before planning or applying infrastructure.

---

# 5. Skipping terraform plan

Never deploy infrastructure blindly.

Always review:

```bash
terraform plan
```

This helps identify unexpected changes before they occur.

---

# 6. Ignoring Terraform State

The state file tracks existing infrastructure.

Never:

- Delete it accidentally
- Modify it manually
- Commit sensitive state files to public repositories

Use remote backends for team projects.

---

# 7. Using Local State for Teams

### ❌ Bad

```
terraform.tfstate

stored on one developer's laptop
```

### ✅ Good

```
Remote Backend

↓

Amazon S3

↓

State Locking

↓

Team Collaboration
```

Remote state improves reliability and collaboration.

---

# 8. Not Using Version Control

Terraform code should always be stored in Git.

Benefits:

- Change history
- Collaboration
- Rollback capability
- Code reviews

---

# 9. Poor Project Structure

### ❌ Bad

```
Everything in one file

↓

main.tf
```

### ✅ Good

```
provider.tf

variables.tf

outputs.tf

network.tf

compute.tf

database.tf
```

Organize configurations into logical files and modules.

---

# 10. Not Using Modules

Copying and pasting resources creates duplication.

Instead:

```
Reusable Module

↓

Multiple Deployments
```

Modules improve maintainability and consistency.

---

# 11. Opening Security Groups to Everyone

### ❌ Bad

```text
0.0.0.0/0
```

for SSH or database access.

### ✅ Good

Allow only trusted IP addresses or application security groups.

Always follow the **principle of least privilege**.

---

# 12. Ignoring Resource Tags

Every cloud resource should have meaningful tags.

Example:

```hcl
tags = {

  Name = "WebServer"

  Environment = "Production"

  Owner = "DevOps"

}
```

Tags help with:

- Cost tracking
- Resource management
- Automation
- Organization

---

# 13. Forgetting Resource Cleanup

Leaving unused infrastructure running increases cloud costs.

After testing:

```bash
terraform destroy
```

Remove resources that are no longer needed.

---

# 14. Manually Changing Infrastructure

Avoid making manual changes through the cloud console.

Example:

```
Terraform

↓

AWS Console

↓

Manual Changes

↓

Infrastructure Drift
```

Manage infrastructure through Terraform whenever possible.

---

# 15. Ignoring Security Scans

Run security tools regularly.

Examples:

```bash
checkov -d .
```

```bash
tfsec .
```

These tools detect security vulnerabilities before deployment.

---

# 16. Not Pinning Provider Versions

### ❌ Bad

```hcl
version = "latest"
```

### ✅ Good

```hcl
version = "~> 5.0"
```

Version constraints improve consistency and reduce unexpected breaking changes.

---

# 17. Not Reviewing CI/CD Pipelines

Always include:

```text
terraform fmt

↓

terraform validate

↓

TFLint

↓

Checkov / tfsec

↓

terraform plan

↓

terraform apply
```

Automation improves deployment quality.

---

# Common Mistake Workflow

```text
Write Terraform
        │
        ▼
Skip Validation
        │
        ▼
terraform apply
        │
        ▼
Deployment Failure
```

---

# Recommended Workflow

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
Checkov / tfsec
        │
        ▼
terraform plan
        │
        ▼
terraform apply
```

---

# Summary Table

| Mistake | Best Practice |
|----------|---------------|
| Hardcoded values | Use variables |
| Hardcoded secrets | Use secret management |
| Local state | Use remote backends |
| No formatting | Run `terraform fmt` |
| No validation | Run `terraform validate` |
| No planning | Review `terraform plan` |
| Manual console changes | Manage infrastructure through Terraform |
| No modules | Build reusable modules |
| Weak security groups | Apply least privilege |
| Untagged resources | Use consistent tagging |
| No security scanning | Run Checkov and tfsec |
| No cleanup | Use `terraform destroy` |

---

# Best Practices Checklist

✅ Use variables instead of hardcoded values.

✅ Store secrets securely.

✅ Run `terraform fmt`.

✅ Run `terraform validate`.

✅ Review `terraform plan`.

✅ Store state remotely.

✅ Use reusable modules.

✅ Tag every resource.

✅ Follow the principle of least privilege.

✅ Scan Terraform code using Checkov and tfsec.

✅ Automate deployments with CI/CD.

✅ Remove unused infrastructure.

---

# Interview Questions

### Why shouldn't secrets be hardcoded in Terraform?

Because they can be exposed through source control, logs, or state files. Secrets should be stored using secure secret management solutions or sensitive variables.

---

### Why is remote state recommended?

It enables collaboration, state locking, centralized storage, and reduces the risk of state corruption.

---

### What causes infrastructure drift?

Manual changes made directly in the cloud provider instead of through Terraform.

---

### Which command formats Terraform code?

```bash
terraform fmt
```

---

### Which command validates Terraform configuration?

```bash
terraform validate
```

---

### Which command removes infrastructure?

```bash
terraform destroy
```

---

# Summary

Avoiding common Terraform mistakes leads to more secure, reliable, and maintainable Infrastructure as Code.

Key concepts include:

- Variables
- Secret management
- Remote state
- Modules
- Security Groups
- Resource tagging
- CI/CD
- Infrastructure drift
- `terraform fmt`
- `terraform validate`
- `terraform plan`
- `terraform destroy`

By following these best practices, you can build production-ready Terraform projects that are easier to manage, collaborate on, and scale.