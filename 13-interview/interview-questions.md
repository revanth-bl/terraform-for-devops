# 🎯 Terraform Interview Questions & Answers

## 📖 Introduction

Terraform is one of the most widely used **Infrastructure as Code (IaC)** tools in modern DevOps and Cloud Engineering. Interviewers often focus on core concepts, workflows, state management, modules, providers, and real-world scenarios.

This guide contains commonly asked Terraform interview questions with concise answers, suitable for both freshers and experienced professionals.

---

# Beginner Level

## 1. What is Terraform?

Terraform is an **Infrastructure as Code (IaC)** tool developed by HashiCorp that allows you to provision and manage infrastructure using configuration files instead of manual processes.

---

## 2. What is Infrastructure as Code (IaC)?

Infrastructure as Code is the practice of managing and provisioning infrastructure using code instead of manual configuration.

Benefits:

- Automation
- Consistency
- Version control
- Faster deployments
- Reduced human errors

---

## 3. Who developed Terraform?

**HashiCorp**

---

## 4. Which language does Terraform use?

**HashiCorp Configuration Language (HCL)**

---

## 5. What file extension does Terraform use?

```text
.tf
```

---

## 6. What is a Provider?

A provider is a plugin that allows Terraform to communicate with cloud platforms and services.

Examples:

- AWS
- Azure
- Google Cloud
- Kubernetes
- Docker

---

## 7. What is a Resource?

A resource represents an infrastructure object managed by Terraform.

Example:

```hcl
resource "aws_instance" "web" {

}
```

---

## 8. What is a Data Source?

A data source reads information from existing infrastructure without creating or modifying it.

Example:

```hcl
data "aws_ami" "ubuntu" {

}
```

---

## 9. What is a Module?

A module is a reusable collection of Terraform configurations that helps reduce duplication and improve maintainability.

---

## 10. What is Terraform State?

Terraform State is a file that maps Terraform configuration to real infrastructure resources.

Default file:

```text
terraform.tfstate
```

---

# Terraform Workflow

## 11. What does `terraform init` do?

Initializes the working directory by downloading providers, modules, and configuring the backend.

---

## 12. What does `terraform fmt` do?

Formats Terraform code according to standard formatting rules.

---

## 13. What does `terraform validate` do?

Checks the syntax and validity of Terraform configuration files.

---

## 14. What does `terraform plan` do?

Shows a preview of infrastructure changes without applying them.

---

## 15. What does `terraform apply` do?

Creates, updates, or deletes infrastructure based on the execution plan.

---

## 16. What does `terraform destroy` do?

Deletes all resources managed by the Terraform configuration.

---

# Variables & Outputs

## 17. What are Variables?

Variables allow values to be reused and customized across environments.

Example:

```hcl
variable "aws_region" {}
```

---

## 18. What are Outputs?

Outputs display useful information after deployment.

Example:

```hcl
output "public_ip" {

  value = aws_instance.web.public_ip

}
```

---

## 19. What are Locals?

Locals define reusable values within a Terraform configuration.

Example:

```hcl
locals {

  project = "terraform"

}
```

---

# State Management

## 20. Why is the Terraform state file important?

It tracks infrastructure resources and allows Terraform to determine what changes are required.

---

## 21. Should `terraform.tfstate` be committed to Git?

No. State files may contain sensitive information and should be stored securely in a remote backend.

---

## 22. What is Remote State?

Remote state stores the Terraform state file in a shared backend such as:

- Amazon S3
- Azure Storage
- Google Cloud Storage
- Terraform Cloud

---

## 23. What is State Locking?

State locking prevents multiple users from modifying the same state file simultaneously, reducing the risk of corruption.

---

# Modules

## 24. Why use Modules?

Modules improve:

- Reusability
- Maintainability
- Consistency
- Scalability

---

## 25. What is the Terraform Registry?

The Terraform Registry is a public repository of reusable providers and modules maintained by HashiCorp and the community.

---

# Security

## 26. How should secrets be managed?

Use:

- AWS Secrets Manager
- Azure Key Vault
- Google Secret Manager
- Sensitive variables
- CI/CD secret stores

Never hardcode secrets in Terraform files.

---

## 27. Which tools scan Terraform code for security issues?

- Checkov
- tfsec

---

## 28. What is the Principle of Least Privilege?

Grant only the permissions required to perform a specific task.

---

# Workspaces

## 29. What are Terraform Workspaces?

Workspaces allow multiple state files within a single Terraform configuration, commonly used for development, testing, and production environments.

---

## 30. How do you list workspaces?

```bash
terraform workspace list
```

---

## 31. How do you switch workspaces?

```bash
terraform workspace select dev
```

---

# Best Practices

## 32. Why should `terraform plan` always be reviewed?

It helps identify unexpected changes before infrastructure is modified.

---

## 33. Why should variables be used instead of hardcoded values?

Variables improve flexibility, reusability, and maintainability.

---

## 34. Why should resources be tagged?

Tags help with:

- Cost allocation
- Organization
- Automation
- Resource management

---

## 35. Why should providers be version-pinned?

To ensure consistent behavior and avoid unexpected issues caused by provider updates.

---

# Scenario-Based Questions

## 36. A teammate manually changes an AWS resource. What happens?

Terraform detects the difference between the configuration and the actual infrastructure during `terraform plan`. This situation is known as **infrastructure drift**.

---

## 37. Two engineers run `terraform apply` at the same time. What prevents problems?

State locking prevents simultaneous modifications to the same state file.

---

## 38. Your deployment fails because of a syntax error. Which command could have caught it?

```bash
terraform validate
```

---

## 39. You accidentally deployed unnecessary resources. What should you do?

Review the plan to understand the changes, then remove the resources using:

```bash
terraform destroy
```

or modify the configuration and run:

```bash
terraform apply
```

---

## 40. A database password is stored directly in `main.tf`. Why is this a problem?

Hardcoded secrets can be exposed through source control, logs, or state files. Use sensitive variables or a secrets management solution instead.

---

# Advanced Questions

## 41. What is Infrastructure Drift?

Infrastructure drift occurs when resources are modified outside of Terraform, causing the actual infrastructure to differ from the Terraform configuration or state.

---

## 42. What is the difference between `count` and `for_each`?

- `count` creates resources based on a numeric value.
- `for_each` creates resources from a map or set, providing stable resource addressing.

---

## 43. What is the purpose of `depends_on`?

It explicitly defines dependencies between resources when Terraform cannot automatically determine the correct creation order.

---

## 44. Why are remote backends recommended?

They provide centralized state storage, state locking, collaboration, and improved security.

---

## 45. How do you upgrade providers?

```bash
terraform init -upgrade
```

---

# Quick Revision Table

| Question | Answer |
|----------|--------|
| IaC Tool | Terraform |
| Developed By | HashiCorp |
| Language | HCL |
| Configuration Files | `.tf` |
| State File | `terraform.tfstate` |
| Initialize | `terraform init` |
| Format | `terraform fmt` |
| Validate | `terraform validate` |
| Preview Changes | `terraform plan` |
| Deploy | `terraform apply` |
| Destroy | `terraform destroy` |
| Reusable Code | Modules |
| Existing Resources | Data Sources |
| Cloud Plugin | Provider |
| Security Scanners | Checkov, tfsec |
| State Storage | Remote Backend |

---

# Tips for Interviews

- Understand the complete Terraform workflow.
- Practice writing simple Terraform configurations.
- Know the purpose of providers, resources, variables, outputs, and modules.
- Be familiar with state management and remote backends.
- Learn common security and best practices.
- Be prepared to explain real-world Terraform projects you have built.

---

# Summary

Terraform interviews typically focus on Infrastructure as Code concepts, the Terraform workflow, state management, modules, providers, security, and real-world deployment scenarios.

Master these topics:

- Infrastructure as Code (IaC)
- Providers
- Resources
- Variables
- Outputs
- Modules
- State Management
- Remote Backends
- Workspaces
- Security
- Best Practices
- Terraform Commands

A solid understanding of these concepts will prepare you for most Terraform interviews, from entry-level to intermediate DevOps and Cloud Engineering roles.