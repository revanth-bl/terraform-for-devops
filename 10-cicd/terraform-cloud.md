# ☁️ Terraform Cloud

## 📖 Introduction

**Terraform Cloud** is HashiCorp's managed platform for developing, managing, and automating Terraform workflows.

Instead of running Terraform commands on your local machine, Terraform Cloud executes them in a secure, centralized environment.

It provides features such as:

- Remote state management
- Team collaboration
- Secure state storage
- Version control integration
- Policy enforcement
- Cost estimation
- Remote execution
- Workspace management

Terraform Cloud is designed to simplify Infrastructure as Code (IaC) for individuals, teams, and enterprises.

---

# What is Terraform Cloud?

Terraform Cloud is a cloud-based service that manages Terraform workflows.

```
Developer

↓

Git Push

↓

Terraform Cloud

↓

Terraform Run

↓

Cloud Provider

↓

Infrastructure
```

Instead of executing Terraform locally, Terraform Cloud performs the operations remotely.

---

# Why Use Terraform Cloud?

Without Terraform Cloud:

```
Developer

↓

Laptop

↓

terraform apply
```

Problems:

- Local state files
- Difficult collaboration
- Manual deployments
- Credential management
- Limited visibility

---

With Terraform Cloud:

```
Developer

↓

Git Repository

↓

Terraform Cloud

↓

Automatic Deployment
```

Benefits:

- Centralized state
- Team collaboration
- Remote execution
- Secure credentials
- Audit history
- Policy enforcement

---

# Key Features

Terraform Cloud provides:

- Remote state storage
- State locking
- Workspaces
- Remote execution
- Version control integration
- Cost estimation
- Policy as Code (Sentinel in supported plans)
- Team access control
- Run history
- Private module registry

---

# Terraform Cloud Workflow

```text
Developer Pushes Code
          │
          ▼
GitHub / GitLab / Bitbucket
          │
          ▼
Terraform Cloud Workspace
          │
          ▼
terraform init
          │
          ▼
terraform plan
          │
          ▼
Approval (Optional)
          │
          ▼
terraform apply
          │
          ▼
Cloud Infrastructure
```

---

# Workspaces

A **Workspace** is an isolated environment that manages:

- Terraform configuration
- Variables
- State
- Runs
- Permissions

Example:

```text
Organization

├── Dev Workspace

├── Test Workspace

└── Production Workspace
```

Each workspace has its own independent Terraform state.

---

# Remote State

Terraform Cloud automatically stores the state remotely.

```
Terraform

↓

Terraform Cloud

↓

Remote State

↓

State Locking
```

Benefits:

- Centralized state
- Automatic backups
- Team collaboration
- Secure storage

---

# Version Control Integration

Terraform Cloud integrates directly with:

- GitHub
- GitLab
- Bitbucket
- Azure DevOps (via supported workflows)

Workflow:

```
Git Push

↓

Terraform Cloud

↓

Automatic Plan

↓

Apply
```

Infrastructure changes can be triggered automatically after code changes.

---

# Remote Execution

Instead of running Terraform on your laptop:

```
Laptop

↓

Terraform Cloud

↓

Remote Runner

↓

Cloud Provider
```

Advantages:

- Consistent execution environment
- No local credentials required
- Better security
- Centralized logs

---

# Variables

Terraform Cloud stores variables securely.

Examples:

```
AWS_ACCESS_KEY_ID

AWS_SECRET_ACCESS_KEY

region

instance_type
```

Sensitive variables are encrypted and hidden from users without appropriate permissions.

---

# Policy as Code

Terraform Cloud supports **Sentinel** (available in supported plans).

Example:

```
Developer

↓

Terraform Plan

↓

Sentinel Policy

↓

Approved

↓

Deploy
```

Policies can enforce organizational standards such as:

- Required tags
- Approved regions
- Resource limits
- Security requirements

---

# Cost Estimation

Terraform Cloud can estimate infrastructure costs before deployment.

```
Terraform Plan

↓

Cost Estimate

↓

Review

↓

Deploy
```

This helps teams make informed deployment decisions.

---

# Terraform Cloud Architecture

```text
Developer
      │
      ▼
Git Repository
      │
      ▼
Terraform Cloud
      │
      ▼
Workspace
      │
      ▼
Terraform Run
      │
      ▼
Cloud Provider
      │
      ▼
Infrastructure
```

---

# Terraform Cloud vs Local Terraform

| Local Terraform | Terraform Cloud |
|-----------------|-----------------|
| Local execution | Remote execution |
| Local state | Remote state |
| Single-user workflow | Team collaboration |
| Manual deployments | Automated workflows |
| Local credentials | Secure credential management |
| Limited run history | Centralized run history |

---

# Terraform Cloud vs GitHub Actions

| Terraform Cloud | GitHub Actions |
|-----------------|----------------|
| Purpose-built for Terraform | General-purpose CI/CD platform |
| Remote state built in | Requires separate remote backend |
| Native workspace management | External management required |
| Cost estimation and policy features | Requires additional tools |
| Managed Terraform workflow | Flexible automation platform |

Many organizations use GitHub Actions to trigger Terraform Cloud runs.

---

# Easy Way to Remember

Think of Google Docs.

Without Google Docs:

```
Document

↓

Your Laptop
```

With Google Docs:

```
Document

↓

Cloud

↓

Everyone Collaborates
```

Terraform Cloud works the same way.

```
Terraform

↓

Terraform Cloud

↓

Shared Infrastructure Management
```

---

# Best Practices

- Use separate workspaces for development, testing, and production.
- Store secrets as sensitive variables.
- Connect Terraform Cloud to version control systems.
- Review execution plans before applying.
- Enable policy enforcement where appropriate.
- Restrict user permissions using role-based access control.
- Use remote state instead of local state for collaborative projects.

---

# Common Mistakes

❌ Using one workspace for every environment.

❌ Storing secrets directly in Terraform code.

❌ Skipping plan reviews.

❌ Granting excessive user permissions.

❌ Ignoring policy violations.

❌ Managing production infrastructure from local state instead of centralized remote state.

---

# Interview Questions

### What is Terraform Cloud?

Terraform Cloud is HashiCorp's managed platform for automating Terraform workflows with remote execution, remote state management, and collaboration features.

---

### What is a Workspace?

A Workspace is an isolated environment that manages Terraform configuration, variables, state, and execution history.

---

### Does Terraform Cloud store Terraform state?

Yes. It provides secure remote state storage with state locking.

---

### Can Terraform Cloud integrate with GitHub?

Yes. It supports integration with GitHub, GitLab, Bitbucket, and other version control systems.

---

### What is the advantage of remote execution?

Terraform runs in a consistent, centralized environment without requiring developers to execute commands locally.

---

### What is Sentinel?

Sentinel is HashiCorp's Policy as Code framework that allows organizations to enforce governance and compliance rules during Terraform runs (available in supported Terraform Cloud plans).

---

# Summary

Terraform Cloud is a managed platform that simplifies Infrastructure as Code by providing remote execution, remote state management, collaboration features, policy enforcement, and secure automation.

Key concepts include:

- Terraform Cloud
- Remote execution
- Remote state
- Workspaces
- Version control integration
- Sensitive variables
- Cost estimation
- Sentinel
- Team collaboration
- Infrastructure automation

Terraform Cloud enables teams to build secure, scalable, and production-ready Infrastructure as Code workflows with centralized management and automation.