# ♻️ Terraform Lifecycle Meta-Argument

## 📖 Introduction

The **Lifecycle** meta-argument controls **how Terraform creates, updates, replaces, and destroys resources**.

Normally, Terraform follows its default behavior:

1. Compare the current infrastructure with the configuration.
2. Create, update, or destroy resources as needed.

Sometimes this default behavior is not ideal.

For example:

- Prevent accidental deletion of a production database.
- Create a new server before deleting the old one.
- Ignore changes made outside Terraform.
- Force replacement when another resource changes.

The `lifecycle` block allows you to customize this behavior.

---

# Why Use Lifecycle?

Without lifecycle:

```
Configuration Changed

↓

Terraform Deletes Resource

↓

Creates New Resource

↓

Application Downtime
```

With lifecycle:

```
Configuration Changed

↓

Create New Resource

↓

Switch Traffic

↓

Delete Old Resource

↓

No Downtime
```

---

# Syntax

```hcl
resource "<TYPE>" "<NAME>" {

  ...

  lifecycle {

  }

}
```

Example:

```hcl
resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = "t2.micro"

  lifecycle {

    create_before_destroy = true

  }

}
```

---

# Lifecycle Options

Terraform currently supports four lifecycle arguments:

- `create_before_destroy`
- `prevent_destroy`
- `ignore_changes`
- `replace_triggered_by`

---

# 1. create_before_destroy

Normally Terraform destroys the old resource before creating a new one.

Default behavior:

```text
Destroy Old Server
        │
        ▼
Create New Server
```

This can cause downtime.

Using:

```hcl
lifecycle {

  create_before_destroy = true

}
```

Terraform changes the order:

```text
Create New Server
        │
        ▼
Switch Traffic
        │
        ▼
Destroy Old Server
```

This reduces downtime.

---

## Example

```hcl
resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = "t2.micro"

  lifecycle {

    create_before_destroy = true

  }

}
```

---

# 2. prevent_destroy

Protects important resources from accidental deletion.

Example:

```hcl
resource "aws_db_instance" "database" {

  ...

  lifecycle {

    prevent_destroy = true

  }

}
```

If someone runs:

```bash
terraform destroy
```

Terraform returns:

```
Error

Resource is protected from destruction.
```

Useful for:

- Production databases
- Critical storage
- Stateful infrastructure

---

# 3. ignore_changes

Sometimes changes occur outside Terraform.

Example:

```
Terraform Creates EC2

↓

Administrator Changes Tags

↓

Terraform Wants To Revert Tags
```

Using:

```hcl
lifecycle {

  ignore_changes = [

    tags

  ]

}
```

Terraform ignores tag changes.

---

## Example

```hcl
resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = "t2.micro"

  tags = {

    Name = "Web"

  }

  lifecycle {

    ignore_changes = [

      tags

    ]

  }

}
```

---

Ignore multiple attributes:

```hcl
ignore_changes = [

  tags,

  user_data

]
```

---

Ignore all changes:

```hcl
ignore_changes = all
```

Use this carefully because Terraform will stop tracking changes to the resource's arguments.

---

# 4. replace_triggered_by

Forces Terraform to replace a resource when another resource or attribute changes.

Example:

```hcl
resource "aws_instance" "web" {

  ...

  lifecycle {

    replace_triggered_by = [

      aws_security_group.web

    ]

  }

}
```

If the security group is replaced, the EC2 instance is also replaced.

---

# Lifecycle Example

```hcl
resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = "t2.micro"

  lifecycle {

    create_before_destroy = true

    ignore_changes = [

      tags

    ]

  }

}
```

Terraform will:

- Create a new instance before deleting the old one.
- Ignore external changes to tags.

---

# Lifecycle Workflow

```text
Configuration Change
         │
         ▼
Lifecycle Rules Checked
         │
         ▼
Terraform Plan
         │
         ▼
Create / Update / Replace / Ignore
         │
         ▼
Infrastructure Updated
```

---

# Real-World Use Cases

## Production Database

```hcl
lifecycle {

  prevent_destroy = true

}
```

Avoid accidental deletion.

---

## Zero-Downtime Deployment

```hcl
lifecycle {

  create_before_destroy = true

}
```

Create the replacement resource before removing the old one.

---

## Auto-Scaling Tags

```hcl
lifecycle {

  ignore_changes = [

    tags

  ]

}
```

Ignore tag updates made by external systems.

---

## Rebuild on Dependency Change

```hcl
lifecycle {

  replace_triggered_by = [

    aws_launch_template.web

  ]

}
```

Replace dependent resources automatically when the launch template changes.

---

# Lifecycle vs Depends On

| Lifecycle | depends_on |
|-----------|------------|
| Controls how a resource is managed | Controls the order of resource operations |
| Uses the `lifecycle` block | Uses the `depends_on` argument |
| Affects create, update, replace, and destroy behavior | Defines dependencies between resources |

---

# Easy Way to Remember

Imagine moving to a new house.

Without planning:

```
Sell Old House

↓

Find New House

↓

No Place To Stay
```

With planning:

```
Buy New House

↓

Move In

↓

Sell Old House
```

This is exactly what:

```hcl
create_before_destroy = true
```

does for infrastructure.

---

# Best Practices

- Use `create_before_destroy` for zero-downtime replacements where supported.
- Protect critical resources with `prevent_destroy`.
- Use `ignore_changes` only for attributes intentionally managed outside Terraform.
- Keep lifecycle rules simple and well documented.
- Test lifecycle behavior in non-production environments first.

---

# Common Mistakes

❌ Using `ignore_changes = all` without understanding the consequences.

❌ Protecting temporary resources with `prevent_destroy`.

❌ Assuming every resource supports create-before-destroy without conflicts (for example, globally unique names may still prevent parallel creation).

❌ Overusing lifecycle rules instead of improving the resource design.

---

# Interview Questions

### What is the Terraform lifecycle block?

The lifecycle block is a meta-argument that controls how Terraform creates, updates, replaces, and destroys resources.

---

### What does `create_before_destroy` do?

It creates a replacement resource before deleting the existing one, helping reduce downtime.

---

### What does `prevent_destroy` do?

It prevents Terraform from accidentally destroying a protected resource.

---

### What does `ignore_changes` do?

It tells Terraform to ignore changes to specified resource attributes, even if they differ from the configuration.

---

### What does `replace_triggered_by` do?

It forces Terraform to replace a resource when another specified resource or attribute changes.

---

### Is `lifecycle` a resource?

No. It is a **meta-argument** used inside a resource block to customize Terraform's behavior.

---

# Summary

The `lifecycle` meta-argument gives you fine-grained control over how Terraform manages infrastructure.

Key concepts include:

- `create_before_destroy`
- `prevent_destroy`
- `ignore_changes`
- `replace_triggered_by`

Understanding lifecycle behavior is essential for building safe, reliable, and production-ready Terraform deployments while minimizing downtime and preventing accidental infrastructure changes.