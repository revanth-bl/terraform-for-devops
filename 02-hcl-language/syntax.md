# 📝 Terraform HCL Syntax

## 📖 Introduction

Terraform uses **HashiCorp Configuration Language (HCL)** to define infrastructure.

HCL is designed to be:

- Human-readable
- Easy to write
- Easy to learn
- Machine-readable

Instead of writing long JSON or XML files, Terraform uses HCL to describe the **desired state** of your infrastructure.

Example:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-123456789"
  instance_type = "t2.micro"
}
```

---

# What is HCL?

**HCL (HashiCorp Configuration Language)** is a declarative configuration language created by HashiCorp.

It is used by:

- Terraform
- Packer
- Consul
- Nomad
- Vault (some configurations)

HCL allows you to describe **what** infrastructure you want, not **how** to build it.

---

# Declarative vs Imperative

### Declarative (Terraform)

You describe the desired result.

```hcl
resource "aws_instance" "web" {
  instance_type = "t2.micro"
}
```

Terraform decides how to create it.

---

### Imperative

You write every step.

```text
Create VM

↓

Configure Storage

↓

Attach Network

↓

Start VM
```

Terraform hides these implementation details.

---

# Basic HCL Structure

Every Terraform configuration is made up of **blocks**.

Example:

```hcl
resource "aws_instance" "web" {

  ami           = "ami-123456789"

  instance_type = "t2.micro"

}
```

Structure:

```
Block Type

↓

Labels

↓

Arguments

↓

Values
```

---

# HCL Blocks

Blocks are the main building blocks of Terraform.

General syntax:

```hcl
block_type "label1" "label2" {

}
```

Example:

```hcl
resource "aws_instance" "web" {

}
```

Common block types:

- provider
- resource
- variable
- output
- locals
- module
- data
- terraform

---

# Arguments

Arguments assign values inside a block.

Example:

```hcl
instance_type = "t2.micro"
```

General syntax:

```hcl
argument = value
```

Example:

```hcl
region = "us-east-1"
```

---

# Values

Values can be:

### String

```hcl
"Terraform"
```

---

### Number

```hcl
10
```

---

### Boolean

```hcl
true
```

---

### List

```hcl
["dev", "test", "prod"]
```

---

### Map

```hcl
{
  Name = "Web"
}
```

---

# Resource Block Example

```hcl
resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = "t2.micro"

  tags = {
    Name = "Web Server"
  }

}
```

---

# Provider Block

Defines which cloud provider Terraform will use.

```hcl
provider "aws" {

  region = "us-east-1"

}
```

---

# Variable Block

```hcl
variable "instance_type" {

  default = "t2.micro"

}
```

Use:

```hcl
var.instance_type
```

---

# Output Block

```hcl
output "public_ip" {

  value = aws_instance.web.public_ip

}
```

---

# Locals Block

```hcl
locals {

  environment = "dev"

}
```

Use:

```hcl
local.environment
```

---

# Data Block

Reads information from existing infrastructure.

```hcl
data "aws_ami" "ubuntu" {

  most_recent = true

}
```

---

# Module Block

Loads reusable Terraform modules.

```hcl
module "network" {

  source = "./modules/network"

}
```

---

# Comments

### Single-line comment

```hcl
# This is a comment
```

or

```hcl
// This is also a comment
```

---

### Multi-line comment

```hcl
/*
This is
a multi-line
comment.
*/
```

---

# Expressions

Terraform allows expressions inside HCL.

Example:

```hcl
instance_type = var.environment == "prod"
  ? "t3.medium"
  : "t2.micro"
```

Expressions make configurations dynamic.

---

# String Interpolation

Combine strings with variables.

```hcl
"${var.environment}-server"
```

Result:

```
dev-server
```

Modern Terraform also supports:

```hcl
"${var.project}-${var.environment}"
```

or simply:

```hcl
"${local.name}"
```

---

# File Naming Convention

Common Terraform files:

| File | Purpose |
|------|---------|
| `main.tf` | Main configuration |
| `providers.tf` | Provider configuration |
| `variables.tf` | Input variables |
| `outputs.tf` | Output values |
| `locals.tf` | Local values |
| `terraform.tfvars` | Variable values |

> **Note:** These filenames are conventions, not strict requirements. Terraform loads all `.tf` files in the working directory.

---

# Complete Example

```hcl
provider "aws" {

  region = "us-east-1"

}

variable "environment" {

  default = "dev"

}

locals {

  project = "terraform-demo"

}

resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = "t2.micro"

  tags = {

    Name = "${local.project}-${var.environment}"

  }

}

output "instance_ip" {

  value = aws_instance.web.public_ip

}
```

---

# Easy Way to Remember

Think of HCL like building with LEGO bricks.

```
Blocks

↓

Arguments

↓

Values

↓

Complete Infrastructure
```

Each Terraform configuration is built by combining different block types.

---

# Best Practices

- Use meaningful resource names.
- Keep formatting consistent (`terraform fmt`).
- Organize code into multiple `.tf` files.
- Use variables instead of hardcoded values.
- Add comments only where necessary.
- Follow a consistent naming convention.

---

# Common Mistakes

❌ Missing quotation marks around strings.

❌ Forgetting opening or closing braces `{}`.

❌ Hardcoding values that should be variables.

❌ Using inconsistent indentation.

❌ Creating one huge `main.tf` file instead of organizing code.

---

# Interview Questions

### What is HCL?

HashiCorp Configuration Language (HCL) is a declarative language used by Terraform to define infrastructure as code.

---

### Is HCL declarative or imperative?

HCL is **declarative**. You define the desired state, and Terraform determines how to achieve it.

---

### What are the main building blocks of HCL?

- Blocks
- Arguments
- Expressions
- Values

---

### What are the common Terraform block types?

- provider
- resource
- variable
- output
- locals
- module
- data
- terraform

---

### Can Terraform use multiple `.tf` files?

Yes. Terraform automatically loads all `.tf` files in the working directory.

---

# Summary

HCL is the language used by Terraform to describe infrastructure in a clear, readable, and declarative way.

Key concepts include:

- Blocks
- Arguments
- Values
- Comments
- Expressions
- Variables
- Resources
- Providers
- Modules

Understanding HCL syntax is the foundation for writing clean, reusable, and production-ready Terraform configurations.