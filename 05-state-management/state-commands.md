# 🗂️ Terraform State Commands

## 📖 Introduction

Terraform keeps track of all managed infrastructure using a **state file** (`terraform.tfstate`).

Sometimes you need to inspect, modify, or repair the state without changing the actual infrastructure.

Terraform provides a set of **state commands** for managing the state file.

These commands allow you to:

- View resources in the state
- Inspect resource details
- Move resources within the state
- Remove resources from the state
- Import existing resources
- Replace resource providers

> **Important:** State commands modify Terraform's state, **not** the cloud infrastructure itself (unless otherwise noted).

---

# What is Terraform State?

Terraform State stores information about:

- Managed resources
- Resource IDs
- Attributes
- Dependencies
- Metadata

Example:

```text
Terraform Configuration
          │
          ▼
Terraform State
          │
          ▼
AWS / Azure / GCP
```

Terraform uses the state file to determine what changes are needed.

---

# Common State Commands

| Command | Purpose |
|----------|---------|
| `terraform state list` | List resources in the state |
| `terraform state show` | Display details of a resource |
| `terraform state mv` | Move or rename resources in the state |
| `terraform state rm` | Remove resources from the state |
| `terraform state replace-provider` | Replace provider references in the state |
| `terraform import` | Import existing infrastructure into the state |

---

# 1. terraform state list

Lists all resources currently managed by Terraform.

```bash
terraform state list
```

Example output:

```text
aws_instance.web

aws_vpc.main

aws_subnet.public

aws_security_group.web
```

Use this command to quickly see what Terraform is managing.

---

# 2. terraform state show

Displays detailed information about a specific resource.

Syntax:

```bash
terraform state show <RESOURCE>
```

Example:

```bash
terraform state show aws_instance.web
```

Example output:

```text
id = i-0123456789abcdef

instance_type = t2.micro

availability_zone = us-east-1a

public_ip = 54.xxx.xxx.xxx
```

---

# 3. terraform state mv

Moves or renames a resource in the state.

Syntax:

```bash
terraform state mv <SOURCE> <DESTINATION>
```

Example:

```bash
terraform state mv aws_instance.web aws_instance.frontend
```

Terraform updates the state without recreating the resource.

Common use cases:

- Renaming resources
- Refactoring configurations
- Moving resources into modules

---

# 4. terraform state rm

Removes a resource from the Terraform state **without deleting the actual infrastructure**.

Syntax:

```bash
terraform state rm <RESOURCE>
```

Example:

```bash
terraform state rm aws_instance.web
```

Result:

```
EC2 Instance Still Exists

↓

Terraform No Longer Manages It
```

Use this carefully.

---

# 5. terraform state replace-provider

Updates provider references stored in the state.

Example:

```bash
terraform state replace-provider hashicorp/aws hashicorp/aws
```

This command is commonly used during provider namespace changes or migrations.

---

# 6. terraform import

Imports existing infrastructure into Terraform state.

Syntax:

```bash
terraform import <RESOURCE> <RESOURCE_ID>
```

Example:

```bash
terraform import aws_instance.web i-0123456789abcdef0
```

Terraform begins managing the existing EC2 instance.

> **Important:** Import only updates the state. You should also write the corresponding resource block in your Terraform configuration.

---

# State Command Workflow

```text
Terraform State
        │
        ▼
State Command
        │
        ▼
Inspect or Modify State
        │
        ▼
Terraform State Updated
```

---

# Real-World Examples

## View Managed Resources

```bash
terraform state list
```

---

## Inspect an EC2 Instance

```bash
terraform state show aws_instance.web
```

---

## Rename a Resource

Before:

```text
aws_instance.web
```

Command:

```bash
terraform state mv aws_instance.web aws_instance.frontend
```

After:

```text
aws_instance.frontend
```

---

## Stop Managing a Resource

```bash
terraform state rm aws_s3_bucket.logs
```

The S3 bucket remains in AWS, but Terraform forgets it.

---

## Import an Existing VPC

```bash
terraform import aws_vpc.main vpc-0abc123456789def0
```

Terraform starts tracking the existing VPC.

---

# Local vs Remote State Commands

Whether your state is local or remote, the commands are generally the same.

```
terraform state list

↓

Local State
```

or

```
terraform state list

↓

Remote Backend

↓

Current State
```

Terraform automatically communicates with the configured backend.

---

# Easy Way to Remember

Imagine a company employee database.

```
Employee Exists

↓

Database Record
```

State commands work on the **database record**, not the employee.

Similarly:

```
Cloud Resource Exists

↓

Terraform State

↓

State Commands Update the Record
```

The infrastructure usually remains unchanged.

---

# Best Practices

- Back up the state before making manual changes.
- Review changes carefully when using state modification commands.
- Prefer normal Terraform workflows (`plan` and `apply`) whenever possible.
- Use `terraform import` before managing existing infrastructure.
- Test state operations in non-production environments first.

---

# Common Mistakes

❌ Editing the state manually unless absolutely necessary.

❌ Removing resources from the state unintentionally.

❌ Forgetting to add configuration after importing resources.

❌ Running state commands without backing up the state.

❌ Assuming `terraform state rm` deletes the infrastructure.

---

# Interview Questions

### What is the purpose of Terraform state commands?

They allow you to inspect and modify Terraform's state without directly changing the infrastructure.

---

### Which command lists all resources in the state?

```bash
terraform state list
```

---

### Which command shows resource details?

```bash
terraform state show
```

---

### Which command removes a resource from the state without deleting it?

```bash
terraform state rm
```

---

### Which command imports existing infrastructure?

```bash
terraform import
```

---

### What does `terraform state mv` do?

It moves or renames a resource within the Terraform state without recreating the underlying infrastructure.

---

# Summary

Terraform state commands provide powerful tools for inspecting, repairing, and reorganizing the Terraform state while keeping infrastructure intact.

Key concepts include:

- `terraform state list`
- `terraform state show`
- `terraform state mv`
- `terraform state rm`
- `terraform state replace-provider`
- `terraform import`

Understanding these commands is essential for safely managing Terraform state, especially in production environments and large Infrastructure as Code projects.