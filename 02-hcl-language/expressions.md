# 🧮 Terraform Expressions

## 📖 Introduction

**Expressions** are one of the most powerful features of Terraform. They allow you to calculate values, reference other resources, combine variables, perform operations, and build dynamic infrastructure configurations.

In simple terms:

> **Expressions are pieces of code that produce a value.**

Instead of hardcoding values everywhere, Terraform expressions let you generate values dynamically.

---

# Why Use Expressions?

Without expressions:

```hcl
resource "aws_instance" "web" {
  instance_type = "t2.micro"
}
```

With expressions:

```hcl
resource "aws_instance" "web" {
  instance_type = var.environment == "prod" ? "t3.medium" : "t2.micro"
}
```

Now the instance type changes automatically depending on the environment.

---

# Types of Expressions

Terraform supports several kinds of expressions:

- Literal values
- References
- Operators
- String interpolation
- Arithmetic expressions
- Boolean expressions
- Collection expressions
- Conditional expressions
- For expressions
- Splat expressions

---

# Literal Expressions

Literal expressions are fixed values.

### String

```hcl
"Terraform"
```

### Number

```hcl
10
```

### Boolean

```hcl
true
```

### List

```hcl
["dev", "test", "prod"]
```

### Map

```hcl
{
  region = "us-east-1"
}
```

---

# References

Terraform allows one resource to reference another.

Example:

```hcl
resource "aws_vpc" "main" {}

resource "aws_subnet" "public" {
  vpc_id = aws_vpc.main.id
}
```

Terraform automatically understands the dependency.

---

# Variable References

```hcl
variable "instance_type" {
  default = "t2.micro"
}

resource "aws_instance" "web" {
  instance_type = var.instance_type
}
```

Syntax:

```hcl
var.variable_name
```

---

# Local References

```hcl
locals {
  environment = "production"
}

output "env" {
  value = local.environment
}
```

Syntax:

```hcl
local.name
```

---

# Output References

Outputs from child modules can be referenced.

```hcl
module.network.vpc_id
```

---

# Operators

## Arithmetic Operators

| Operator | Meaning |
|----------|---------|
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Division |
| `%` | Modulus |

Example:

```hcl
5 + 5
```

Result:

```
10
```

---

## Comparison Operators

| Operator | Meaning |
|----------|---------|
| `==` | Equal |
| `!=` | Not Equal |
| `>` | Greater Than |
| `<` | Less Than |
| `>=` | Greater Than or Equal |
| `<=` | Less Than or Equal |

Example:

```hcl
var.environment == "prod"
```

---

## Logical Operators

| Operator | Meaning |
|----------|---------|
| `&&` | AND |
| `||` | OR |
| `!` | NOT |

Example:

```hcl
var.enabled && var.production
```

---

# String Interpolation

Terraform inserts values into strings using `${}`.

Example:

```hcl
resource "aws_instance" "web" {
  tags = {
    Name = "web-${var.environment}"
  }
}
```

Result:

```
web-dev
```

> **Note:** In modern Terraform, you can often reference values directly without interpolation unless you're combining them within a string.

---

# Collection Expressions

## List

```hcl
["dev", "test", "prod"]
```

Access:

```hcl
var.environments[0]
```

Result:

```
dev
```

---

## Map

```hcl
{
  dev = "t2.micro"
  prod = "t3.medium"
}
```

Access:

```hcl
var.instance_types["prod"]
```

Result:

```
t3.medium
```

---

# Conditional Expressions

Syntax:

```hcl
condition ? true_value : false_value
```

Example:

```hcl
instance_type = var.environment == "prod" ? "t3.medium" : "t2.micro"
```

Result:

```
Production → t3.medium

Development → t2.micro
```

---

# For Expressions

Example:

```hcl
locals {
  doubled = [
    for n in [1,2,3] : n * 2
  ]
}
```

Result:

```
[2,4,6]
```

---

# Filtering Collections

```hcl
locals {
  even = [
    for n in [1,2,3,4,5] :
    n
    if n % 2 == 0
  ]
}
```

Result:

```
[2,4]
```

---

# Splat Expressions

Splat expressions retrieve values from multiple resources.

Example:

```hcl
aws_instance.web[*].public_ip
```

Instead of:

```hcl
aws_instance.web[0].public_ip

aws_instance.web[1].public_ip

aws_instance.web[2].public_ip
```

Terraform returns all public IP addresses as a list.

---

# Combining Expressions

Example:

```hcl
locals {
  server_name = "${var.environment}-${var.application}"
}
```

Result:

```
dev-web

prod-api
```

---

# Real-World Example

```hcl
variable "environment" {
  default = "prod"
}

locals {
  instance_type = var.environment == "prod"
    ? "t3.medium"
    : "t2.micro"
}

resource "aws_instance" "web" {

  ami           = "ami-123456789"

  instance_type = local.instance_type

  tags = {
    Name = "server-${var.environment}"
  }
}
```

Terraform automatically selects the correct instance type and naming convention.

---

# Expression Flow

```text
Variables
      │
      ▼
Expressions
      │
      ▼
Terraform Evaluation
      │
      ▼
Infrastructure Created
```

---

# Easy Way to Remember

Think of expressions as **formulas in Microsoft Excel**.

Example:

```
Excel

=A1+B1

↓

Returns a Value
```

Terraform:

```hcl
var.cpu + 2
```

↓

Returns a value.

Expressions simply calculate or produce values.

---

# Best Practices

- Keep expressions simple and readable.
- Use locals for complex expressions.
- Avoid deeply nested conditional expressions.
- Use descriptive variable names.
- Reuse expressions instead of duplicating logic.

---

# Common Mistakes

❌ Writing overly complex one-line expressions.

❌ Confusing variables with locals.

❌ Hardcoding values instead of using expressions.

❌ Using unnecessary string interpolation in modern Terraform.

❌ Forgetting that expressions are evaluated during the plan phase.

---

# Interview Questions

### What are Terraform expressions?

Expressions are pieces of code that evaluate to a value and are used to build dynamic Terraform configurations.

---

### What are the different types of expressions?

- Literal
- References
- Operators
- Conditional
- Collection
- For
- Splat

---

### What is string interpolation?

It inserts values into strings using `${}`.

---

### What is a splat expression?

A shorthand for retrieving attributes from multiple resources.

Example:

```hcl
aws_instance.web[*].public_ip
```

---

### What is the purpose of `for` expressions?

They transform or filter collections such as lists, maps, and sets.

---

# Summary

Terraform expressions allow you to create dynamic, reusable, and flexible configurations.

The most commonly used expressions include:

- Literal values
- References
- Operators
- String interpolation
- Conditional expressions
- Collection expressions
- For expressions
- Splat expressions

Mastering expressions makes your Terraform code cleaner, more maintainable, and suitable for real-world DevOps and cloud infrastructure projects.