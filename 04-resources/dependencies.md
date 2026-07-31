# 🔗 Terraform Dependencies

## 📖 Introduction

A **dependency** is the relationship between Terraform resources that determines **the order in which resources are created, updated, or destroyed**.

Terraform automatically builds a **dependency graph** to understand which resources depend on others.

For example:

- A Subnet depends on a VPC.
- An EC2 instance depends on a Subnet.
- A Route Table Association depends on both the Route Table and the Subnet.

Terraform ensures that dependent resources are created in the correct order.

---

# Why Are Dependencies Important?

Imagine creating an EC2 instance before its VPC exists.

```
EC2 Instance

↓

Needs VPC

↓

VPC Doesn't Exist

↓

Deployment Fails
```

Terraform prevents this by creating the VPC first.

---

# Types of Dependencies

Terraform supports two types of dependencies:

1. **Implicit Dependencies**
2. **Explicit Dependencies**

---

# Implicit Dependencies

An **implicit dependency** is automatically detected when one resource references another.

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

Terraform sees:

```text
aws_subnet.public
        │
        ▼
aws_vpc.main
```

The subnet cannot be created until the VPC exists.

No extra configuration is needed.

---

# How Terraform Detects Dependencies

Terraform looks for references such as:

```hcl
aws_vpc.main.id
```

or

```hcl
module.network.vpc_id
```

or

```hcl
var.region
```

These references help Terraform build its dependency graph.

---

# Example: EC2 Depends on Subnet

```hcl
resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = "t2.micro"

  subnet_id = aws_subnet.public.id

}
```

Dependency chain:

```text
VPC
 │
 ▼
Subnet
 │
 ▼
EC2 Instance
```

Terraform creates them in exactly this order.

---

# Explicit Dependencies

Sometimes Terraform cannot automatically detect a dependency.

In such cases, use:

```hcl
depends_on
```

Example:

```hcl
resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = "t2.micro"

  depends_on = [

    aws_security_group.web

  ]

}
```

Terraform ensures the Security Group is created first.

---

# When to Use `depends_on`

Use `depends_on` only when Terraform cannot infer the dependency automatically.

Common scenarios:

- Provisioners
- External resources
- IAM policies
- Cloud service propagation delays
- Null resources
- Modules with hidden relationships

---

# Example: IAM Role Before EC2

```hcl
resource "aws_iam_role" "ec2_role" {

  name = "ec2-role"

  assume_role_policy = data.aws_iam_policy_document.ec2.json

}

resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = "t2.micro"

  depends_on = [

    aws_iam_role.ec2_role

  ]

}
```

Terraform waits until the IAM Role is created.

---

# Dependencies Between Modules

Modules can also depend on one another.

```hcl
module "network" {

  source = "./modules/network"

}

module "compute" {

  source = "./modules/compute"

  vpc_id = module.network.vpc_id

}
```

Terraform automatically understands:

```text
Network Module
       │
       ▼
Compute Module
```

---

# Module `depends_on`

Modules also support explicit dependencies.

```hcl
module "compute" {

  source = "./modules/compute"

  depends_on = [

    module.network

  ]

}
```

---

# Dependency Graph

Terraform internally creates a graph like this:

```text
VPC
 │
 ▼
Subnet
 │
 ▼
Route Table
 │
 ▼
Security Group
 │
 ▼
EC2 Instance
```

This graph determines the execution order.

---

# Viewing the Dependency Graph

Generate the dependency graph:

```bash
terraform graph
```

You can visualize it using Graphviz:

```bash
terraform graph | dot -Tpng > graph.png
```

This creates an image of the infrastructure dependency graph.

---

# Resource Creation Order

Terraform follows this sequence:

```text
Read Configuration
        │
        ▼
Build Dependency Graph
        │
        ▼
Determine Execution Order
        │
        ▼
Create Resources
```

---

# Resource Destruction Order

Terraform destroys resources in the reverse order.

Example:

Creation:

```text
VPC

↓

Subnet

↓

EC2
```

Destruction:

```text
EC2

↓

Subnet

↓

VPC
```

This prevents dependency-related errors.

---

# Real-World Example

```hcl
resource "aws_vpc" "main" {

  cidr_block = "10.0.0.0/16"

}

resource "aws_subnet" "public" {

  vpc_id = aws_vpc.main.id

  cidr_block = "10.0.1.0/24"

}

resource "aws_instance" "web" {

  ami = "ami-123456789"

  instance_type = "t2.micro"

  subnet_id = aws_subnet.public.id

}
```

Terraform automatically creates:

```text
VPC

↓

Subnet

↓

EC2
```

No `depends_on` is needed because the references create implicit dependencies.

---

# Easy Way to Remember

Think about building a house.

```
Foundation

↓

Walls

↓

Roof

↓

Paint
```

You cannot paint the house before the walls exist.

Terraform works the same way.

```
VPC

↓

Subnet

↓

EC2

↓

Application
```

Each resource waits for the resources it depends on.

---

# Best Practices

- Prefer **implicit dependencies** whenever possible.
- Use `depends_on` only when necessary.
- Avoid unnecessary explicit dependencies.
- Organize resources logically.
- Keep dependency chains simple and easy to understand.

---

# Common Mistakes

❌ Using `depends_on` when Terraform already detects the dependency.

❌ Creating circular dependencies between resources.

❌ Assuming Terraform creates resources in the order they appear in the file.

❌ Forgetting that resource destruction occurs in reverse dependency order.

---

# Interview Questions

### What is a Terraform dependency?

A dependency defines the relationship between resources and determines the order in which Terraform creates, updates, and destroys them.

---

### What are the two types of dependencies?

- Implicit dependencies
- Explicit dependencies

---

### What is an implicit dependency?

An implicit dependency is automatically detected when one resource references another.

Example:

```hcl
subnet_id = aws_subnet.public.id
```

---

### What is `depends_on`?

`depends_on` is an argument used to explicitly define a dependency when Terraform cannot determine it automatically.

---

### Which dependency type is preferred?

Implicit dependencies are preferred because they make configurations cleaner and easier to maintain.

---

### What command displays the dependency graph?

```bash
terraform graph
```

---

# Summary

Terraform dependencies ensure that infrastructure is created and destroyed in the correct order.

Key concepts include:

- Implicit dependencies
- Explicit dependencies
- `depends_on`
- Dependency graph
- Module dependencies
- Resource creation order
- Resource destruction order

Understanding dependencies is essential for building reliable, predictable, and production-ready Terraform infrastructure.