# 🔀 Terraform Conditionals & Loops

## 📖 Introduction

Terraform provides **conditional expressions** and **looping constructs** that allow you to create flexible, reusable, and dynamic infrastructure.

Instead of writing the same resource multiple times, you can use:

- Conditional expressions (`condition ? true : false`)
- `count`
- `for_each`
- `for` expressions
- Dynamic blocks

These features help reduce duplicate code and make Terraform configurations easier to maintain.

---

# Why Use Conditionals and Loops?

Without conditionals or loops:

```hcl
resource "aws_instance" "web1" {
  instance_type = "t2.micro"
}

resource "aws_instance" "web2" {
  instance_type = "t2.micro"
}

resource "aws_instance" "web3" {
  instance_type = "t2.micro"
}
```

This creates repetitive code.

With loops:

```hcl
resource "aws_instance" "web" {
  count         = 3
  ami           = "ami-123456789"
  instance_type = "t2.micro"
}
```

Much cleaner and easier to maintain.

---

# Conditional Expressions

Terraform uses the following syntax:

```hcl
condition ? true_value : false_value
```

Example:

```hcl
variable "environment" {
  default = "dev"
}

locals {
  instance_type = var.environment == "prod" ? "t3.medium" : "t2.micro"
}
```

Result:

```
dev  → t2.micro
prod → t3.medium
```

---

# Another Example

```hcl
output "backup_enabled" {
  value = var.environment == "prod" ? true : false
}
```

Output:

```
Production → true

Development → false
```

---

# count

The `count` meta-argument creates multiple identical resources.

Example:

```hcl
resource "aws_instance" "web" {
  count         = 3
  ami           = "ami-123456789"
  instance_type = "t2.micro"
}
```

Terraform creates:

```
aws_instance.web[0]

aws_instance.web[1]

aws_instance.web[2]
```

---

# Accessing count.index

```hcl
resource "aws_instance" "web" {
  count = 3

  ami           = "ami-123456789"
  instance_type = "t2.micro"

  tags = {
    Name = "Server-${count.index}"
  }
}
```

Result:

```
Server-0

Server-1

Server-2
```

---

# When to Use count

Use `count` when resources are identical.

Examples:

- EC2 instances
- S3 buckets
- Security groups
- Test servers

---

# for_each

`for_each` creates resources from a collection.

Example using a set:

```hcl
resource "aws_s3_bucket" "bucket" {
  for_each = toset([
    "dev",
    "test",
    "prod"
  ])

  bucket = each.value
}
```

Terraform creates:

```
dev

test

prod
```

---

# for_each with a Map

```hcl
variable "instances" {
  default = {
    app = "t2.micro"
    db  = "t3.small"
  }
}

resource "aws_instance" "server" {
  for_each = var.instances

  ami           = "ami-123456789"
  instance_type = each.value

  tags = {
    Name = each.key
  }
}
```

Result:

```
app → t2.micro

db → t3.small
```

---

# count vs for_each

| count | for_each |
|--------|----------|
| Uses numbers | Uses keys or values |
| Best for identical resources | Best for unique resources |
| Uses `count.index` | Uses `each.key` and `each.value` |
| Index changes can recreate resources | More stable resource tracking |

---

# for Expressions

Terraform can transform collections.

Example:

```hcl
locals {
  numbers = [1, 2, 3, 4]

  doubled = [
    for n in local.numbers : n * 2
  ]
}
```

Result:

```
[2, 4, 6, 8]
```

---

# Filtering with for

Example:

```hcl
locals {
  even_numbers = [
    for n in [1,2,3,4,5,6] :
    n
    if n % 2 == 0
  ]
}
```

Result:

```
[2,4,6]
```

---

# Dynamic Blocks

Dynamic blocks generate nested configuration blocks automatically.

Example:

```hcl
dynamic "ingress" {
  for_each = var.ports

  content {
    from_port = ingress.value
    to_port   = ingress.value
    protocol  = "tcp"
  }
}
```

Instead of writing multiple ingress blocks manually, Terraform generates them.

---

# Practical Example

Create EC2 instances based on environment.

```hcl
resource "aws_instance" "web" {

  count = var.environment == "prod" ? 3 : 1

  ami           = "ami-123456789"
  instance_type = "t2.micro"
}
```

Result

```
Development

1 EC2 Instance

Production

3 EC2 Instances
```

---

# Workflow Example

```text
Variable

↓

Condition

↓

Loop

↓

Terraform

↓

Infrastructure Created
```

---

# Easy Way to Remember

Think of it like a classroom.

### Condition

```
If today is Monday

↓

Wear Uniform

Else

↓

Wear Casual Clothes
```

---

### count

```
Need 5 Chairs?

↓

Create Chair × 5
```

---

### for_each

```
Students

↓

Ayan

Rahul

Kiran

↓

Create one ID card for each student
```

---

# Best Practices

- Use `count` for identical resources.
- Use `for_each` when resources have unique names or attributes.
- Prefer `for_each` for long-term infrastructure because it provides stable resource addressing.
- Keep conditional expressions simple and readable.
- Use dynamic blocks only when necessary.

---

# Common Mistakes

❌ Using `count` when resource identities matter.

❌ Confusing `count.index` with `each.key`.

❌ Creating overly complex conditional expressions.

❌ Using loops when a single resource is sufficient.

❌ Forgetting that changing a `count` index may recreate resources.

---

# Interview Questions

### What is a conditional expression in Terraform?

A conditional expression evaluates a condition and returns one value if true and another if false.

Syntax:

```hcl
condition ? true_value : false_value
```

---

### What is the difference between `count` and `for_each`?

- `count` creates resources based on a number.
- `for_each` creates resources from a map or set and provides stable resource identities.

---

### When should you use `for_each` instead of `count`?

Use `for_each` when each resource has a unique key or configuration.

---

### What is `count.index`?

It is the index of the current resource created with `count`, starting from `0`.

---

### What are `each.key` and `each.value`?

- `each.key` returns the key from a map or the value from a set.
- `each.value` returns the corresponding value from a map or the value itself for a set.

---

### What are dynamic blocks?

Dynamic blocks generate nested configuration blocks automatically using loops, reducing repetitive code.

---

# Summary

Terraform provides several ways to make infrastructure more dynamic:

- **Conditional Expressions** – Make decisions based on conditions.
- **count** – Create multiple identical resources.
- **for_each** – Create resources from maps or sets.
- **for Expressions** – Transform or filter collections.
- **Dynamic Blocks** – Generate nested blocks automatically.

Mastering these features allows you to write cleaner, more reusable, and production-ready Terraform configurations.