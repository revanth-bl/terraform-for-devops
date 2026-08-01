# 🎨 Terraform fmt

## 📖 Introduction

`terraform fmt` is a built-in Terraform command that **automatically formats Terraform configuration files** according to HashiCorp's official style guide.

Instead of manually aligning code, spacing, or indentation, Terraform formats your files consistently, making them easier to read, review, and maintain.

Although `terraform fmt` **does not validate or deploy infrastructure**, it is considered a best practice to run it before committing code.

---

# What is terraform fmt?

`terraform fmt` reformats Terraform configuration files (`.tf` files).

It automatically:

- Fixes indentation
- Aligns assignments
- Removes unnecessary spacing
- Organizes formatting
- Applies Terraform's standard style

Example:

```
Terraform Code

↓

terraform fmt

↓

Clean & Consistent Code
```

---

# Why Use terraform fmt?

Without formatting:

```hcl
resource "aws_instance" "web"{
ami="ami-123456"

instance_type="t2.micro"
}
```

Hard to read and inconsistent.

---

After running:

```bash
terraform fmt
```

Result:

```hcl
resource "aws_instance" "web" {

  ami           = "ami-123456"
  instance_type = "t2.micro"

}
```

Much cleaner and easier to maintain.

---

# Basic Command

Format the current directory:

```bash
terraform fmt
```

---

# Format a Specific File

```bash
terraform fmt main.tf
```

---

# Format a Specific Directory

```bash
terraform fmt terraform/
```

---

# Format Recursively

Format all Terraform files in the current directory and its subdirectories:

```bash
terraform fmt -recursive
```

This is useful for large projects with multiple modules.

---

# Check Formatting Without Modifying Files

Use the `-check` option:

```bash
terraform fmt -check
```

Terraform reports files that need formatting but does not modify them.

This is commonly used in CI/CD pipelines.

---

# Show Differences

Use:

```bash
terraform fmt -diff
```

Terraform displays the formatting changes that would be applied.

---

# Format and Show Changes

Combine options:

```bash
terraform fmt -check -diff
```

Useful for pull requests and automated code reviews.

---

# Write Changes (Default Behavior)

```bash
terraform fmt
```

Terraform updates files automatically.

---

# Formatting Workflow

```text
Write Terraform Code
          │
          ▼
terraform fmt
          │
          ▼
Consistent Formatting
          │
          ▼
Commit Code
```

---

# Example Project

Before formatting:

```hcl
resource "aws_s3_bucket" "bucket"{
bucket="demo-bucket"
}
```

Run:

```bash
terraform fmt
```

After formatting:

```hcl
resource "aws_s3_bucket" "bucket" {

  bucket = "demo-bucket"

}
```

---

# CI/CD Integration

A common workflow:

```text
Developer Pushes Code
          │
          ▼
GitHub Actions
          │
          ▼
terraform fmt -check
          │
          ▼
Pass / Fail
```

If formatting is incorrect, the pipeline can fail until the code is properly formatted.

---

# terraform fmt vs terraform validate

| terraform fmt | terraform validate |
|---------------|--------------------|
| Formats code | Validates configuration |
| Improves readability | Checks syntax and configuration |
| Does not detect infrastructure errors | Detects configuration issues |
| Changes formatting | Does not modify files |

Both commands are commonly run before deployment.

---

# Common Workflow

```text
terraform fmt
      │
      ▼
terraform validate
      │
      ▼
terraform plan
      │
      ▼
terraform apply
```

This sequence helps ensure clean, valid, and predictable Terraform code.

---

# Easy Way to Remember

Think of formatting a document in Microsoft Word.

```
Messy Document

↓

Auto Format

↓

Professional Document
```

Terraform works the same way.

```
Messy Terraform Code

↓

terraform fmt

↓

Clean Terraform Code
```

---

# Best Practices

- Run `terraform fmt` before every commit.
- Use `terraform fmt -recursive` for larger projects.
- Add `terraform fmt -check` to CI/CD pipelines.
- Format code before creating pull requests.
- Keep formatting consistent across the team.
- Combine with `terraform validate`, TFLint, and Checkov for better quality.

---

# Common Mistakes

❌ Forgetting to run `terraform fmt`.

❌ Assuming `terraform fmt` checks for syntax or security issues.

❌ Manually formatting code inconsistently.

❌ Ignoring formatting failures in CI/CD pipelines.

❌ Running only `terraform fmt` without validating the configuration.

---

# Interview Questions

### What does `terraform fmt` do?

It automatically formats Terraform configuration files according to Terraform's standard style.

---

### Does `terraform fmt` validate infrastructure?

No. It only formats the code.

---

### Which command formats every Terraform file recursively?

```bash
terraform fmt -recursive
```

---

### Which command checks formatting without changing files?

```bash
terraform fmt -check
```

---

### What is the purpose of the `-diff` option?

It displays the formatting changes that would be applied.

---

### Why is `terraform fmt` important?

It improves readability, maintains consistent coding standards, and simplifies collaboration during code reviews.

---

# Summary

`terraform fmt` is Terraform's built-in code formatting tool that ensures consistent, readable, and standardized Terraform configuration files.

Key concepts include:

- Automatic formatting
- Standard style
- `terraform fmt`
- `-recursive`
- `-check`
- `-diff`
- CI/CD integration
- Code consistency

Using `terraform fmt` before every commit helps maintain clean Infrastructure as Code and supports efficient collaboration across development teams.