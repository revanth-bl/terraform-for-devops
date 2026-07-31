# 🏗️ Terraform Resources

## 📖 Introduction

A **Resource** is the most important building block in Terraform.

A resource represents a piece of infrastructure that Terraform creates, manages, updates, and destroys.

Examples include:

- EC2 Instances
- S3 Buckets
- VPCs
- Security Groups
- Azure Virtual Machines
- Google Cloud Storage Buckets
- Kubernetes Deployments

Almost everything you create with Terraform is defined using a **resource block**.

---

# What is a Resource?

A resource tells Terraform:

- **What** to create
- **How** to configure it
- **Which provider** to use

Example:

```hcl
resource "aws_instance" "web" {

  ami           = "ami-123456789"

  instance_type = "t2.micro"

}
```

This tells Terraform to create an AWS EC2 instance.

---

# Resource Syntax

General syntax:

```hcl
resource "<RESOURCE_TYPE>" "<RESOURCE_NAME>" {

  argument = value

}
```

Example:

```hcl
resource "aws_s3_bucket" "logs" {

  bucket = "my-demo-bucket"

}
```

---

# Resource Components

Every resource has three main parts:

```text
resource
     │
     ▼
Resource Type
     │
     ▼
Resource Name
     │
     ▼
Arguments
```

Example:

```hcl
resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = "t2.micro"

}
```

- **Resource Type:** `aws_instance`
- **Resource Name:** `web`
- **Arguments:** `ami`, `instance_type`

---

# Resource Type

The resource type identifies **what** Terraform should create.

Examples:

```text
aws_instance

aws_vpc

aws_subnet

aws_security_group

aws_s3_bucket

aws_iam_role

azurerm_virtual_machine

google_compute_instance
```

---

# Resource Name

The resource name is a **local identifier** used only within the Terraform configuration.

Example:

```hcl
resource "aws_instance" "frontend" {

}
```

Reference it later:

```hcl
aws_instance.frontend.id
```

The resource name does **not** become the cloud resource's actual name unless you configure it explicitly.

---

# Arguments

Arguments configure the resource.

Example:

```hcl
resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = "t2.micro"

}
```

Arguments:

- `ami`
- `instance_type`

Different resource types support different arguments.

---

# Example: Create an EC2 Instance

```hcl
resource "aws_instance" "web" {

  ami           = "ami-123456789"

  instance_type = "t2.micro"

  tags = {

    Name = "Web Server"

  }

}
```

Terraform creates:

- One EC2 instance
- Instance type: `t2.micro`
- Name tag: `Web Server`

---

# Example: Create an S3 Bucket

```hcl
resource "aws_s3_bucket" "logs" {

  bucket = "terraform-demo-bucket"

}
```

---

# Example: Create a VPC

```hcl
resource "aws_vpc" "main" {

  cidr_block = "10.0.0.0/16"

}
```

---

# Resource References

Resources can reference one another.

Example:

```hcl
resource "aws_vpc" "main" {

  cidr_block = "10.0.0.0/16"

}

resource "aws_subnet" "public" {

  vpc_id = aws_vpc.main.id

  cidr_block = "10.0.1.0/24"

}
```

Terraform automatically creates the VPC before the subnet.

---

# Resource Attributes

Terraform exposes resource attributes after creation.

Example:

```hcl
aws_instance.web.id
```

Other common attributes:

```text
id

arn

public_ip

private_ip

public_dns

availability_zone
```

Example output:

```hcl
output "server_ip" {

  value = aws_instance.web.public_ip

}
```

---

# Multiple Resources with `count`

```hcl
resource "aws_instance" "web" {

  count = 3

  ami = "ami-123456789"

  instance_type = "t2.micro"

}
```

Terraform creates:

```
web[0]

web[1]

web[2]
```

---

# Multiple Resources with `for_each`

```hcl
resource "aws_s3_bucket" "bucket" {

  for_each = toset([

    "logs",

    "images",

    "backup"

  ])

  bucket = each.value

}
```

Terraform creates three different buckets.

---

# Resource Dependencies

Example:

```hcl
resource "aws_vpc" "main" {

  cidr_block = "10.0.0.0/16"

}

resource "aws_subnet" "public" {

  vpc_id = aws_vpc.main.id

}
```

Terraform detects the dependency automatically.

---

# Resource Lifecycle

Resources can customize Terraform behavior using the `lifecycle` meta-argument.

Example:

```hcl
resource "aws_instance" "web" {

  lifecycle {

    create_before_destroy = true

  }

}
```

---

# Common Resource Types

### AWS

- aws_instance
- aws_vpc
- aws_subnet
- aws_security_group
- aws_s3_bucket
- aws_iam_role
- aws_lambda_function
- aws_db_instance

---

### Azure

- azurerm_resource_group
- azurerm_virtual_network
- azurerm_subnet
- azurerm_linux_virtual_machine
- azurerm_storage_account

---

### Google Cloud

- google_compute_instance
- google_compute_network
- google_storage_bucket
- google_compute_subnetwork
- google_container_cluster

---

# Resource Workflow

```text
Write Resource Block
         │
         ▼
terraform init
         │
         ▼
terraform plan
         │
         ▼
terraform apply
         │
         ▼
Resource Created
         │
         ▼
Terraform State Updated
```

---

# Easy Way to Remember

Think of Terraform as an architect.

```
Blueprint

↓

Construction

↓

Building
```

Terraform Resource:

```
Resource Block

↓

Terraform

↓

Cloud Resource
```

A resource block is simply the blueprint that tells Terraform what to build.

---

# Best Practices

- Use descriptive resource names.
- Tag resources consistently.
- Avoid hardcoding values; use variables and locals.
- Use modules for reusable infrastructure.
- Reference resources instead of copying IDs.
- Follow cloud provider naming conventions.

---

# Common Mistakes

❌ Using duplicate resource names within the same module.

❌ Hardcoding IDs instead of referencing resources.

❌ Forgetting required arguments.

❌ Creating unnecessary duplicate resources.

❌ Not using tags or labels for resource identification.

---

# Interview Questions

### What is a Terraform resource?

A resource is a block that defines infrastructure Terraform should create, manage, update, or destroy.

---

### What is the syntax of a resource block?

```hcl
resource "<TYPE>" "<NAME>" {

}
```

---

### What is the difference between a resource type and a resource name?

- **Resource Type** identifies what kind of infrastructure is being managed (for example, `aws_instance`).
- **Resource Name** is the local identifier used within the Terraform configuration.

---

### How do you reference a resource attribute?

```hcl
aws_instance.web.id
```

---

### Can Terraform automatically detect dependencies between resources?

Yes. When one resource references another, Terraform creates an implicit dependency.

---

### Can a resource create multiple instances?

Yes. You can use either:

- `count`
- `for_each`

to create multiple resources.

---

# Summary

Resources are the foundation of every Terraform configuration. They define the infrastructure Terraform creates and manages.

Key concepts include:

- Resource blocks
- Resource types
- Resource names
- Arguments
- Attributes
- Resource references
- Dependencies
- Lifecycle
- `count`
- `for_each`

Understanding resources is essential for building scalable, maintainable, and production-ready Infrastructure as Code with Terraform.