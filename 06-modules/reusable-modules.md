# ♻️ Reusable Terraform Modules

## 📖 Introduction

One of the biggest advantages of Terraform is the ability to create **reusable modules**.

Instead of writing the same infrastructure code multiple times, you can write it **once** and use it in multiple projects, environments, or cloud accounts.

Reusable modules help make Infrastructure as Code:

- Consistent
- Scalable
- Maintainable
- Easy to update
- Less error-prone

---

# What is a Reusable Module?

A reusable module is a Terraform module that can be used multiple times with different input values.

Instead of hardcoding values:

```hcl
instance_type = "t2.micro"
```

Use variables:

```hcl
instance_type = var.instance_type
```

Now the same module can be used for:

- Development
- Testing
- Staging
- Production

---

# Why Reuse Modules?

Without reusable modules:

```
Project A

↓

Copy Code

↓

Project B

↓

Copy Code

↓

Project C
```

Problems:

- Duplicate code
- Difficult maintenance
- Inconsistent infrastructure
- Higher chance of mistakes

---

With reusable modules:

```
One Module

↓

Many Projects

↓

Consistent Infrastructure
```

---

# Module Reusability Workflow

```text
Write Module
        │
        ▼
Use Variables
        │
        ▼
Create Outputs
        │
        ▼
Reuse Module
        │
        ▼
Deploy Anywhere
```

---

# Example Module

Directory:

```text
modules/

└── ec2/

    ├── main.tf

    ├── variables.tf

    └── outputs.tf
```

---

## variables.tf

```hcl
variable "instance_type" {

  type = string

}

variable "instance_name" {

  type = string

}
```

---

## main.tf

```hcl
resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = var.instance_type

  tags = {

    Name = var.instance_name

  }

}
```

---

## outputs.tf

```hcl
output "instance_id" {

  value = aws_instance.web.id

}
```

The module is now reusable.

---

# Using the Module

Development:

```hcl
module "dev_server" {

  source = "./modules/ec2"

  instance_type = "t2.micro"

  instance_name = "dev-server"

}
```

---

Production:

```hcl
module "prod_server" {

  source = "./modules/ec2"

  instance_type = "t3.large"

  instance_name = "prod-server"

}
```

The same module provisions different infrastructure using different input values.

---

# Reusing Modules Multiple Times

Example:

```hcl
module "frontend" {

  source = "./modules/ec2"

  instance_type = "t2.micro"

  instance_name = "frontend"

}

module "backend" {

  source = "./modules/ec2"

  instance_type = "t3.medium"

  instance_name = "backend"

}
```

One module creates two different EC2 instances.

---

# Reusing Modules Across Projects

```
Module Repository

↓

Project A

↓

Project B

↓

Project C
```

The same module can be shared across multiple repositories.

---

# Local Reusable Modules

Example:

```hcl
module "network" {

  source = "./modules/network"

}
```

Terraform loads the module from the local directory.

---

# Registry Modules

Modules can also come from the Terraform Registry.

Example:

```hcl
module "vpc" {

  source = "terraform-aws-modules/vpc/aws"

  version = "5.1.0"

}
```

Terraform downloads the module during:

```bash
terraform init
```

---

# Parameterizing Modules

Instead of:

```hcl
cidr_block = "10.0.0.0/16"
```

Use:

```hcl
cidr_block = var.vpc_cidr
```

Now each environment can use a different CIDR block.

Example:

Development:

```
10.0.0.0/16
```

Production:

```
172.16.0.0/16
```

---

# Module Inputs and Outputs

```
Variables

↓

Module

↓

Outputs
```

Example:

Input:

```hcl
instance_type = "t2.micro"
```

Output:

```hcl
module.server.instance_id
```

This makes modules flexible and easy to integrate.

---

# Reusable Module Architecture

```text
Root Module
      │
      ├── Network Module
      │
      ├── Compute Module
      │
      ├── Database Module
      │
      └── Monitoring Module
```

Each module performs one specific task and can be reused independently.

---

# Benefits of Reusable Modules

- Less code duplication
- Easier maintenance
- Consistent infrastructure
- Faster deployments
- Simplified testing
- Better collaboration
- Easier upgrades

---

# Easy Way to Remember

Think of reusable modules like LEGO bricks.

```
One LEGO Brick

↓

Build House

↓

Build Car

↓

Build Castle
```

Terraform modules work the same way.

```
One Module

↓

Many Projects

↓

Many Environments
```

---

# Best Practices

- Design modules for a single responsibility.
- Use variables instead of hardcoded values.
- Return useful outputs.
- Keep modules independent where possible.
- Document module inputs and outputs.
- Pin module versions when using remote modules.
- Store reusable modules in version control.

---

# Common Mistakes

❌ Hardcoding instance types, regions, or resource names.

❌ Creating one large module that performs unrelated tasks.

❌ Forgetting to expose outputs.

❌ Copying infrastructure code instead of creating reusable modules.

❌ Not documenting module usage.

❌ Not versioning shared modules.

---

# Interview Questions

### What is a reusable Terraform module?

A reusable module is a Terraform module designed to be used multiple times with different input values.

---

### Why should modules use variables?

Variables allow the same module to be reused across different environments without changing the module code.

---

### What is the benefit of outputs?

Outputs expose important resource information so other modules or the root module can reference it.

---

### Can the same module be used multiple times?

Yes. A module can be instantiated multiple times with different input values.

---

### Can reusable modules be shared across projects?

Yes. Modules can be stored locally, in Git repositories, or in the Terraform Registry for reuse across multiple projects.

---

### What is the biggest advantage of reusable modules?

They reduce code duplication while improving consistency, maintainability, and scalability.

---

# Summary

Reusable modules are a core feature of Terraform that promote clean, modular, and scalable Infrastructure as Code.

Key concepts include:

- Variables
- Outputs
- Module reuse
- Parameterization
- Local modules
- Registry modules
- Shared module repositories

Building reusable modules helps teams standardize infrastructure, reduce duplication, and create production-ready Terraform configurations that are easier to maintain over time.