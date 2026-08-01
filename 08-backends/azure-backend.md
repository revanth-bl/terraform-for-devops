# ☁️ Terraform Azure Backend

## 📖 Introduction

A **Terraform Backend** determines where Terraform stores its **state file** (`terraform.tfstate`).

For production environments, storing the state locally is not recommended.

The **Azure Backend** stores the Terraform state in an **Azure Storage Account**, providing:

- Centralized state storage
- Team collaboration
- State locking
- High availability
- Encryption
- Better security

It is the recommended backend when managing infrastructure on Microsoft Azure.

---

# What is the Azure Backend?

The Azure Backend stores the Terraform state inside an Azure Blob Storage container.

```
Terraform

↓

Azure Storage Account

↓

Blob Container

↓

terraform.tfstate
```

Instead of saving the state on your local computer, Terraform reads and writes it from Azure Storage.

---

# Why Use the Azure Backend?

Without Azure Backend:

```
Laptop

↓

terraform.tfstate

↓

Single User
```

Problems:

- No collaboration
- Easy to lose
- No centralized storage
- Difficult backups

---

With Azure Backend:

```
Terraform

↓

Azure Storage

↓

Shared State

↓

Entire Team
```

Benefits:

- Shared state
- Secure storage
- Automatic encryption at rest
- State locking
- Reliable access

---

# Azure Backend Architecture

```text
Terraform CLI
       │
       ▼
Azure Storage Account
       │
       ▼
Blob Container
       │
       ▼
terraform.tfstate
```

---

# Backend Configuration

Example:

```hcl
terraform {

  backend "azurerm" {

    resource_group_name  = "terraform-rg"

    storage_account_name = "tfstorageaccount"

    container_name       = "tfstate"

    key                  = "production/terraform.tfstate"

  }

}
```

---

# Backend Parameters

| Parameter | Description |
|-----------|-------------|
| `resource_group_name` | Azure Resource Group containing the Storage Account |
| `storage_account_name` | Azure Storage Account name |
| `container_name` | Blob container where the state is stored |
| `key` | Name and path of the state file |

---

# Azure Resources Required

Before using the backend, create:

```
Resource Group

↓

Storage Account

↓

Blob Container
```

Terraform stores the state inside the blob container.

---

# Initialize the Backend

After configuring the backend:

```bash
terraform init
```

Terraform initializes the Azure backend and connects to the storage account.

---

# State Locking

Azure Blob Storage provides **lease-based locking**.

Workflow:

```text
terraform apply
        │
        ▼
Acquire Blob Lease
        │
        ▼
Update Infrastructure
        │
        ▼
Update State
        │
        ▼
Release Lease
```

This prevents multiple users from modifying the state simultaneously.

---

# State Encryption

Azure Storage automatically encrypts data at rest.

```
Terraform State

↓

Azure Storage

↓

Encrypted
```

No additional configuration is required for basic encryption at rest.

---

# Versioning

Azure Blob Storage supports versioning.

Benefits:

- Recover previous state versions
- Restore deleted state files
- Improve disaster recovery

Enable versioning from the Azure Storage Account settings.

---

# Separate State Per Environment

Use different keys for different environments.

Development:

```hcl
key = "dev/terraform.tfstate"
```

Staging:

```hcl
key = "stage/terraform.tfstate"
```

Production:

```hcl
key = "prod/terraform.tfstate"
```

This keeps environment state files isolated.

---

# Backend Workflow

```text
terraform init
        │
        ▼
Connect To Azure Storage
        │
        ▼
Read State
        │
        ▼
terraform plan
        │
        ▼
terraform apply
        │
        ▼
Update State
```

---

# Local State vs Azure Backend

| Local State | Azure Backend |
|-------------|---------------|
| Stored on local machine | Stored in Azure Storage |
| Single-user | Multi-user |
| No centralized storage | Centralized storage |
| No locking | Blob lease locking |
| Easy to lose | Highly available |
| Limited security | Encryption and access control |

---

# Azure Backend Directory Example

```text
terraform-project/

├── backend.tf
├── main.tf
├── variables.tf
├── outputs.tf
└── terraform.tfvars
```

**backend.tf**

```hcl
terraform {

  backend "azurerm" {

    resource_group_name  = "terraform-rg"

    storage_account_name = "tfstorageaccount"

    container_name       = "tfstate"

    key                  = "prod/terraform.tfstate"

  }

}
```

---

# Easy Way to Remember

Think of cloud storage.

```
Local File

↓

One Computer
```

```
Cloud Drive

↓

Everyone Can Access It
```

Terraform Azure Backend works similarly.

```
Terraform State

↓

Azure Storage

↓

Shared Secure State
```

---

# Best Practices

- Use Azure Storage for production state.
- Enable Blob versioning.
- Restrict access using Azure RBAC and least privilege.
- Keep separate state files for each environment.
- Enable soft delete for Blob Storage when appropriate.
- Never commit `terraform.tfstate` to Git.
- Back up important state data.

---

# Common Mistakes

❌ Storing production state locally.

❌ Using the same state file for multiple environments.

❌ Granting excessive permissions to the storage account.

❌ Forgetting to enable versioning.

❌ Committing state files to version control.

---

# Interview Questions

### What is the Terraform Azure Backend?

It is a backend that stores the Terraform state file in an Azure Storage Account using Blob Storage.

---

### Which Azure service stores the state?

Azure Blob Storage inside an Azure Storage Account.

---

### Which command initializes the backend?

```bash
terraform init
```

---

### Does the Azure Backend support state locking?

Yes. It uses Azure Blob Storage lease-based locking.

---

### Why should Azure Backend be used instead of local state?

It provides centralized storage, collaboration, encryption, high availability, and state locking.

---

### How can you separate environments?

Use different state keys, such as:

```text
dev/terraform.tfstate

stage/terraform.tfstate

prod/terraform.tfstate
```

---

# Summary

The Terraform Azure Backend is the recommended way to store Terraform state when working with Microsoft Azure.

Key concepts include:

- Azure Storage Account
- Blob Container
- Remote state
- State locking
- Encryption
- Versioning
- Environment isolation
- `terraform init`

Using the Azure Backend helps teams collaborate safely while keeping Terraform state secure, reliable, and production-ready.