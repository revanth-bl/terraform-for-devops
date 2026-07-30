# 📦 Terraform Locals

## 📖 Introduction

**Locals** (or **Local Values**) allow you to assign a value to a name and reuse it throughout your Terraform configuration.

Think of locals as **variables that exist only within the current Terraform module**. Unlike input variables, local values cannot be changed by users—they are calculated or defined inside the configuration.

Locals help reduce repetition, improve readability, and make Terraform code easier to maintain.

---

# Why Use Locals?

Without locals:

```hcl
resource "aws_instance" "web" {
  tags = {
    Name        = "dev-web-server"
    Environment = "dev"
  }
}

resource "aws_s3_bucket" "logs" {
  bucket = "dev-logs-bucket"

  tags = {
    Environment = "dev"
  }
}
```

The value `"dev"` is repeated multiple times.

Using locals:

```hcl
locals {
  environment = "dev"
}

resource "aws_instance" "web" {
  tags = {
    Name        = "${local.environment}-web-server"
    Environment = local.environment
  }
}

resource "aws_s3_bucket" "logs" {
  bucket = "${local.environment}-logs-bucket"

  tags = {
    Environment = local.environment
  }
}
```

Now the value only needs to be changed in one place.

---

# Syntax

```hcl
locals {
  name = value
}
```

Example:

```hcl
locals {
  project = "terraform-demo"
}
```

Use it with:

```hcl
local.project
```

---

# Single Local Value

```hcl
locals {
  instance_type = "t2.micro"
}
```

Using the local:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-123456789"
  instance_type = local.instance_type
}
```

---

# Multiple Local Values

```hcl
locals {
  environment = "dev"
  project     = "terraform-demo"
  owner       = "DevOps Team"
}
```

Access them as:

```hcl
local.environment
local.project
local.owner
```

---

# Locals Using Variables

Locals can use input variables.

```hcl
variable "environment" {
  default = "dev"
}

locals {
  bucket_name = "${var.environment}-logs-bucket"
}
```

Result:

```
dev-logs-bucket
```

---

# Locals with Expressions

```hcl
locals {
  instance_type = var.environment == "prod" ? "t3.medium" : "t2.micro"
}
```

Result:

```
Production → t3.medium

Development → t2.micro
```

---

# Locals with Functions

```hcl
locals {
  project_name = upper(var.project)
}
```

If:

```hcl
project = "cloud"
```

Result:

```
CLOUD
```

---

# Local Maps

```hcl
locals {
  instance_types = {
    dev  = "t2.micro"
    test = "t3.small"
    prod = "t3.medium"
  }
}
```

Access:

```hcl
local.instance_types["prod"]
```

Result:

```
t3.medium
```

---

# Local Lists

```hcl
locals {
  availability_zones = [
    "us-east-1a",
    "us-east-1b",
    "us-east-1c"
  ]
}
```

Access:

```hcl
local.availability_zones[0]
```

Result:

```
us-east-1a
```

---

# Practical Example

```hcl
variable "environment" {
  default = "dev"
}

variable "project" {
  default = "cloud-app"
}

locals {

  name_prefix = "${var.project}-${var.environment}"

  common_tags = {
    Project     = var.project
    Environment = var.environment
    ManagedBy   = "Terraform"
  }

}

resource "aws_instance" "web" {

  ami           = "ami-123456789"

  instance_type = "t2.micro"

  tags = merge(
    local.common_tags,
    {
      Name = "${local.name_prefix}-server"
    }
  )

}
```

Result:

```
Project = cloud-app

Environment = dev

ManagedBy = Terraform

Name = cloud-app-dev-server
```

---

# Locals vs Variables

| Variables | Locals |
|-----------|--------|
| User input | Internal values |
| Can be overridden | Cannot be overridden |
| Declared with `variable` | Declared with `locals` |
| Used for configuration | Used to simplify configuration |
| Passed into modules | Available only within the current module |

---

# Workflow

```text
Variables
     │
     ▼
Local Values
     │
     ▼
Resources
     │
     ▼
Infrastructure Created
```

---

# Easy Way to Remember

Imagine you're writing a long document.

Without locals:

```
Company Name

Company Name

Company Name

Company Name
```

If the company name changes, you must edit it everywhere.

With locals:

```
Company = OpenAI

↓

Use Company throughout the document.
```

Change it once, and every reference updates automatically.

Terraform locals work exactly the same way.

---

# Best Practices

- Use locals to avoid repeating values.
- Store naming conventions in locals.
- Store common tags in locals.
- Use locals for calculated values.
- Keep local names meaningful and descriptive.

---

# Common Mistakes

❌ Using locals when an input variable is more appropriate.

❌ Giving locals unclear names.

❌ Creating unnecessary locals for values used only once.

❌ Placing complex business logic inside locals.

---

# Interview Questions

### What are Terraform locals?

Locals are named values defined within a Terraform module that can be reused throughout the configuration.

---

### What is the difference between variables and locals?

Variables accept user input and can be overridden, while locals are internal values defined within the configuration and cannot be overridden.

---

### How do you reference a local value?

```hcl
local.name
```

Example:

```hcl
local.environment
```

---

### Why are locals useful?

They reduce code duplication, improve readability, and make configurations easier to maintain.

---

### Can locals use variables and functions?

Yes. Local values can reference variables, other locals, functions, and expressions.

---

# Summary

Terraform locals provide reusable values within a module, helping simplify configurations and eliminate duplication.

They are commonly used for:

- Naming conventions
- Common tags
- Calculated values
- Environment-specific settings
- Reusable expressions

Using locals effectively leads to cleaner, more maintainable, and production-ready Terraform code.