# 🗂️ Terraform Workspaces

## 📖 Introduction

**Terraform Workspaces** allow you to manage **multiple state files** using the same Terraform configuration.

Instead of copying your Terraform code for different environments, you can use workspaces to deploy separate infrastructure while keeping the configuration unchanged.

Workspaces are commonly used for environments such as:

- Development (Dev)
- Testing (Test)
- Staging (Stage)
- Production (Prod)

Each workspace maintains its own **Terraform state**, allowing infrastructure to remain isolated.

---

# What is a Workspace?

A workspace is an isolated instance of a Terraform state.

For example:

```
Terraform Code

↓

Workspace

↓

State File

↓

Infrastructure
```

The Terraform configuration remains the same, but each workspace has its own state.

---

# Default Workspace

Every Terraform project starts with a default workspace named:

```text
default
```

Check the current workspace:

```bash
terraform workspace show
```

Example output:

```text
default
```

---

# Why Use Workspaces?

Without workspaces:

```
Project

↓

Copy Folder

↓

Dev

↓

Copy Folder

↓

Prod
```

Problems:

- Duplicate code
- Hard to maintain
- Easy to make mistakes

---

With workspaces:

```
One Terraform Configuration

↓

Dev Workspace

↓

Stage Workspace

↓

Prod Workspace
```

One codebase manages multiple environments.

---

# Workspace Workflow

```text
Terraform Code
        │
        ▼
Select Workspace
        │
        ▼
Load Workspace State
        │
        ▼
Deploy Infrastructure
```

Each workspace has its own state file.

---

# Workspace Commands

## Show Current Workspace

```bash
terraform workspace show
```

---

## List Workspaces

```bash
terraform workspace list
```

Example:

```text
* default

  dev

  stage

  prod
```

The `*` indicates the active workspace.

---

## Create a Workspace

```bash
terraform workspace new dev
```

Terraform creates:

```
dev

↓

Separate State
```

---

## Switch Workspaces

```bash
terraform workspace select prod
```

Terraform now uses the production state.

---

## Delete a Workspace

```bash
terraform workspace delete dev
```

> You cannot delete the currently selected workspace. Switch to another workspace first.

---

# Using Workspace Information

Terraform provides the current workspace name through:

```hcl
terraform.workspace
```

Example:

```hcl
tags = {

  Environment = terraform.workspace

}
```

If the active workspace is `dev`, the tag becomes:

```text
Environment = dev
```

---

# Environment-Specific Values

Example:

```hcl
locals {

  instance_type = terraform.workspace == "prod" ? "t3.large" : "t2.micro"

}
```

Production:

```
t3.large
```

Development:

```
t2.micro
```

---

# Workspace State

Each workspace has its own state.

Example:

```
default

↓

terraform.tfstate
```

```
dev

↓

dev State
```

```
prod

↓

prod State
```

Although the configuration is the same, the infrastructure remains separate.

---

# Example Deployment

Create a workspace:

```bash
terraform workspace new dev
```

Apply:

```bash
terraform apply
```

Switch:

```bash
terraform workspace select prod
```

Apply again:

```bash
terraform apply
```

Terraform creates a separate production infrastructure because it uses a different state.

---

# Workspaces with Remote Backends

Many remote backends support workspaces.

Example (Amazon S3):

```
S3 Bucket

↓

Workspace

↓

Separate State Files
```

Each workspace stores its own state independently.

---

# Workspace Architecture

```text
Terraform Code
        │
        ▼
   Workspace
        │
   ┌────┼────┐
   │    │    │
 Dev Stage Prod
   │    │    │
State State State
```

The same Terraform configuration is reused while each workspace maintains isolated state.

---

# When to Use Workspaces

Good use cases:

- Development environments
- Testing environments
- Feature branches
- Demonstrations
- Small to medium-sized projects

---

# When Not to Use Workspaces

For many enterprise production environments, teams often prefer:

- Separate directories
- Separate backends
- Separate CI/CD pipelines
- Separate cloud accounts or subscriptions

This approach provides stronger isolation and simpler operational management.

---

# Workspaces vs Separate Directories

| Workspaces | Separate Directories |
|------------|----------------------|
| One codebase | Multiple environment directories |
| Separate state | Separate configuration and state |
| Less code duplication | Greater isolation |
| Easy switching | Better for complex enterprise environments |
| Good for simpler setups | Common in production-scale deployments |

---

# Easy Way to Remember

Think of a hotel.

```
Same Building

↓

Different Rooms

↓

Different Guests
```

Terraform workspaces work the same way.

```
Same Terraform Code

↓

Different Workspaces

↓

Different Infrastructure
```

---

# Best Practices

- Use descriptive workspace names.
- Keep separate state for every workspace.
- Tag resources with the workspace name.
- Use remote backends for team collaboration.
- Avoid storing secrets in Terraform configuration.
- Consider separate directories and backends for large production environments.

---

# Common Mistakes

❌ Forgetting to switch to the correct workspace before running `terraform apply`.

❌ Assuming workspaces provide complete isolation for every enterprise use case.

❌ Using the `default` workspace for production without careful planning.

❌ Mixing development and production resources accidentally.

❌ Hardcoding environment-specific values instead of using variables or `terraform.workspace`.

---

# Interview Questions

### What is a Terraform workspace?

A Terraform workspace is an isolated instance of Terraform state that allows the same configuration to manage multiple deployments.

---

### What is the default workspace called?

```text
default
```

---

### Which command creates a new workspace?

```bash
terraform workspace new <NAME>
```

---

### Which command switches workspaces?

```bash
terraform workspace select <NAME>
```

---

### How do you access the current workspace inside Terraform?

```hcl
terraform.workspace
```

---

### Should workspaces always be used for production?

Not necessarily. Many enterprise teams prefer separate directories, separate backends, and separate deployment pipelines for stronger isolation in production.

---

# Summary

Terraform Workspaces make it possible to reuse the same Terraform configuration while maintaining separate state files for different environments.

Key concepts include:

- Workspace
- Default workspace
- Workspace commands
- Separate state
- `terraform.workspace`
- Environment isolation
- Remote backends
- Workspaces vs separate directories

Workspaces are a powerful feature for managing multiple environments efficiently, especially in development and smaller deployments, while larger production environments often benefit from stronger isolation strategies.