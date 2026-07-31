# ☁️ Terraform Google Cloud Provider

## 📖 Introduction

The **Google Cloud Provider** enables Terraform to provision and manage resources on **Google Cloud Platform (GCP)** using Infrastructure as Code (IaC).

Instead of manually creating resources through the Google Cloud Console, you define them in Terraform configuration files and deploy them automatically.

The Google Cloud Provider is officially maintained by **HashiCorp** and integrates with Google Cloud APIs to manage cloud infrastructure.

Provider Name:

```text
google
```

---

# Why Use the Google Cloud Provider?

Without Terraform:

```
Open Google Cloud Console

↓

Create Project

↓

Create VPC

↓

Create VM

↓

Configure Firewall

↓

Repeat for every environment
```

With Terraform:

```bash
terraform apply
```

Terraform provisions the entire infrastructure automatically.

---

# Prerequisites

Before using the Google Cloud Provider, you should have:

- Google Cloud Account
- Google Cloud Project
- Billing Enabled
- Terraform Installed
- Google Cloud CLI (`gcloud`) Installed
- Appropriate IAM permissions

Verify the Google Cloud CLI:

```bash
gcloud version
```

Verify Terraform:

```bash
terraform version
```

---

# Authenticate with Google Cloud

Login using the Google Cloud CLI:

```bash
gcloud auth application-default login
```

Terraform uses **Application Default Credentials (ADC)** to authenticate.

Verify the active account:

```bash
gcloud auth list
```

---

# Configure the Google Provider

```hcl
terraform {

  required_providers {

    google = {

      source  = "hashicorp/google"

      version = "~> 6.0"

    }

  }

}

provider "google" {

  project = "my-gcp-project"

  region  = "us-central1"

  zone    = "us-central1-a"

}
```

---

# Provider Block Explained

```hcl
provider "google" {

  project = "my-gcp-project"

  region = "us-central1"

  zone = "us-central1-a"

}
```

- `project` – Google Cloud project ID.
- `region` – Default region for regional resources.
- `zone` – Default availability zone for zonal resources.

---

# Common Google Cloud Regions

Popular regions include:

- us-central1
- us-east1
- us-west1
- europe-west1
- europe-west2
- asia-south1
- asia-east1
- australia-southeast1

---

# Create a VPC Network

```hcl
resource "google_compute_network" "vpc" {

  name                    = "terraform-vpc"

  auto_create_subnetworks = false

}
```

---

# Create a Subnetwork

```hcl
resource "google_compute_subnetwork" "subnet" {

  name = "public-subnet"

  region = "us-central1"

  network = google_compute_network.vpc.id

  ip_cidr_range = "10.0.1.0/24"

}
```

---

# Create a Compute Engine VM

```hcl
resource "google_compute_instance" "vm" {

  name = "terraform-vm"

  machine_type = "e2-micro"

  zone = "us-central1-a"

  boot_disk {

    initialize_params {

      image = "debian-cloud/debian-12"

    }

  }

  network_interface {

    network = google_compute_network.vpc.id

  }

}
```

---

# Create a Cloud Storage Bucket

```hcl
resource "google_storage_bucket" "bucket" {

  name = "terraform-demo-bucket-12345"

  location = "US"

  force_destroy = true

}
```

> **Note:** Bucket names must be globally unique.

---

# Common Google Cloud Resources

Terraform can manage:

- Compute Engine
- VPC Networks
- Subnets
- Firewall Rules
- Cloud Storage
- Cloud SQL
- Kubernetes Engine (GKE)
- Cloud Functions
- Cloud Run
- Pub/Sub
- BigQuery
- IAM
- DNS
- Load Balancers

---

# Google Cloud Provider Workflow

```text
Write Terraform Code
          │
          ▼
terraform init
          │
          ▼
Download Google Provider
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
Google Cloud Resources Created
```

---

# Authentication Methods

Terraform supports multiple authentication methods.

### Application Default Credentials (ADC)

Recommended for development.

```bash
gcloud auth application-default login
```

---

### Service Account

Recommended for automation and CI/CD.

Example:

```bash
export GOOGLE_APPLICATION_CREDENTIALS="service-account.json"
```

Windows PowerShell:

```powershell
$env:GOOGLE_APPLICATION_CREDENTIALS="service-account.json"
```

---

### Workload Identity Federation

Recommended for secure production environments without long-lived service account keys.

---

# Best Practices

- Pin the Google provider version.
- Use separate projects for different environments.
- Store Terraform state remotely.
- Follow the principle of least privilege for IAM.
- Use modules for reusable infrastructure.
- Avoid hardcoding project IDs and regions.

---

# Common Mistakes

❌ Forgetting to enable required Google Cloud APIs.

❌ Not authenticating with `gcloud`.

❌ Hardcoding credentials in Terraform files.

❌ Using duplicate Cloud Storage bucket names.

❌ Granting overly permissive IAM roles.

---

# Interview Questions

### What is the Google Cloud Provider?

The Google Cloud Provider allows Terraform to provision and manage Google Cloud Platform resources.

---

### What is the provider name?

```text
google
```

---

### Which command authenticates Terraform using Application Default Credentials?

```bash
gcloud auth application-default login
```

---

### What is the recommended authentication method for CI/CD?

A **Service Account** or **Workload Identity Federation**, depending on the environment and security requirements.

---

### Can Terraform provision Google Kubernetes Engine (GKE)?

Yes. Terraform can provision and manage GKE clusters using the Google Cloud Provider.

---

### Why must Cloud Storage bucket names be unique?

Because bucket names are globally unique across all Google Cloud users.

---

# Summary

The Google Cloud Provider enables Terraform to automate the provisioning and management of Google Cloud infrastructure.

Key concepts include:

- Google Cloud authentication
- Provider configuration
- Compute Engine
- VPC Networks
- Cloud Storage
- IAM
- GKE
- Service Account authentication
- Google Cloud best practices

Mastering the Google Cloud Provider allows you to build scalable, repeatable, and production-ready Google Cloud infrastructure using Infrastructure as Code.