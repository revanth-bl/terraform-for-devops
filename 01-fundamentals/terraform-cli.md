# 🖥️ Terraform CLI (Command Line Interface)

## 📖 Introduction

The **Terraform CLI (Command Line Interface)** is the primary way to interact with Terraform.

It allows you to:

- Initialize Terraform projects
- Validate configuration files
- Generate execution plans
- Provision infrastructure
- Destroy infrastructure
- Manage state
- Work with modules and workspaces

Every Terraform workflow begins and ends with the CLI.

---

# Terraform Workflow

The most common workflow is:

```text
Write Configuration
        │
        ▼
terraform init
        │
        ▼
terraform validate
        │
        ▼
terraform fmt
        │
        ▼
terraform plan
        │
        ▼
terraform apply
        │
        ▼
Infrastructure Created
```

---

# General Syntax

```bash
terraform <command> [options]
```

Example:

```bash
terraform plan
```

---

# Frequently Used Commands

| Command | Purpose |
|----------|---------|
| `terraform init` | Initialize a Terraform project |
| `terraform fmt` | Format configuration files |
| `terraform validate` | Validate Terraform configuration |
| `terraform plan` | Preview infrastructure changes |
| `terraform apply` | Create or update infrastructure |
| `terraform destroy` | Delete infrastructure |
| `terraform show` | Show current state or plan |
| `terraform output` | Display output variables |
| `terraform state` | Manage Terraform state |
| `terraform workspace` | Manage workspaces |
| `terraform providers` | Display providers |
| `terraform version` | Show Terraform version |

---

# terraform init

Initializes the working directory.

Downloads:

- Providers
- Modules
- Backend configuration

Command:

```bash
terraform init
```

Example Output:

```text
Initializing the backend...

Initializing provider plugins...

Terraform has been successfully initialized!
```

Run this command:

- Before using Terraform
- After changing providers
- After changing modules
- After configuring a backend

---

# terraform fmt

Formats Terraform configuration files.

Command:

```bash
terraform fmt
```

Format all files recursively:

```bash
terraform fmt -recursive
```

Example

Before:

```hcl
resource "aws_instance" "web"{
ami="ami-123456"
instance_type="t2.micro"
}
```

After:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

Best Practice:

Always run `terraform fmt` before committing code.

---

# terraform validate

Checks whether the Terraform configuration is valid.

Command:

```bash
terraform validate
```

Example Output:

```text
Success! The configuration is valid.
```

Validation checks:

- Syntax
- Required arguments
- Resource definitions
- Variable references

---

# terraform plan

Creates an execution plan without making any changes.

Command:

```bash
terraform plan
```

Terraform compares:

- Configuration files
- State file
- Existing infrastructure

Example:

```text
Plan: 2 to add, 1 to change, 0 to destroy.
```

Use `terraform plan` before every apply.

---

# terraform apply

Creates or updates infrastructure.

Command:

```bash
terraform apply
```

Terraform asks for confirmation:

```text
Do you want to perform these actions?

Enter a value:
```

Type:

```text
yes
```

Skip confirmation:

```bash
terraform apply -auto-approve
```

---

# terraform destroy

Deletes all managed infrastructure.

Command:

```bash
terraform destroy
```

Skip confirmation:

```bash
terraform destroy -auto-approve
```

Example:

```text
Destroy complete!
```

⚠️ Use with caution in production environments.

---

# terraform show

Displays information about the current state.

Command:

```bash
terraform show
```

Show a saved plan:

```bash
terraform show tfplan
```

Useful for debugging and reviewing infrastructure.

---

# terraform output

Displays output variables.

Example:

```hcl
output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

Command:

```bash
terraform output
```

Specific output:

```bash
terraform output instance_ip
```

---

# terraform state

Manages the Terraform state file.

Common commands:

```bash
terraform state list
```

```bash
terraform state show aws_instance.web
```

```bash
terraform state rm aws_instance.web
```

```bash
terraform state mv
```

Used for advanced state management.

---

# terraform workspace

Workspaces allow multiple environments using the same configuration.

Create:

```bash
terraform workspace new dev
```

List:

```bash
terraform workspace list
```

Switch:

```bash
terraform workspace select prod
```

Current workspace:

```bash
terraform workspace show
```

---

# terraform providers

Shows the providers used in the project.

```bash
terraform providers
```

Example Output:

```text
Providers required by configuration:

provider[registry.terraform.io/hashicorp/aws]
```

---

# terraform version

Displays the installed Terraform version.

```bash
terraform version
```

Upgrade information:

```bash
terraform version -json
```

---

# Useful Command Options

Skip approval:

```bash
-auto-approve
```

Disable colored output:

```bash
-no-color
```

Save execution plan:

```bash
terraform plan -out=tfplan
```

Apply saved plan:

```bash
terraform apply tfplan
```

---

# Complete Workflow Example

```bash
terraform init

terraform fmt

terraform validate

terraform plan

terraform apply

terraform output

terraform destroy
```

---

# Easy Way to Remember

```text
init
 ↓
fmt
 ↓
validate
 ↓
plan
 ↓
apply
 ↓
output
 ↓
destroy
```

Think of it as:

```
Prepare
↓

Check

↓

Preview

↓

Deploy

↓

Verify

↓

Remove
```

---

# Best Practices

✅ Run `terraform fmt` before committing code.

✅ Always validate configuration.

✅ Review the execution plan before applying.

✅ Never skip reading the plan in production.

✅ Use version control for all `.tf` files.

✅ Avoid using `-auto-approve` in production.

---

# Common Mistakes

❌ Forgetting `terraform init`

❌ Running `terraform apply` without reviewing the plan

❌ Editing the state file manually

❌ Forgetting to commit configuration files

❌ Destroying production infrastructure accidentally

---

# Interview Questions

### What is the purpose of `terraform init`?

It initializes the working directory by downloading providers, modules, and configuring the backend.

---

### What is the difference between `terraform plan` and `terraform apply`?

- `terraform plan` previews changes without modifying infrastructure.
- `terraform apply` executes the planned changes.

---

### Why is `terraform fmt` important?

It formats Terraform code into a consistent style, improving readability and collaboration.

---

### What does `terraform validate` check?

It verifies the syntax and internal consistency of the Terraform configuration without accessing cloud resources.

---

### Which command deletes all Terraform-managed resources?

```bash
terraform destroy
```

---

### How do you display output variables?

```bash
terraform output
```

---

# Summary

The Terraform CLI is the primary interface for managing infrastructure. The most important commands you'll use daily are:

- `terraform init`
- `terraform fmt`
- `terraform validate`
- `terraform plan`
- `terraform apply`
- `terraform output`
- `terraform destroy`

Mastering these commands is the foundation of working effectively with Terraform in real-world DevOps environments.