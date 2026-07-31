# ☁️ Terraform Azure Provider

## 📖 Introduction

The **Azure Provider** enables Terraform to provision and manage Microsoft Azure resources using Infrastructure as Code (IaC).

Instead of manually creating resources through the Azure Portal, you can define them in Terraform configuration files and deploy them consistently and repeatedly.

The Azure provider is officially maintained by **HashiCorp** in collaboration with **Microsoft**.

Provider Name:

```text
azurerm
```

---

# Why Use the Azure Provider?

Without Terraform:

```
Login to Azure Portal

↓

Create Resource Group

↓

Create Virtual Network

↓

Create VM

↓

Configure Networking

↓

Repeat for every environment
```

With Terraform:

```bash
terraform apply
```

Terraform automatically creates all resources.

---

# Prerequisites

Before using the Azure Provider, you should have:

- Azure Subscription
- Azure Account
- Azure CLI Installed
- Terraform Installed
- Required Azure permissions

Verify Azure CLI:

```bash
az version
```

Verify Terraform:

```bash
terraform version
```

---

# Authenticate with Azure

Login using Azure CLI:

```bash
az login
```

Terraform automatically uses your Azure CLI credentials.

Verify your account:

```bash
az account show
```

---

# Configure the Azure Provider

Basic configuration:

```hcl
terraform {

  required_providers {

    azurerm = {

      source  = "hashicorp/azurerm"

      version = "~> 4.0"

    }

  }

}

provider "azurerm" {

  features {}

}
```

The `features {}` block is required by the Azure provider, even if left empty.

---

# Provider Block Explained

```hcl
provider "azurerm" {

  features {}

}
```

- `provider` specifies the cloud provider.
- `azurerm` is the Azure Resource Manager provider.
- `features {}` enables provider-specific functionality.

---

# Configure a Region

Azure resources are deployed within a **Resource Group**, which specifies the location.

Example:

```hcl
resource "azurerm_resource_group" "main" {

  name     = "rg-demo"

  location = "East US"

}
```

Common Azure regions:

- East US
- West US
- Central US
- North Europe
- West Europe
- Southeast Asia
- East Asia
- Australia East
- Central India
- South India

---

# Create a Resource Group

```hcl
resource "azurerm_resource_group" "main" {

  name     = "rg-terraform-demo"

  location = "East US"

}
```

---

# Create a Virtual Network

```hcl
resource "azurerm_virtual_network" "main" {

  name                = "demo-vnet"

  location            = azurerm_resource_group.main.location

  resource_group_name = azurerm_resource_group.main.name

  address_space = [
    "10.0.0.0/16"
  ]

}
```

Terraform automatically recognizes the dependency on the resource group.

---

# Create a Subnet

```hcl
resource "azurerm_subnet" "public" {

  name = "public-subnet"

  resource_group_name = azurerm_resource_group.main.name

  virtual_network_name = azurerm_virtual_network.main.name

  address_prefixes = [
    "10.0.1.0/24"
  ]

}
```

---

# Create a Storage Account

```hcl
resource "azurerm_storage_account" "storage" {

  name = "terraformstorage123"

  resource_group_name = azurerm_resource_group.main.name

  location = azurerm_resource_group.main.location

  account_tier = "Standard"

  account_replication_type = "LRS"

}
```

> **Note:** Storage account names must be globally unique and use only lowercase letters and numbers.

---

# Create a Virtual Machine

```hcl
resource "azurerm_linux_virtual_machine" "vm" {

  name = "terraform-vm"

  resource_group_name = azurerm_resource_group.main.name

  location = azurerm_resource_group.main.location

  size = "Standard_B1s"

  admin_username = "azureuser"

}
```

A production VM also requires networking, disks, and an SSH key or password configuration.

---

# Common Azure Resources

Terraform can manage:

- Resource Groups
- Virtual Networks (VNet)
- Subnets
- Network Security Groups
- Virtual Machines
- Storage Accounts
- Load Balancers
- Azure Kubernetes Service (AKS)
- Azure SQL Database
- App Services
- Key Vault
- Virtual Machine Scale Sets
- Managed Identities

---

# Azure Provider Workflow

```text
Write Terraform Code
          │
          ▼
terraform init
          │
          ▼
Download Azure Provider
          │
          ▼
terraform plan
          │
          ▼
Review Changes
          │
          ▼
terraform apply
          │
          ▼
Azure Resources Created
```

---

# Authentication Methods

Terraform supports multiple authentication methods.

### Azure CLI

```bash
az login
```

Best for learning and development.

---

### Service Principal

Used in automation and CI/CD pipelines.

Example environment variables:

```bash
ARM_CLIENT_ID
ARM_CLIENT_SECRET
ARM_SUBSCRIPTION_ID
ARM_TENANT_ID
```

---

### Managed Identity

Commonly used when Terraform runs inside Azure.

---

# Best Practices

- Use the latest stable Azure provider version.
- Create separate Resource Groups for different environments.
- Use remote state for team collaboration.
- Store secrets securely (Azure Key Vault or environment variables).
- Use modules for reusable infrastructure.
- Follow consistent naming conventions.

---

# Common Mistakes

❌ Forgetting to run `az login`.

❌ Not specifying the Azure provider version.

❌ Hardcoding secrets in Terraform files.

❌ Using non-unique Storage Account names.

❌ Deploying all resources into a single Resource Group without planning.

---

# Interview Questions

### What is the Azure Provider?

The Azure Provider (`azurerm`) allows Terraform to provision and manage Microsoft Azure resources.

---

### What command authenticates Terraform with Azure using the Azure CLI?

```bash
az login
```

---

### What is the required provider name for Azure?

```text
azurerm
```

---

### Why is the `features {}` block required?

It initializes provider-specific features and is mandatory, even when empty.

---

### Which authentication method is commonly used in CI/CD pipelines?

**Service Principal** authentication using environment variables.

---

### Can Terraform manage Azure Kubernetes Service (AKS)?

Yes. Terraform can provision and manage AKS clusters using the Azure Provider.

---

# Summary

The Azure Provider (`azurerm`) enables Terraform to automate the creation and management of Microsoft Azure infrastructure.

Key concepts include:

- Azure authentication
- Provider configuration
- Resource Groups
- Virtual Networks
- Storage Accounts
- Virtual Machines
- Service Principal authentication
- Azure best practices

Mastering the Azure Provider allows you to deploy scalable, repeatable, and production-ready Azure infrastructure using Infrastructure as Code.