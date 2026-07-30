# 📥 Terraform Variables

## 📖 Introduction

Variables allow you to make Terraform configurations **dynamic, reusable, and flexible**.

Instead of hardcoding values such as instance types, AWS regions, or environment names, you can define them as **input variables** and provide different values for different environments.

Without variables, every change requires modifying the Terraform code. With variables, the same configuration can be reused across development, testing, and production.

---

# Why Use Variables?

Without variables:

```hcl
resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = "t2.micro"

}
```

Changing the instance type requires editing the code.

Using variables:

```hcl
variable "instance_type" {

  default = "t2.micro"

}

resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = var.instance_type

}
```

Now only the variable value needs to change.

---

# Variable Block Syntax

```hcl
variable "variable_name" {

}
```

Example:

```hcl
variable "region" {

  default = "us-east-1"

}
```

Access the variable using:

```hcl
var.region
```

---

# Simple Variable

```hcl
variable "environment" {

  default = "dev"

}
```

Use it:

```hcl
resource "aws_s3_bucket" "logs" {

  bucket = "${var.environment}-logs"

}
```

Result:

```
dev-logs
```

---

# Variable Types

Terraform supports several data types.

## String

```hcl
variable "instance_type" {

  type = string

  default = "t2.micro"

}
```

---

## Number

```hcl
variable "instance_count" {

  type = number

  default = 2

}
```

---

## Boolean

```hcl
variable "enable_monitoring" {

  type = bool

  default = true

}
```

---

## List

```hcl
variable "availability_zones" {

  type = list(string)

  default = [

    "us-east-1a",

    "us-east-1b"

  ]

}
```

---

## Map

```hcl
variable "instance_types" {

  type = map(string)

  default = {

    dev = "t2.micro"

    prod = "t3.medium"

  }

}
```

---

## Object

```hcl
variable "server" {

  type = object({

    name = string

    cpu  = number

  })

}
```

---

# Required Variables

If a variable has **no default value**, Terraform asks for it during execution.

```hcl
variable "project_name" {

  type = string

}
```

Running:

```bash
terraform apply
```

Terraform prompts:

```
project_name:
```

---

# Default Values

```hcl
variable "region" {

  default = "us-east-1"

}
```

If no value is supplied, Terraform uses the default.

---

# Variable Description

Descriptions improve readability.

```hcl
variable "instance_type" {

  description = "EC2 instance type"

  type = string

  default = "t2.micro"

}
```

---

# Sensitive Variables

Sensitive variables prevent secrets from appearing in CLI output.

```hcl
variable "db_password" {

  type = string

  sensitive = true

}
```

Terraform hides the value when displaying output.

---

# Variable Validation

Validation ensures users provide acceptable values.

```hcl
variable "environment" {

  type = string

  validation {

    condition = contains(
      ["dev", "test", "prod"],
      var.environment
    )

    error_message = "Environment must be dev, test, or prod."

  }

}
```

If an invalid value is entered:

```
Error: Invalid value for variable.
```

---

# Assigning Variable Values

## 1. Default Value

```hcl
default = "t2.micro"
```

---

## 2. Command Line

```bash
terraform apply -var="instance_type=t3.micro"
```

---

## 3. Multiple Variables

```bash
terraform apply \
-var="region=us-east-1" \
-var="instance_type=t3.micro"
```

---

## 4. terraform.tfvars

Create:

```text
terraform.tfvars
```

Example:

```hcl
region = "us-east-1"

instance_type = "t3.micro"

environment = "prod"
```

Terraform automatically loads this file.

---

## 5. Variable File

Example:

```text
production.tfvars
```

Run:

```bash
terraform apply -var-file="production.tfvars"
```

---

## 6. Environment Variables

Linux/macOS:

```bash
export TF_VAR_region="us-east-1"
```

Windows PowerShell:

```powershell
$env:TF_VAR_region = "us-east-1"
```

Terraform automatically reads variables prefixed with `TF_VAR_`.

---

# Using Variables

Example:

```hcl
variable "instance_type" {

  default = "t2.micro"

}

resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = var.instance_type

}
```

---

# Variables with Expressions

```hcl
locals {

  server_size = var.environment == "prod"

    ? "t3.medium"

    : "t2.micro"

}
```

Variables work seamlessly with expressions and conditionals.

---

# Variables vs Locals

| Variables | Locals |
|-----------|--------|
| Accept user input | Internal reusable values |
| Can be overridden | Cannot be overridden |
| Defined with `variable` | Defined with `locals` |
| Used for configuration | Used for calculations and reuse |

---

# Real-World Example

```hcl
variable "environment" {

  default = "dev"

}

variable "instance_type" {

  default = "t2.micro"

}

resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = var.instance_type

  tags = {

    Environment = var.environment

  }

}
```

Change only:

```hcl
environment = "prod"

instance_type = "t3.medium"
```

No infrastructure code needs to change.

---

# Variable Precedence

Terraform resolves variables in the following order (highest precedence first):

1. `-var` and `-var-file` command-line arguments
2. `*.auto.tfvars` files
3. `terraform.tfvars`
4. Environment variables (`TF_VAR_*`)
5. Default values in the `variable` block

---

# Easy Way to Remember

Think of variables like blanks in a form.

```
Name: ________

Age: ________

City: ________
```

Each person fills in different values.

Terraform works the same way.

```
Environment = dev

Region = us-east-1

Instance Type = t2.micro
```

The configuration stays the same—only the values change.

---

# Best Practices

- Use descriptive variable names.
- Add descriptions for every variable.
- Specify data types.
- Use validation whenever possible.
- Mark secrets as `sensitive`.
- Store environment-specific values in `.tfvars` files.
- Avoid hardcoding values.

---

# Common Mistakes

❌ Hardcoding environment-specific values.

❌ Omitting variable types.

❌ Forgetting variable validation.

❌ Committing sensitive `.tfvars` files to Git.

❌ Using unclear variable names.

---

# Interview Questions

### What are Terraform variables?

Variables are input values that make Terraform configurations reusable and configurable.

---

### How do you reference a variable?

```hcl
var.variable_name
```

Example:

```hcl
var.region
```

---

### What happens if a variable has no default value?

Terraform prompts the user to provide a value during execution.

---

### What is the purpose of `terraform.tfvars`?

It stores variable values separately from the Terraform configuration, making it easy to manage different environments.

---

### What does `sensitive = true` do?

It prevents sensitive variable values from being displayed in Terraform output.

---

### How do you pass variables from the command line?

```bash
terraform apply -var="region=us-east-1"
```

---

# Summary

Variables make Terraform configurations flexible and reusable by separating configuration values from infrastructure code.

Key concepts include:

- Variable declarations
- Data types
- Default values
- Required variables
- Validation
- Sensitive variables
- `.tfvars` files
- Environment variables
- Variable precedence

Mastering variables is essential for writing reusable, maintainable, and production-ready Terraform configurations.