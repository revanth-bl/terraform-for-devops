# 🔄 Terraform Workflow

## 📖 Introduction

Terraform follows a predictable workflow for provisioning and managing infrastructure. Each step has a specific purpose, ensuring infrastructure changes are **safe, repeatable, and version-controlled**.

Understanding this workflow is essential before working with real-world cloud environments.

---

# Terraform Workflow Overview

```text
Write Configuration (.tf files)
            │
            ▼
     terraform init
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
            │
            ▼
 Infrastructure Created
            │
            ▼
   terraform output
            │
            ▼
Modify Configuration
            │
            ▼
 terraform plan/apply
            │
            ▼
 terraform destroy (Optional)
```

---

# Step 1 – Write Configuration

The first step is to create Terraform configuration files.

Common files include:

```text
main.tf
variables.tf
outputs.tf
terraform.tfvars
providers.tf
```

Example:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web" {
  ami           = "ami-1234567890abcdef0"
  instance_type = "t2.micro"
}
```

At this stage, Terraform has not created any infrastructure.

---

# Step 2 – Initialize the Project

Run:

```bash
terraform init
```

This command:

- Downloads required provider plugins.
- Installs modules (if any).
- Configures the backend.
- Creates the `.terraform/` directory.

Example Output:

```text
Terraform has been successfully initialized!
```

Run `terraform init`:

- When starting a new project.
- After adding or changing providers.
- After configuring a backend.
- After adding new modules.

---

# Step 3 – Format the Configuration

Run:

```bash
terraform fmt
```

This formats Terraform files according to HashiCorp's standard style.

Example:

Before:

```hcl
resource "aws_instance" "web"{
ami="ami-123"
instance_type="t2.micro"
}
```

After:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-123"
  instance_type = "t2.micro"
}
```

Formatting improves readability and consistency.

---

# Step 4 – Validate the Configuration

Run:

```bash
terraform validate
```

Terraform checks:

- Syntax
- Resource definitions
- Variable references
- Configuration structure

Example Output:

```text
Success! The configuration is valid.
```

This step does **not** create any infrastructure.

---

# Step 5 – Create an Execution Plan

Run:

```bash
terraform plan
```

Terraform compares:

- Configuration files
- Current infrastructure
- Terraform state file

It then generates an execution plan.

Example Output:

```text
Plan: 2 to add, 1 to change, 0 to destroy.
```

This allows you to review changes before applying them.

---

# Step 6 – Apply the Changes

Run:

```bash
terraform apply
```

Terraform displays the planned changes and asks for confirmation.

```text
Do you want to perform these actions?

Enter a value:
```

Type:

```text
yes
```

Terraform then communicates with the cloud provider and provisions the infrastructure.

Example Output:

```text
Apply complete!
Resources: 2 added, 0 changed, 0 destroyed.
```

---

# Step 7 – Update the State File

After applying changes, Terraform updates the state file:

```text
terraform.tfstate
```

The state file stores information such as:

- Resource IDs
- Attributes
- Dependencies
- Current infrastructure state

Terraform uses this file to determine future changes.

---

# Step 8 – View Outputs

If outputs are defined:

```hcl
output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

Display them with:

```bash
terraform output
```

Example:

```text
instance_ip = 54.123.45.67
```

Outputs make important values easy to retrieve.

---

# Step 9 – Modify Infrastructure

Need to change something?

Simply edit your `.tf` files.

Example:

```hcl
instance_type = "t3.micro"
```

Then run:

```bash
terraform plan
```

Review the proposed changes, then:

```bash
terraform apply
```

Terraform updates only the necessary resources.

---

# Step 10 – Destroy Infrastructure (Optional)

When the infrastructure is no longer needed:

```bash
terraform destroy
```

Terraform removes all resources it manages.

Example Output:

```text
Destroy complete!
Resources: 2 destroyed.
```

⚠️ Use this command carefully, especially in production environments.

---

# Complete Workflow Example

```bash
terraform init

terraform fmt

terraform validate

terraform plan

terraform apply

terraform output
```

To remove the infrastructure:

```bash
terraform destroy
```

---

# Visual Workflow

```text
Developer
    │
    ▼
Write .tf Files
    │
    ▼
terraform init
    │
    ▼
Download Providers
    │
    ▼
terraform fmt
    │
    ▼
Format Code
    │
    ▼
terraform validate
    │
    ▼
Check Syntax
    │
    ▼
terraform plan
    │
    ▼
Preview Changes
    │
    ▼
terraform apply
    │
    ▼
Cloud Infrastructure
    │
    ▼
terraform.tfstate Updated
```

---

# Easy Way to Remember

Remember the workflow as:

```text
Write
  ↓
Init
  ↓
Format
  ↓
Validate
  ↓
Plan
  ↓
Apply
  ↓
Output
  ↓
Destroy
```

Or simply:

> **Write → Initialize → Check → Preview → Deploy → Verify → Clean Up**

---

# Best Practices

- Always run `terraform fmt` before committing code.
- Validate your configuration before planning.
- Review the execution plan carefully.
- Store state files securely.
- Never edit the state file manually.
- Use remote state for team environments.
- Commit only Terraform configuration files, not sensitive state files.

---

# Common Mistakes

❌ Forgetting to run `terraform init`

❌ Skipping `terraform validate`

❌ Applying changes without reviewing the plan

❌ Editing `terraform.tfstate` manually

❌ Committing secrets or state files to GitHub

❌ Using `terraform destroy` in the wrong environment

---

# Interview Questions

### What is the standard Terraform workflow?

Write configuration → Initialize → Format → Validate → Plan → Apply → Output → Destroy (optional).

---

### Why is `terraform plan` important?

It previews infrastructure changes before they are applied, helping prevent unintended modifications.

---

### What happens during `terraform init`?

Terraform downloads provider plugins, installs modules, configures the backend, and prepares the working directory.

---

### Does `terraform validate` create infrastructure?

No. It only checks the syntax and validity of the configuration.

---

### What file does Terraform update after applying changes?

`terraform.tfstate`

---

# Summary

A typical Terraform workflow consists of:

1. Write Terraform configuration.
2. Initialize the project.
3. Format the code.
4. Validate the configuration.
5. Review the execution plan.
6. Apply the changes.
7. View outputs.
8. Modify and reapply as needed.
9. Destroy infrastructure when it is no longer required.

Following this workflow helps ensure that infrastructure changes are consistent, predictable, and safe.