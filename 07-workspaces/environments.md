# 🌍 Terraform Environments

## 📖 Introduction

In real-world projects, the same infrastructure is usually deployed multiple times for different purposes.

Common environments include:

- Development (Dev)
- Testing (Test)
- Staging (Stage)
- Production (Prod)

Each environment has its own configuration, resources, and settings while often sharing the same Terraform code.

Using separate environments helps teams safely develop, test, and deploy infrastructure without affecting production systems.

---

# What is a Terraform Environment?

A Terraform environment is an isolated deployment of infrastructure with its own:

- State
- Variables
- Resources
- Configuration values

Example:

```
Development

↓

Testing

↓

Staging

↓

Production
```

Each environment represents a separate infrastructure deployment.

---

# Why Use Multiple Environments?

Imagine making changes directly in production.

```
Developer

↓

Modify Infrastructure

↓

Production

↓

Downtime
```

Instead:

```
Development

↓

Testing

↓

Staging

↓

Production
```

Changes are validated before reaching production.

---

# Common Environments

## Development (Dev)

Purpose:

- Build new features
- Experiment safely
- Learn and test ideas

Typical characteristics:

- Smaller resources
- Lower cost
- Frequent changes

Example:

```
EC2

↓

t2.micro
```

---

## Testing (Test)

Purpose:

- Verify functionality
- Run automated tests
- Validate deployments

Typical characteristics:

- Temporary infrastructure
- Automated provisioning and cleanup

---

## Staging (Stage)

Purpose:

- Simulate production
- Final validation before release

Typical characteristics:

- Nearly identical to production
- Used for user acceptance and performance testing

---

## Production (Prod)

Purpose:

- Serve real users
- Run business workloads

Typical characteristics:

- Highly available
- Secure
- Monitored
- Backed up
- Stable

Example:

```
Production

↓

Multiple EC2 Instances

↓

Load Balancer

↓

Database

↓

Monitoring
```

---

# Environment Workflow

```text
Development
      │
      ▼
Testing
      │
      ▼
Staging
      │
      ▼
Production
```

Each stage increases confidence before deployment.

---

# Managing Environments

There are several ways to manage environments in Terraform.

### Option 1: Workspaces

```
default

↓

dev

↓

stage

↓

prod
```

Each workspace has its own state file.

Example:

```bash
terraform workspace new dev

terraform workspace new prod
```

---

### Option 2: Separate Directories

Example:

```text
terraform/

├── dev/
│   ├── main.tf
│   └── terraform.tfvars
│
├── stage/
│   ├── main.tf
│   └── terraform.tfvars
│
└── prod/
    ├── main.tf
    └── terraform.tfvars
```

Each directory contains environment-specific configuration.

---

### Option 3: Variable Files

Use the same Terraform code with different variable files.

Development:

```bash
terraform apply -var-file="dev.tfvars"
```

Production:

```bash
terraform apply -var-file="prod.tfvars"
```

Example:

**dev.tfvars**

```hcl
instance_type = "t2.micro"

instance_count = 1
```

**prod.tfvars**

```hcl
instance_type = "t3.large"

instance_count = 3
```

---

# Environment-Specific Naming

Use clear naming conventions.

Example:

```text
dev-web-server

stage-web-server

prod-web-server
```

Or:

```hcl
tags = {

  Environment = "Production"

}
```

This makes resources easier to identify.

---

# Environment-Specific Variables

Example:

```hcl
variable "environment" {

  type = string

}
```

Usage:

```hcl
tags = {

  Environment = var.environment

}
```

Development:

```text
Environment = Dev
```

Production:

```text
Environment = Prod
```

---

# Environment Architecture

```text
Terraform Code
        │
        ▼
Environment Configuration
        │
        ├── Dev
        ├── Test
        ├── Stage
        └── Prod
```

The same codebase can create multiple isolated environments.

---

# Environment Isolation

Good practice:

```
Dev

↓

Separate State

↓

Separate Resources

↓

Separate Variables
```

Avoid sharing state files between environments.

---

# Example Project Structure

```text
terraform-project/

├── modules/
│
├── environments/
│   ├── dev/
│   │   ├── main.tf
│   │   └── terraform.tfvars
│   │
│   ├── stage/
│   │   ├── main.tf
│   │   └── terraform.tfvars
│   │
│   └── prod/
│       ├── main.tf
│       └── terraform.tfvars
│
└── modules/
```

This structure is common in enterprise Terraform projects.

---

# Easy Way to Remember

Think of software development.

```
Write Code

↓

Test

↓

Review

↓

Release
```

Terraform environments follow the same process.

```
Dev

↓

Test

↓

Stage

↓

Production
```

Each environment protects the next one from unexpected changes.

---

# Best Practices

- Keep production isolated from non-production environments.
- Use separate state files for each environment.
- Use different variable files or workspaces.
- Apply the principle of least privilege to each environment.
- Use consistent naming and tagging.
- Test infrastructure changes before deploying to production.
- Enable monitoring and backups for production.

---

# Common Mistakes

❌ Using one state file for all environments.

❌ Deploying directly to production without testing.

❌ Hardcoding environment-specific values.

❌ Sharing credentials across environments.

❌ Using inconsistent naming conventions.

❌ Forgetting to tag resources by environment.

---

# Interview Questions

### What is a Terraform environment?

A Terraform environment is an isolated deployment of infrastructure with its own configuration, variables, resources, and state.

---

### What are the common infrastructure environments?

- Development (Dev)
- Testing (Test)
- Staging (Stage)
- Production (Prod)

---

### How can Terraform manage multiple environments?

Common approaches include:

- Workspaces
- Separate directories
- Variable files (`*.tfvars`)

---

### Why should production be isolated?

To protect critical infrastructure, reduce risk, and prevent accidental changes from affecting live workloads.

---

### Should different environments share the same state file?

No. Each environment should have its own separate state file.

---

### Which approach is commonly preferred for production deployments?

Many organizations prefer **separate directories with separate backends and variable files** for stronger isolation, while workspaces are often suitable for simpler or smaller deployments.

---

# Summary

Terraform environments allow the same Infrastructure as Code to be deployed safely across multiple stages of the software lifecycle.

Key concepts include:

- Development
- Testing
- Staging
- Production
- Environment isolation
- Variable files
- Workspaces
- Separate state files
- Naming conventions

Properly managing environments improves reliability, reduces deployment risk, and supports scalable, production-ready Terraform workflows.