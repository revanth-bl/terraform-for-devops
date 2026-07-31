# 🏗️ Terraform Module Structure

## 📖 Introduction

A **Terraform module** is a collection of Terraform configuration files that work together to provision a specific set of infrastructure resources.

A well-organized module structure makes your Infrastructure as Code:

- Easy to understand
- Easy to maintain
- Easy to reuse
- Easy to test
- Easy to scale

Following a consistent structure is a best practice for both personal and enterprise Terraform projects.

---

# Why Module Structure Matters

Without a proper structure:

```
Many Files

↓

Random Organization

↓

Hard To Understand

↓

Hard To Maintain
```

With a proper structure:

```
Organized Files

↓

Clear Purpose

↓

Easy To Reuse

↓

Easy To Maintain
```

---

# Standard Module Structure

A typical Terraform module looks like this:

```text
network-module/

├── main.tf
├── variables.tf
├── outputs.tf
├── versions.tf
├── README.md
├── examples/
│   └── basic/
│       └── main.tf
├── modules/
│   └── subnet/
└── tests/
```

Not every module needs every directory, but this layout scales well as modules grow.

---

# File Overview

| File | Purpose |
|------|----------|
| `main.tf` | Defines the resources |
| `variables.tf` | Declares input variables |
| `outputs.tf` | Defines output values |
| `versions.tf` | Specifies Terraform and provider versions |
| `README.md` | Documents module usage |
| `examples/` | Sample implementations |
| `modules/` | Child modules (optional) |
| `tests/` | Module tests (optional) |

---

# 1. main.tf

Contains the primary resource definitions.

Example:

```hcl
resource "aws_vpc" "main" {

  cidr_block = var.vpc_cidr

}
```

This file focuses on the infrastructure the module creates.

---

# 2. variables.tf

Defines the module's input variables.

Example:

```hcl
variable "vpc_cidr" {

  type = string

}
```

Variables make the module reusable across different environments.

---

# 3. outputs.tf

Exports useful information from the module.

Example:

```hcl
output "vpc_id" {

  value = aws_vpc.main.id

}
```

The root module can access:

```hcl
module.network.vpc_id
```

---

# 4. versions.tf

Defines required Terraform and provider versions.

Example:

```hcl
terraform {

  required_version = ">= 1.8.0"

  required_providers {

    aws = {

      source  = "hashicorp/aws"

      version = "~> 5.0"

    }

  }

}
```

Version constraints improve stability and reproducibility.

---

# 5. README.md

Every module should include documentation.

Typical sections:

- Purpose
- Requirements
- Inputs
- Outputs
- Example usage
- Notes
- Limitations

Good documentation makes modules easier for others to use.

---

# 6. examples/

Provides working examples of the module.

Example:

```text
examples/

└── basic/

    └── main.tf
```

Example usage:

```hcl
module "network" {

  source = "../../"

  vpc_cidr = "10.0.0.0/16"

}
```

Examples help users quickly understand how to use the module.

---

# 7. modules/

A module can contain child modules.

Example:

```text
modules/

├── subnet/

├── routing/

└── security/
```

This is useful when a module grows large and needs further organization.

---

# 8. tests/

Stores module tests.

Example:

```text
tests/

├── basic/

└── integration/
```

Tests help verify that modules behave correctly before deployment.

---

# Root Module vs Child Module

## Root Module

```
main.tf

↓

Calls Modules

↓

Deploys Infrastructure
```

Example:

```hcl
module "network" {

  source = "./modules/network"

}
```

---

## Child Module

```
Receives Variables

↓

Creates Resources

↓

Returns Outputs
```

---

# Example Project Structure

```text
terraform-project/

├── main.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
├── versions.tf
└── modules/
    ├── network/
    │   ├── main.tf
    │   ├── variables.tf
    │   ├── outputs.tf
    │   ├── versions.tf
    │   └── README.md
    ├── compute/
    │   ├── main.tf
    │   ├── variables.tf
    │   ├── outputs.tf
    │   └── README.md
    └── database/
        ├── main.tf
        ├── variables.tf
        ├── outputs.tf
        └── README.md
```

This structure is common in production Terraform projects.

---

# Module Workflow

```text
Write Module
        │
        ▼
Define Variables
        │
        ▼
Create Resources
        │
        ▼
Define Outputs
        │
        ▼
Document Module
        │
        ▼
Reuse Module
```

---

# Easy Way to Remember

Think of a smartphone.

```
Screen

Battery

Camera

Processor
```

Each component has a specific job.

Terraform modules work the same way.

```
main.tf

↓

variables.tf

↓

outputs.tf

↓

README.md
```

Each file has a clear responsibility.

---

# Best Practices

- Keep modules focused on a single purpose.
- Use descriptive variable and output names.
- Include a `README.md` in every module.
- Provide working examples.
- Pin Terraform and provider versions.
- Organize large modules into child modules when appropriate.
- Validate and test modules before production use.

---

# Common Mistakes

❌ Putting everything into `main.tf`.

❌ Hardcoding values instead of using variables.

❌ Forgetting outputs.

❌ Not documenting the module.

❌ Creating overly large modules with multiple unrelated responsibilities.

❌ Omitting version constraints.

---

# Interview Questions

### What is a Terraform module?

A Terraform module is a collection of configuration files that work together to provision a specific set of infrastructure resources.

---

### What is the purpose of `main.tf`?

It contains the primary resource definitions for the module.

---

### What does `variables.tf` contain?

Input variable declarations that make the module configurable and reusable.

---

### Why is `outputs.tf` important?

It exposes useful values that other modules or the root module can reference.

---

### What is the purpose of `versions.tf`?

It defines the required Terraform CLI version and provider versions.

---

### Why should modules include a `README.md`?

To document the module's purpose, requirements, inputs, outputs, and usage examples, making it easier for others to understand and use.

---

# Summary

A consistent module structure improves readability, maintainability, and reusability in Terraform projects.

Key components include:

- `main.tf`
- `variables.tf`
- `outputs.tf`
- `versions.tf`
- `README.md`
- `examples/`
- `modules/`
- `tests/`

Following a standard module structure helps you build scalable, well-organized, and production-ready Infrastructure as Code.