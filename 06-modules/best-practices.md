# 📦 Terraform Module Best Practices

## 📖 Introduction

As Terraform projects grow, configurations can become difficult to manage if everything is placed in a single file.

**Modules** help organize infrastructure into reusable components, but to fully benefit from them, they should follow established best practices.

Well-designed modules are:

- Reusable
- Maintainable
- Scalable
- Secure
- Easy to understand
- Easy to test

Following these best practices makes Infrastructure as Code more reliable for both small and enterprise-scale deployments.

---

# Why Module Best Practices Matter

Without modules:

```
One Huge File

↓

Thousands of Lines

↓

Hard To Read

↓

Hard To Maintain
```

With well-designed modules:

```
Small Modules

↓

Reusable Components

↓

Cleaner Code

↓

Easy Maintenance
```

---

# 1. Keep Modules Small and Focused

A module should have **one clear responsibility**.

Good examples:

- VPC Module
- EC2 Module
- RDS Module
- IAM Module
- EKS Module

Avoid creating a module that provisions an entire application stack unless that is its explicit purpose.

---

# 2. Make Modules Reusable

Avoid hardcoding values.

❌ Bad:

```hcl
instance_type = "t2.micro"
```

✅ Good:

```hcl
instance_type = var.instance_type
```

This allows different environments to reuse the same module.

---

# 3. Use Variables

Accept inputs through variables instead of fixed values.

Example:

```hcl
variable "region" {

  type = string

}
```

Inside the module:

```hcl
provider "aws" {

  region = var.region

}
```

---

# 4. Expose Useful Outputs

Return important values using outputs.

Example:

```hcl
output "vpc_id" {

  value = aws_vpc.main.id

}
```

Other modules can then reference:

```hcl
module.network.vpc_id
```

---

# 5. Use Meaningful Names

Choose descriptive names.

Good:

```text
network

database

eks

web_server
```

Avoid:

```text
module1

test

abc

temp
```

---

# 6. Follow a Standard Module Structure

Example:

```text
network-module/

├── main.tf
├── variables.tf
├── outputs.tf
├── versions.tf
├── README.md
└── examples/
```

Keeping a consistent structure makes modules easier to understand and maintain.

---

# 7. Pin Provider Versions

Specify compatible provider versions.

Example:

```hcl
terraform {

  required_providers {

    aws = {

      source  = "hashicorp/aws"

      version = "~> 5.0"

    }

  }

}
```

This helps avoid unexpected breaking changes.

---

# 8. Use Version Control

Store modules in Git repositories.

Example:

```text
GitHub

↓

Terraform Module

↓

Version Tags

↓

Reuse
```

Versioning allows safe upgrades and rollbacks.

---

# 9. Avoid Duplicate Code

If the same configuration appears multiple times:

```
Copy

Paste

Copy

Paste
```

Create a reusable module instead.

---

# 10. Document Every Module

Every module should include a `README.md` describing:

- Purpose
- Inputs
- Outputs
- Requirements
- Example usage

Example:

```
Module

↓

README

↓

Easy To Use
```

---

# 11. Validate Input Variables

Define types and validation rules.

Example:

```hcl
variable "instance_count" {

  type = number

  validation {

    condition     = var.instance_count > 0

    error_message = "Instance count must be greater than zero."

  }

}
```

This prevents invalid input values.

---

# 12. Use Sensible Defaults

Provide defaults when appropriate.

Example:

```hcl
variable "instance_type" {

  default = "t2.micro"

}
```

Do **not** provide defaults for values that should always be supplied explicitly, such as production database passwords.

---

# 13. Keep Modules Cloud-Agnostic When Practical

Some modules can be designed to work across environments by avoiding provider-specific assumptions.

Example:

```
Application Module

↓

Different Providers

↓

Reusable Logic
```

However, infrastructure modules such as VPCs or Virtual Networks are naturally provider-specific.

---

# 14. Keep Providers in the Root Module

A common best practice is to configure providers in the **root module** and pass them to child modules when needed.

Root module:

```hcl
provider "aws" {

  region = "us-east-1"

}
```

Child module:

```hcl
module "network" {

  source = "./modules/network"

}
```

This simplifies provider management and supports provider aliases.

---

# 15. Test Modules Before Production

Always verify modules.

Run:

```bash
terraform fmt
```

```bash
terraform validate
```

```bash
terraform plan
```

You can also use tools such as:

- TFLint
- tfsec
- Checkov

---

# Recommended Module Workflow

```text
Create Module
        │
        ▼
Add Variables
        │
        ▼
Add Outputs
        │
        ▼
Write Documentation
        │
        ▼
Validate
        │
        ▼
Reuse Module
```

---

# Good Module Example

```
modules/

├── network/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── versions.tf
│   └── README.md
│
├── compute/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── README.md
│
└── database/
    ├── main.tf
    ├── variables.tf
    ├── outputs.tf
    └── README.md
```

---

# Easy Way to Remember

Think of LEGO blocks.

```
One Block

↓

Reusable

↓

Build Many Models
```

Terraform modules work the same way.

```
One Module

↓

Reusable

↓

Build Many Environments
```

---

# Best Practices Checklist

✅ One responsibility per module

✅ Reusable design

✅ Use variables

✅ Provide outputs

✅ Document everything

✅ Pin provider versions

✅ Validate inputs

✅ Use version control

✅ Keep providers in the root module

✅ Test before deployment

---

# Common Mistakes

❌ Creating one massive module for everything.

❌ Hardcoding regions, instance types, or resource names.

❌ Forgetting outputs.

❌ Not documenting module usage.

❌ Duplicating code instead of creating reusable modules.

❌ Embedding secrets directly in modules.

❌ Skipping validation and testing.

---

# Interview Questions

### Why should Terraform modules be reusable?

Reusable modules reduce duplication, simplify maintenance, and ensure consistency across environments.

---

### What files should every module typically contain?

- `main.tf`
- `variables.tf`
- `outputs.tf`
- `versions.tf`
- `README.md`

---

### Why should variables be used instead of hardcoded values?

Variables make modules flexible and reusable across different environments.

---

### Where should providers generally be configured?

In the **root module**, with child modules receiving provider configurations as needed.

---

### Why are outputs important?

Outputs expose useful resource information so that other modules or configurations can reference it.

---

### How do you verify a module before using it?

Run:

```bash
terraform fmt
terraform validate
terraform plan
```

Optionally use tools such as TFLint, tfsec, and Checkov for additional quality checks.

---

# Summary

Following Terraform module best practices leads to cleaner, reusable, and production-ready Infrastructure as Code.

Key concepts include:

- Single-purpose modules
- Reusability
- Variables
- Outputs
- Standard module structure
- Documentation
- Provider management
- Input validation
- Version control
- Testing

Applying these practices will help you build scalable Terraform projects that are easier to maintain, collaborate on, and extend over time.