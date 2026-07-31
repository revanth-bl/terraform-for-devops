# 📚 Terraform Data Sources

## 📖 Introduction

A **Data Source** allows Terraform to **read information about existing infrastructure** without creating or modifying it.

While **Resources** create and manage infrastructure, **Data Sources** retrieve information from infrastructure that already exists.

This is useful when you need to reference existing cloud resources such as:

- VPCs
- AMIs
- Security Groups
- IAM Roles
- Subnets
- Availability Zones
- Existing Storage Buckets

---

# Resource vs Data Source

| Resource | Data Source |
|----------|-------------|
| Creates infrastructure | Reads existing infrastructure |
| Managed by Terraform | Not managed by Terraform |
| Uses `resource` block | Uses `data` block |
| Can create/update/delete | Read-only |
| Stored in Terraform state | Information stored, but resource ownership remains external |

---

# Why Use Data Sources?

Imagine your company already has:

- Existing VPC
- Existing IAM Roles
- Existing Security Groups
- Existing Database

Instead of recreating them, Terraform simply reads their information.

Example:

```
Existing VPC

↓

Terraform Data Source

↓

Retrieve VPC ID

↓

Use It
```

---

# Syntax

General syntax:

```hcl
data "<PROVIDER_RESOURCE>" "<NAME>" {

}
```

Example:

```hcl
data "aws_vpc" "main" {

  default = true

}
```

---

# Referencing a Data Source

Syntax:

```hcl
data.<TYPE>.<NAME>.<ATTRIBUTE>
```

Example:

```hcl
data.aws_vpc.main.id
```

---

# Example 1: Existing VPC

```hcl
data "aws_vpc" "default" {

  default = true

}
```

Output:

```hcl
output "vpc_id" {

  value = data.aws_vpc.default.id

}
```

---

# Example 2: Get Latest Amazon Linux AMI

```hcl
data "aws_ami" "amazon_linux" {

  most_recent = true

  owners = ["amazon"]

  filter {

    name = "name"

    values = [
      "al2023-ami-*"
    ]

  }

}
```

Use:

```hcl
resource "aws_instance" "web" {

  ami = data.aws_ami.amazon_linux.id

  instance_type = "t2.micro"

}
```

Terraform always uses the latest matching AMI.

---

# Example 3: Availability Zones

```hcl
data "aws_availability_zones" "available" {

  state = "available"

}
```

Output:

```hcl
output "zones" {

  value = data.aws_availability_zones.available.names

}
```

---

# Example 4: Existing Security Group

```hcl
data "aws_security_group" "web" {

  name = "web-sg"

}
```

Use:

```hcl
resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = "t2.micro"

  vpc_security_group_ids = [

    data.aws_security_group.web.id

  ]

}
```

---

# Example 5: Existing Subnet

```hcl
data "aws_subnet" "public" {

  id = "subnet-12345678"

}
```

Use:

```hcl
subnet_id = data.aws_subnet.public.id
```

---

# Data Sources in Azure

Example:

```hcl
data "azurerm_resource_group" "rg" {

  name = "production-rg"

}
```

Use:

```hcl
data.azurerm_resource_group.rg.location
```

---

# Data Sources in Google Cloud

Example:

```hcl
data "google_compute_image" "debian" {

  family = "debian-12"

  project = "debian-cloud"

}
```

Use:

```hcl
data.google_compute_image.debian.self_link
```

---

# Combining Resources and Data Sources

```hcl
data "aws_vpc" "default" {

  default = true

}

resource "aws_subnet" "public" {

  vpc_id = data.aws_vpc.default.id

  cidr_block = "10.0.1.0/24"

}
```

Terraform reads the existing VPC and creates a new subnet inside it.

---

# Common AWS Data Sources

Frequently used AWS data sources:

- `aws_ami`
- `aws_vpc`
- `aws_subnet`
- `aws_security_group`
- `aws_availability_zones`
- `aws_caller_identity`
- `aws_region`
- `aws_iam_role`
- `aws_route53_zone`
- `aws_kms_key`

---

# Data Source Workflow

```text
Existing Infrastructure
           │
           ▼
Terraform Data Source
           │
           ▼
Read Resource Information
           │
           ▼
Use Values in Resources
           │
           ▼
Deploy Infrastructure
```

---

# Easy Way to Remember

Think of a **library**.

```
Book Already Exists

↓

Read Book

↓

Do Not Rewrite It
```

Terraform Data Sources work the same way.

```
Resource Already Exists

↓

Read Information

↓

Use It
```

Data Sources **read** existing infrastructure—they **do not create** it.

---

# Best Practices

- Use Data Sources when infrastructure already exists.
- Avoid duplicating existing resources.
- Prefer filtering resources over hardcoding IDs when possible.
- Combine Data Sources with Resources for reusable configurations.
- Keep data source names descriptive.

---

# Common Mistakes

❌ Using a Data Source when a Resource should be created.

❌ Hardcoding resource IDs unnecessarily.

❌ Assuming Data Sources create infrastructure.

❌ Using incorrect filters that return no results.

❌ Forgetting to reference attributes with `data.<TYPE>.<NAME>.<ATTRIBUTE>`.

---

# Interview Questions

### What is a Terraform Data Source?

A Data Source retrieves information about existing infrastructure without creating or managing it.

---

### What is the difference between a Resource and a Data Source?

A **Resource** creates and manages infrastructure, while a **Data Source** only reads information about existing infrastructure.

---

### Which block is used for a Data Source?

```hcl
data
```

---

### How do you reference a Data Source?

```hcl
data.<TYPE>.<NAME>.<ATTRIBUTE>
```

Example:

```hcl
data.aws_vpc.default.id
```

---

### Can a Data Source modify existing resources?

No. Data Sources are read-only and cannot create, update, or delete resources.

---

### Why are Data Sources useful?

They allow Terraform to integrate with existing infrastructure, avoiding duplication and enabling reuse of resources such as VPCs, AMIs, IAM roles, and subnets.

---

# Summary

Terraform Data Sources provide a way to read information about existing infrastructure without managing it.

Key concepts include:

- `data` block
- Existing infrastructure
- Resource vs Data Source
- AWS, Azure, and Google Cloud examples
- Attribute references
- Reusable infrastructure

Using Data Sources effectively allows Terraform to work seamlessly with existing cloud environments while keeping configurations clean, reusable, and production-ready.