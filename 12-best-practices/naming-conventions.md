# 🏷️ Terraform Naming Conventions

## 📖 Introduction

Consistent naming conventions make Terraform projects easier to read, maintain, and scale. Well-named resources help teams quickly understand what infrastructure does, reduce confusion, and improve collaboration.

A good naming strategy should be:

- Consistent
- Predictable
- Descriptive
- Scalable
- Easy to search

Whether you're managing 10 resources or 10,000, following naming conventions is an essential Terraform best practice.

---

# Why Naming Conventions Matter

Without naming standards:

```text
EC2

↓

server1

↓

server2

↓

test123

↓

abc
```

Problems:

- Difficult to identify resources
- Poor readability
- Harder troubleshooting
- Inconsistent infrastructure
- Difficult automation

---

With naming standards:

```text
dev-web-server

↓

prod-db-server

↓

eks-cluster

↓

vpc-main
```

Benefits:

- Easy identification
- Better organization
- Simpler maintenance
- Improved collaboration
- Cleaner Infrastructure as Code

---

# General Naming Rules

A good resource name should be:

- Lowercase
- Descriptive
- Consistent
- Easy to understand
- Free of unnecessary abbreviations

Example:

```text
production-web-server
```

Avoid:

```text
server123abc
```

---

# Use Hyphens for Cloud Resource Names

Many cloud resources use hyphens (`-`) in names.

Examples:

```text
dev-web-server

prod-rds

eks-cluster

terraform-vpc
```

---

# Use Underscores in Terraform Resource Labels

Terraform resource labels should use underscores (`_`).

Example:

```hcl
resource "aws_instance" "web_server" {

}
```

Instead of:

```hcl
resource "aws_instance" "web-server" {

}
```

Terraform labels are internal identifiers and commonly follow `snake_case`.

---

# Include the Environment

Include the deployment environment in resource names.

Examples:

```text
dev-web-server

test-web-server

staging-web-server

prod-web-server
```

This helps distinguish resources across environments.

---

# Include the Resource Type

Examples:

```text
prod-web-server

prod-rds

prod-vpc

prod-alb
```

The name should clearly indicate the purpose of the resource.

---

# Use Variables for Naming

Example:

```hcl
variable "environment" {

  default = "dev"

}
```

```hcl
locals {

  name = "${var.environment}-web-server"

}
```

Changing the environment automatically updates resource names.

---

# Use Locals for Reusable Names

Example:

```hcl
locals {

  project = "terraform"

  environment = "dev"

  common_name = "${local.project}-${local.environment}"

}
```

Example usage:

```hcl
tags = {

  Name = local.common_name

}
```

This improves consistency and reduces duplication.

---

# Naming Examples

| Resource | Example |
|----------|----------|
| VPC | `prod-vpc` |
| EC2 | `dev-web-server` |
| RDS | `prod-mysql-db` |
| S3 Bucket | `company-prod-backups` |
| Lambda | `image-resizer` |
| EKS Cluster | `production-eks` |
| IAM Role | `terraform-ec2-role` |
| Security Group | `web-security-group` |

---

# File Naming Conventions

Keep Terraform files organized.

Recommended structure:

```text
provider.tf

versions.tf

variables.tf

outputs.tf

main.tf

network.tf

compute.tf

database.tf

locals.tf
```

Avoid:

```text
abc.tf

test.tf

newfile.tf

temp.tf
```

---

# Module Naming

Good examples:

```text
modules/

├── vpc/

├── ec2/

├── rds/

├── eks/

└── lambda/
```

Avoid:

```text
module1

module2

projectmodule
```

Modules should clearly describe their purpose.

---

# Variable Naming

Good:

```hcl
instance_type

aws_region

vpc_cidr

db_username
```

Avoid:

```hcl
x

a

var1
```

Variable names should be descriptive and use `snake_case`.

---

# Output Naming

Good:

```hcl
instance_id

public_ip

vpc_id

database_endpoint
```

Avoid:

```hcl
id

output1

data
```

Outputs should clearly describe the value they return.

---

# Tagging Convention

Example:

```hcl
tags = {

  Name        = "prod-web-server"

  Environment = "Production"

  Project     = "Terraform"

  Owner       = "DevOps"

}
```

Consistent tagging improves resource management, automation, and cost reporting.

---

# Naming Workflow

```text
Project
      │
      ▼
Environment
      │
      ▼
Resource Type
      │
      ▼
Consistent Name
```

Example:

```text
terraform

↓

production

↓

web-server

↓

terraform-production-web-server
```

---

# Good vs Bad Naming

| Good | Bad |
|------|-----|
| `prod-web-server` | `server1` |
| `terraform-vpc` | `network` |
| `dev-rds` | `database123` |
| `eks-cluster` | `cluster-new` |
| `public-subnet-1` | `subnet1` |

---

# Easy Way to Remember

Think of organizing files on your computer.

Bad:

```text
file1

file2

newfile

test
```

Good:

```text
resume.pdf

project-report.docx

budget.xlsx
```

Terraform resources should be named with the same level of clarity.

---

# Best Practices

- Use lowercase names.
- Use `snake_case` for Terraform resource labels, variables, locals, and outputs.
- Use hyphens for cloud resource names where appropriate.
- Include the environment in names.
- Keep names descriptive and consistent.
- Use variables and locals to build names.
- Follow the same naming convention across the entire project.
- Tag all resources consistently.

---

# Common Mistakes

❌ Using random or meaningless names.

❌ Mixing uppercase and lowercase naming styles.

❌ Using different naming patterns within the same project.

❌ Hardcoding environment names throughout the code.

❌ Using abbreviations that are difficult to understand.

❌ Not tagging resources.

---

# Interview Questions

### Why are naming conventions important in Terraform?

They improve readability, maintainability, collaboration, and automation.

---

### Should Terraform resource labels use hyphens or underscores?

Resource labels typically use **underscores (`snake_case`)**.

Example:

```hcl
resource "aws_instance" "web_server" {}
```

---

### Should cloud resource names use hyphens?

Yes. Many cloud providers commonly use hyphen-separated names.

Example:

```text
prod-web-server
```

---

### Why should environment names be included?

They help distinguish resources across development, testing, staging, and production environments.

---

### Why use locals for naming?

Locals centralize naming logic, reduce duplication, and make naming consistent throughout the project.

---

# Summary

Consistent naming conventions make Terraform configurations easier to understand, maintain, and scale.

Key concepts include:

- Descriptive naming
- `snake_case`
- Hyphen-separated cloud resource names
- Variables
- Locals
- Environment naming
- Module naming
- Output naming
- Resource tagging

Following a consistent naming strategy results in cleaner, more maintainable, and production-ready Terraform projects.