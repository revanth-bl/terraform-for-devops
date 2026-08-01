# ✅ Terraform validate

## 📖 Introduction

`terraform validate` is a built-in Terraform command that checks whether your Terraform configuration is **syntactically correct** and **internally consistent**.

It verifies that your configuration follows Terraform language rules and that resource references, variable declarations, outputs, and providers are correctly configured.

Unlike `terraform plan`, **`terraform validate` does not connect to cloud providers or create infrastructure**. It only analyzes the configuration files.

Running `terraform validate` is one of the first quality checks every Terraform project should perform.

---

# What is terraform validate?

`terraform validate` checks whether your Terraform configuration is valid.

```
Terraform Configuration

↓

terraform validate

↓

Valid or Invalid
```

It helps detect configuration problems before deployment.

---

# Why Use terraform validate?

Without validation:

```
Write Terraform

↓

terraform apply

↓

Configuration Error
```

Problems:

- Syntax errors
- Missing variables
- Invalid resource references
- Incorrect provider configuration

---

With validation:

```
Write Terraform

↓

terraform validate

↓

Fix Errors

↓

terraform plan

↓

terraform apply
```

Errors are detected before deployment begins.

---

# Initialize First

Before running `terraform validate`, initialize the project:

```bash
terraform init
```

Terraform downloads:

- Providers
- Modules
- Backend configuration

After initialization, validation can begin.

---

# Basic Command

Validate the current project:

```bash
terraform validate
```

Example output:

```text
Success! The configuration is valid.
```

---

# Example

Terraform configuration:

```hcl
resource "aws_instance" "web" {

  ami           = "ami-123456"

  instance_type = "t2.micro"

}
```

Run:

```bash
terraform validate
```

Output:

```text
Success! The configuration is valid.
```

---

# Invalid Example

Configuration:

```hcl
resource "aws_instance" "web" {

  ami =

}
```

Run:

```bash
terraform validate
```

Example output:

```text
Error:

Expected an expression after '='
```

Terraform identifies the configuration problem immediately.

---

# What terraform validate Checks

It verifies:

- Terraform syntax
- Resource definitions
- Variable declarations
- Output definitions
- Module configuration
- Provider configuration
- Resource references
- Expression validity

---

# What terraform validate Does NOT Check

It does **not**:

- Create infrastructure
- Modify infrastructure
- Connect to cloud providers to verify resource existence
- Detect security vulnerabilities
- Estimate infrastructure changes

For those tasks, use:

- `terraform plan`
- TFLint
- Checkov
- tfsec

---

# Validation Workflow

```text
Write Terraform Code
          │
          ▼
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

This is the recommended Terraform workflow.

---

# CI/CD Integration

Validation is commonly performed in automated pipelines.

```text
Developer Pushes Code
          │
          ▼
GitHub Actions
          │
          ▼
terraform validate
          │
          ▼
Pass / Fail
```

This prevents invalid configurations from reaching production.

---

# Example GitHub Actions Step

```yaml
- name: Terraform Validate

  run: terraform validate
```

---

# terraform validate vs terraform plan

| terraform validate | terraform plan |
|--------------------|----------------|
| Checks configuration validity | Shows infrastructure changes |
| No infrastructure changes | No infrastructure changes |
| Detects syntax and configuration errors | Compares desired state with current infrastructure |
| Fast | May communicate with cloud providers depending on the configuration |

---

# terraform validate vs terraform fmt

| terraform validate | terraform fmt |
|--------------------|---------------|
| Validates configuration | Formats code |
| Detects configuration errors | Improves readability |
| No formatting | No validation |

Both commands should be used together.

---

# terraform validate vs TFLint

| terraform validate | TFLint |
|--------------------|---------|
| Checks Terraform configuration | Checks code quality and best practices |
| Built into Terraform | External linting tool |
| Syntax and configuration | Static analysis and provider-specific rules |

---

# Easy Way to Remember

Think of spell checking a document.

```
Write Document

↓

Spell Check

↓

Fix Errors

↓

Print
```

Terraform follows a similar process.

```
Write Terraform

↓

terraform validate

↓

Fix Errors

↓

Deploy
```

Validation ensures the configuration is correct before deployment.

---

# Best Practices

- Always run `terraform init` before validation.
- Validate every change before planning or applying.
- Include `terraform validate` in CI/CD pipelines.
- Combine validation with `terraform fmt`, TFLint, Checkov, and tfsec.
- Fix all validation errors before deployment.

---

# Common Mistakes

❌ Running `terraform validate` before `terraform init`.

❌ Assuming validation checks cloud resources.

❌ Believing validation performs security analysis.

❌ Skipping validation in CI/CD pipelines.

❌ Ignoring validation errors before running `terraform apply`.

---

# Interview Questions

### What does `terraform validate` do?

It checks whether a Terraform configuration is syntactically correct and internally consistent.

---

### Does `terraform validate` create infrastructure?

No. It only validates the configuration.

---

### Which command should be run before validation?

```bash
terraform init
```

---

### What is the difference between `terraform validate` and `terraform plan`?

`terraform validate` checks the configuration for correctness, while `terraform plan` shows the infrastructure changes Terraform intends to make.

---

### Does `terraform validate` detect security vulnerabilities?

No. Security analysis should be performed with tools such as Checkov or tfsec.

---

### Why is `terraform validate` important?

It catches configuration errors early, reducing failed deployments and improving Infrastructure as Code quality.

---

# Summary

`terraform validate` is Terraform's built-in configuration validation tool. It helps detect syntax and configuration issues before planning or deploying infrastructure.

Key concepts include:

- Configuration validation
- Syntax checking
- Resource validation
- Variable validation
- Provider validation
- `terraform init`
- CI/CD integration
- Best practices

Running `terraform validate` as part of every Terraform workflow helps ensure reliable, consistent, and production-ready Infrastructure as Code.