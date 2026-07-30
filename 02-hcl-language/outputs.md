# 📤 Terraform Outputs

## 📖 Introduction

**Outputs** are values that Terraform displays after successfully creating or updating infrastructure.

They allow you to:

- View important resource information.
- Share values between Terraform modules.
- Pass information to other tools and automation pipelines.
- Avoid manually looking up resource details in the cloud console.

For example, after creating an EC2 instance, Terraform can automatically display its **public IP address**, **instance ID**, or **DNS name**.

---

# Why Use Outputs?

Without outputs:

```
Create EC2 Instance

↓

Open AWS Console

↓

Search for EC2

↓

Copy Public IP
```

With outputs:

```bash
terraform apply
```

Terraform automatically prints:

```
instance_ip = 54.210.12.34
```

Much faster and more convenient.

---

# Syntax

```hcl
output "output_name" {
  value = value_to_display
}
```

Example:

```hcl
output "instance_id" {
  value = aws_instance.web.id
}
```

---

# Simple Output

```hcl
resource "aws_instance" "web" {

  ami           = "ami-123456789"

  instance_type = "t2.micro"

}

output "instance_id" {
  value = aws_instance.web.id
}
```

After running:

```bash
terraform apply
```

Output:

```
instance_id = i-0123456789abcdef0
```

---

# Output Multiple Values

```hcl
output "instance_public_ip" {
  value = aws_instance.web.public_ip
}

output "instance_private_ip" {
  value = aws_instance.web.private_ip
}

output "instance_dns" {
  value = aws_instance.web.public_dns
}
```

Terraform displays:

```
instance_public_ip = 54.210.12.34

instance_private_ip = 10.0.1.25

instance_dns = ec2-54-210-12-34.compute.amazonaws.com
```

---

# Output Using Variables

```hcl
variable "environment" {
  default = "dev"
}

output "environment" {
  value = var.environment
}
```

Result:

```
environment = dev
```

---

# Output Using Locals

```hcl
locals {
  project = "terraform-demo"
}

output "project_name" {
  value = local.project
}
```

Output:

```
project_name = terraform-demo
```

---

# Output Maps

```hcl
output "instance_details" {

  value = {

    id = aws_instance.web.id

    ip = aws_instance.web.public_ip

    dns = aws_instance.web.public_dns

  }

}
```

Output:

```
instance_details = {

id = i-0123456789abcdef0

ip = 54.210.12.34

dns = ec2-54-210-12-34.compute.amazonaws.com

}
```

---

# Output Lists

```hcl
output "availability_zones" {
  value = [
    "us-east-1a",
    "us-east-1b",
    "us-east-1c"
  ]
}
```

Result:

```
[
  us-east-1a,
  us-east-1b,
  us-east-1c
]
```

---

# Sensitive Outputs

Some values should not be displayed openly.

Example:

```hcl
output "db_password" {

  value = aws_db_instance.mysql.password

  sensitive = true

}
```

Terraform Output:

```
db_password = (sensitive value)
```

This prevents secrets from being shown in terminal output.

---

# Output Descriptions

Outputs can include descriptions.

```hcl
output "instance_ip" {

  description = "Public IP address of the EC2 instance."

  value = aws_instance.web.public_ip

}
```

Descriptions improve readability and documentation.

---

# Accessing Outputs

Display all outputs:

```bash
terraform output
```

Example:

```
instance_ip = 54.210.12.34

instance_id = i-0123456789abcdef0
```

---

Display a specific output:

```bash
terraform output instance_ip
```

Result:

```
54.210.12.34
```

---

Display output as JSON:

```bash
terraform output -json
```

Useful for automation and scripting.

---

# Outputs Between Modules

Outputs allow child modules to expose values to parent modules.

### Child Module

```hcl
output "vpc_id" {
  value = aws_vpc.main.id
}
```

### Parent Module

```hcl
module "network" {
  source = "./modules/network"
}

output "network_vpc" {
  value = module.network.vpc_id
}
```

This enables modules to share information cleanly.

---

# Real-World Example

```hcl
resource "aws_instance" "web" {

  ami           = "ami-123456789"

  instance_type = "t2.micro"

}

output "instance_information" {

  value = {

    id         = aws_instance.web.id

    public_ip  = aws_instance.web.public_ip

    private_ip = aws_instance.web.private_ip

    dns        = aws_instance.web.public_dns

  }

}
```

After `terraform apply`:

```
instance_information = {

id = i-0123456789abcdef0

public_ip = 54.210.12.34

private_ip = 10.0.1.25

dns = ec2-54-210-12-34.compute.amazonaws.com

}
```

---

# Workflow

```text
Terraform Apply
        │
        ▼
Resources Created
        │
        ▼
Output Values Generated
        │
        ▼
Displayed in Terminal
        │
        ▼
Used by Users or Other Modules
```

---

# Easy Way to Remember

Imagine ordering food online.

```
Restaurant

↓

Prepares Order

↓

Delivery Driver

↓

You Receive Tracking Details
```

Terraform works similarly.

```
Terraform

↓

Creates Resources

↓

Outputs Important Information

↓

You Use It
```

Outputs are simply the important information Terraform gives you after completing its work.

---

# Best Practices

- Output only useful information.
- Mark secrets as `sensitive`.
- Add descriptions to outputs.
- Use outputs for module communication.
- Keep output names descriptive and consistent.

---

# Common Mistakes

❌ Outputting sensitive credentials without marking them as sensitive.

❌ Creating unnecessary outputs.

❌ Using unclear output names.

❌ Forgetting that outputs are primarily intended for users and other modules.

---

# Interview Questions

### What are Terraform outputs?

Outputs are values that Terraform displays after creating or updating infrastructure and can also be used to share information between modules.

---

### Why are outputs useful?

They provide quick access to important resource information without manually checking the cloud provider's console.

---

### How do you display all outputs?

```bash
terraform output
```

---

### How do you display a specific output?

```bash
terraform output <output_name>
```

Example:

```bash
terraform output instance_ip
```

---

### What does `sensitive = true` do?

It hides the output value from normal terminal output, helping protect sensitive information such as passwords or API keys.

---

### Can outputs be used between modules?

Yes. Child modules expose values using outputs, and parent modules can reference them using:

```hcl
module.<module_name>.<output_name>
```

---

# Summary

Terraform outputs provide a convenient way to retrieve and share important information after infrastructure is created.

They are commonly used to display:

- Resource IDs
- Public and private IP addresses
- DNS names
- ARNs
- URLs
- Values shared between modules

Using outputs effectively improves automation, simplifies infrastructure management, and makes Terraform configurations easier to integrate with other tools and workflows.