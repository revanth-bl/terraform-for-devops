# ⚙️ Terraform Meta-Arguments

## 📖 Introduction

**Meta-arguments** are special arguments that can be used with Terraform resources and modules to control **how Terraform creates, manages, and organizes infrastructure**.

Unlike normal arguments (such as `ami` or `instance_type`), meta-arguments do **not** configure the resource itself. Instead, they control Terraform's behavior.

For example, meta-arguments allow you to:

- Create multiple resources
- Create resources conditionally
- Define dependencies
- Customize resource lifecycle
- Specify different provider configurations

---

# What Are Meta-Arguments?

Normal Argument:

```hcl
instance_type = "t2.micro"
```

This configures the EC2 instance.

---

Meta-Argument:

```hcl
count = 3
```

This tells Terraform to create **three** EC2 instances.

---

# Available Meta-Arguments

Terraform provides the following commonly used meta-arguments:

- `count`
- `for_each`
- `depends_on`
- `lifecycle`
- `provider`
- `providers` (modules only)

---

# 1. count

Creates multiple copies of a resource.

Example:

```hcl
resource "aws_instance" "web" {

  count = 3

  ami = "ami-123456789"

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

### Access Count Index

```hcl
count.index
```

Example:

```hcl
tags = {

  Name = "server-${count.index}"

}
```

Result:

```
server-0

server-1

server-2
```

---

# 2. for_each

Creates resources from a map or set.

Example:

```hcl
resource "aws_s3_bucket" "bucket" {

  for_each = toset([

    "logs",

    "images",

    "backups"

  ])

  bucket = each.value

}
```

Terraform creates:

```
logs

images

backups
```

---

### Access Values

```hcl
each.key

each.value
```

For maps:

```hcl
for_each = {

  dev  = "t2.micro"

  prod = "t3.medium"

}
```

Example:

```hcl
instance_type = each.value
```

---

# count vs for_each

| count | for_each |
|--------|----------|
| Uses numbers | Uses keys |
| Best for identical resources | Best for unique resources |
| Uses `count.index` | Uses `each.key` and `each.value` |

---

# 3. depends_on

Creates an explicit dependency.

Example:

```hcl
resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = "t2.micro"

  depends_on = [

    aws_security_group.web

  ]

}
```

Terraform creates the Security Group before the EC2 instance.

---

# 4. lifecycle

Controls how Terraform creates, replaces, and destroys resources.

Example:

```hcl
resource "aws_instance" "web" {

  lifecycle {

    create_before_destroy = true

  }

}
```

Common lifecycle options:

- `create_before_destroy`
- `prevent_destroy`
- `ignore_changes`
- `replace_triggered_by`

---

# 5. provider

Selects a specific provider configuration for a resource.

Example:

```hcl
provider "aws" {

  alias = "east"

  region = "us-east-1"

}

provider "aws" {

  alias = "west"

  region = "us-west-2"

}

resource "aws_instance" "server" {

  provider = aws.west

  ami = "ami-123456789"

  instance_type = "t2.micro"

}
```

The EC2 instance is created in **us-west-2**.

---

# 6. providers (Modules Only)

When using modules, you can specify which provider configuration the module should use.

Example:

```hcl
module "network" {

  source = "./modules/network"

  providers = {

    aws = aws.east

  }

}
```

This passes the aliased provider configuration to the module.

---

# Real-World Example

```hcl
provider "aws" {

  region = "us-east-1"

}

resource "aws_instance" "web" {

  count = 2

  ami = "ami-123456789"

  instance_type = "t2.micro"

  lifecycle {

    create_before_destroy = true

  }

}
```

Terraform creates two EC2 instances and replaces them using create-before-destroy when necessary.

---

# Meta-Argument Workflow

```text
Read Configuration
        │
        ▼
Read Meta-Arguments
        │
        ▼
Build Dependency Graph
        │
        ▼
Determine Resource Count
        │
        ▼
Create Infrastructure
```

---

# Choosing the Right Meta-Argument

| Scenario | Meta-Argument |
|----------|---------------|
| Create multiple identical resources | `count` |
| Create multiple unique resources | `for_each` |
| Force creation order | `depends_on` |
| Control replacement or deletion | `lifecycle` |
| Use a different provider configuration | `provider` |
| Pass provider configurations to modules | `providers` |

---

# Easy Way to Remember

Think of a construction project.

```
How Many Buildings?

↓

count
```

```
Different Building Names?

↓

for_each
```

```
Foundation First?

↓

depends_on
```

```
Protect Building From Demolition?

↓

lifecycle
```

```
Build In Another City?

↓

provider
```

Terraform meta-arguments answer these kinds of operational questions.

---

# Best Practices

- Use `count` for identical resources.
- Use `for_each` for unique resources identified by keys.
- Prefer implicit dependencies before using `depends_on`.
- Use lifecycle rules only when needed.
- Use provider aliases for multi-region or multi-account deployments.
- Keep configurations simple and readable.

---

# Common Mistakes

❌ Using both `count` and `for_each` on the same resource (Terraform does not allow this).

❌ Overusing `depends_on` when implicit dependencies already exist.

❌ Confusing normal arguments with meta-arguments.

❌ Using provider aliases without specifying the correct provider configuration.

❌ Adding unnecessary lifecycle rules.

---

# Interview Questions

### What are Terraform meta-arguments?

Meta-arguments are special arguments that control Terraform's behavior rather than configuring the resource itself.

---

### What are the commonly used meta-arguments?

- `count`
- `for_each`
- `depends_on`
- `lifecycle`
- `provider`
- `providers` (modules)

---

### Can `count` and `for_each` be used together?

No. A resource or module can use **either** `count` **or** `for_each`, but not both.

---

### What is the difference between `count` and `for_each`?

`count` uses numeric indexes and is best for identical resources, while `for_each` uses keys or set values and is better for uniquely identified resources.

---

### What is the purpose of the `provider` meta-argument?

It tells Terraform which provider configuration (such as a specific AWS region or account) to use for a resource.

---

### Which meta-argument controls resource replacement behavior?

The `lifecycle` meta-argument.

---

# Summary

Meta-arguments give you control over how Terraform manages resources without changing the resource configuration itself.

Key concepts include:

- `count`
- `for_each`
- `depends_on`
- `lifecycle`
- `provider`
- `providers`

Mastering meta-arguments is essential for writing flexible, scalable, and production-ready Terraform configurations.