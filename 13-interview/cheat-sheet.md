# 📄 Terraform Cheat Sheet

## 🚀 Terraform Workflow

```bash
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
terraform destroy
```

---

# 📦 Initialization

Initialize the working directory.

```bash
terraform init
```

Upgrade providers.

```bash
terraform init -upgrade
```

Reconfigure backend.

```bash
terraform init -reconfigure
```

---

# ✍️ Formatting

Format Terraform files.

```bash
terraform fmt
```

Format recursively.

```bash
terraform fmt -recursive
```

Check formatting only.

```bash
terraform fmt -check
```

---

# ✅ Validation

Validate configuration.

```bash
terraform validate
```

---

# 📋 Planning

Preview infrastructure changes.

```bash
terraform plan
```

Save execution plan.

```bash
terraform plan -out=tfplan
```

Use variable file.

```bash
terraform plan -var-file="terraform.tfvars"
```

---

# 🚀 Apply Changes

Apply infrastructure.

```bash
terraform apply
```

Apply saved plan.

```bash
terraform apply tfplan
```

Auto approve.

```bash
terraform apply -auto-approve
```

---

# ❌ Destroy Infrastructure

Destroy everything.

```bash
terraform destroy
```

Skip confirmation.

```bash
terraform destroy -auto-approve
```

---

# 📂 Workspace Commands

Show current workspace.

```bash
terraform workspace show
```

List workspaces.

```bash
terraform workspace list
```

Create workspace.

```bash
terraform workspace new dev
```

Switch workspace.

```bash
terraform workspace select dev
```

Delete workspace.

```bash
terraform workspace delete dev
```

---

# 📊 State Commands

Show state resources.

```bash
terraform state list
```

Show resource details.

```bash
terraform state show RESOURCE_NAME
```

Move resource.

```bash
terraform state mv
```

Remove resource from state.

```bash
terraform state rm
```

Pull remote state.

```bash
terraform state pull
```

Push state.

```bash
terraform state push
```

---

# 📤 Output Commands

Show outputs.

```bash
terraform output
```

Show specific output.

```bash
terraform output public_ip
```

Output in JSON.

```bash
terraform output -json
```

---

# 🔍 Console

Open Terraform console.

```bash
terraform console
```

Exit:

```bash
exit
```

---

# 🔒 Provider Commands

View providers.

```bash
terraform providers
```

Lock providers.

```bash
terraform providers lock
```

---

# 📦 Module Commands

Download modules.

```bash
terraform get
```

Upgrade modules.

```bash
terraform get -update
```

---

# 🔐 Terraform Login

Login to Terraform Cloud.

```bash
terraform login
```

Logout.

```bash
terraform logout
```

---

# 🌍 Environment Variables

AWS:

```bash
export AWS_ACCESS_KEY_ID=xxxxxxxx

export AWS_SECRET_ACCESS_KEY=xxxxxxxx

export AWS_DEFAULT_REGION=us-east-1
```

Windows PowerShell:

```powershell
$env:AWS_ACCESS_KEY_ID="xxxxxxxx"

$env:AWS_SECRET_ACCESS_KEY="xxxxxxxx"

$env:AWS_DEFAULT_REGION="us-east-1"
```

---

# 📁 Important Terraform Files

```text
main.tf

provider.tf

versions.tf

variables.tf

terraform.tfvars

locals.tf

outputs.tf

terraform.tfstate

terraform.lock.hcl
```

---

# 🏗️ Common Resource Types

```hcl
resource "aws_instance"

resource "aws_vpc"

resource "aws_subnet"

resource "aws_security_group"

resource "aws_s3_bucket"

resource "aws_db_instance"

resource "aws_lambda_function"

resource "aws_eks_cluster"

resource "aws_iam_role"
```

---

# 📚 Common Blocks

Provider

```hcl
provider "aws" {}
```

Variable

```hcl
variable "name" {}
```

Output

```hcl
output "id" {}
```

Local

```hcl
locals {}
```

Data Source

```hcl
data "aws_ami" "ubuntu" {}
```

Module

```hcl
module "vpc" {}
```

---

# 🔄 Terraform Lifecycle

```text
Write Code

↓

terraform init

↓

terraform fmt

↓

terraform validate

↓

terraform plan

↓

terraform apply

↓

Infrastructure Created

↓

terraform destroy
```

---

# ⚡ Useful AWS CLI Commands

Check credentials.

```bash
aws sts get-caller-identity
```

Configure AWS CLI.

```bash
aws configure
```

List EC2 instances.

```bash
aws ec2 describe-instances
```

List S3 buckets.

```bash
aws s3 ls
```

---

# 🛡️ Security Checklist

✅ Use variables

✅ Never hardcode secrets

✅ Store state remotely

✅ Use least privilege IAM

✅ Run Checkov

✅ Run tfsec

✅ Review `terraform plan`

---

# 📁 Best Practices

✅ Use modules

✅ Use remote state

✅ Tag resources

✅ Use workspaces

✅ Version control Terraform code

✅ Keep providers updated

✅ Organize projects logically

---

# 🎯 Interview Quick Facts

| Question | Answer |
|----------|--------|
| Infrastructure as Code | Managing infrastructure using code |
| Configuration file | `.tf` |
| State file | `terraform.tfstate` |
| Initialize project | `terraform init` |
| Format code | `terraform fmt` |
| Validate configuration | `terraform validate` |
| Preview changes | `terraform plan` |
| Deploy infrastructure | `terraform apply` |
| Delete infrastructure | `terraform destroy` |
| Variables file | `terraform.tfvars` |
| Output values | `output` block |
| Reusable code | Modules |
| Cloud plugins | Providers |
| Remote state | S3, Azure Storage, GCS, Terraform Cloud |
| Security tools | Checkov, tfsec |

---

# 🧠 Easy Memory Trick

Remember this sequence:

```text
INIT

↓

FMT

↓

VALIDATE

↓

PLAN

↓

APPLY

↓

DESTROY
```

Or simply:

> **"I Formally Validate Plans Before Deploying."**

---

# 📌 One-Line Summary

**Terraform automates cloud infrastructure using Infrastructure as Code (IaC), enabling you to provision, manage, and destroy resources safely through version-controlled configuration files.**