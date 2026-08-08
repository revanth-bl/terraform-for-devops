# 🎯 Terraform Scenario-Based Interview Questions

## 📖 Introduction

Scenario-based interview questions test your ability to apply Terraform concepts to real-world situations rather than simply recalling definitions or commands.

Interviewers use these questions to evaluate:

- Problem-solving skills
- Terraform workflow knowledge
- Cloud architecture understanding
- Troubleshooting ability
- Best practices
- Production experience

This guide covers some of the most common Terraform scenarios asked in DevOps and Cloud Engineer interviews.

---

# Scenario 1: Infrastructure Drift

### Question

A teammate manually changes an EC2 instance through the AWS Console. What happens when you run Terraform?

### Answer

Terraform compares the current infrastructure with the Terraform configuration and state.

When you run:

```bash
terraform plan
```

Terraform detects the difference (**infrastructure drift**) and shows the required changes.

The preferred solution is to manage infrastructure only through Terraform whenever possible.

---

# Scenario 2: State File Deleted

### Question

Someone accidentally deletes the local `terraform.tfstate` file. What should you do?

### Answer

If using a **remote backend**, restore the state from the remote storage.

If only a local state existed, recovery is difficult and may require importing existing resources:

```bash
terraform import
```

This is why production projects should always use remote state.

---

# Scenario 3: Two Engineers Run `terraform apply`

### Question

Two engineers run `terraform apply` at the same time. What prevents corruption?

### Answer

State locking.

Remote backends such as Amazon S3 (with DynamoDB locking), Azure Storage, Google Cloud Storage, and Terraform Cloud prevent multiple users from modifying the state simultaneously.

---

# Scenario 4: Wrong Resource Will Be Deleted

### Question

You run:

```bash
terraform plan
```

Terraform shows that an important production database will be destroyed.

What should you do?

### Answer

Do **not** run `terraform apply`.

Review:

- Recent code changes
- Variables
- Modules
- State file
- Resource dependencies

Identify why Terraform plans to destroy the resource before making any changes.

---

# Scenario 5: Deployment Fails

### Question

Your deployment fails immediately because of a Terraform configuration error.

### Answer

Run:

```bash
terraform fmt
```

```bash
terraform validate
```

Then fix any reported syntax or configuration issues before retrying.

---

# Scenario 6: Secret in GitHub

### Question

A teammate accidentally commits AWS credentials into GitHub.

What should you do?

### Answer

Immediately:

- Remove the credentials from the repository.
- Rotate the compromised AWS access keys.
- Update Terraform to use secure secret management.
- Review repository history if necessary.
- Prevent future leaks using secret scanning and CI/CD checks.

Secrets should never be hardcoded.

---

# Scenario 7: Team Collaboration

### Question

Five engineers are working on the same Terraform project.

How should the project be managed?

### Answer

Recommended approach:

- Remote backend
- State locking
- Git version control
- Pull requests
- Code reviews
- CI/CD pipeline
- Shared modules

This reduces conflicts and improves collaboration.

---

# Scenario 8: Multiple Environments

### Question

Your company has Development, Testing, and Production environments.

How should Terraform manage them?

### Answer

Use one of the following approaches:

- Terraform Workspaces
- Separate environment directories
- Reusable modules
- Environment-specific variable files

This keeps environments isolated while reusing common infrastructure.

---

# Scenario 9: Repeated Code

### Question

You notice the same VPC configuration copied into multiple projects.

What should you do?

### Answer

Create a reusable Terraform module.

Example:

```text
modules/

└── vpc/
```

Reuse the module across projects instead of copying code.

---

# Scenario 10: Hardcoded Password

### Question

The database password is written directly in `main.tf`.

Why is this a problem?

### Answer

Hardcoded secrets can be exposed through source control, logs, or state files.

Use:

- Sensitive variables
- AWS Secrets Manager
- Azure Key Vault
- Google Secret Manager
- CI/CD secret stores

---

# Scenario 11: Large Project

### Question

Your Terraform project has over 2,000 lines in one `main.tf`.

How would you improve it?

### Answer

Split the configuration into logical files such as:

```text
provider.tf

variables.tf

outputs.tf

network.tf

compute.tf

database.tf

security.tf
```

Use reusable modules where appropriate.

---

# Scenario 12: Security Group Open to the Internet

### Question

A security group allows SSH from:

```text
0.0.0.0/0
```

Is this recommended?

### Answer

No.

Restrict SSH access to trusted IP addresses or use secure access methods such as VPNs or AWS Systems Manager Session Manager where appropriate.

Follow the principle of least privilege.

---

# Scenario 13: Unexpected Cloud Bill

### Question

Your AWS bill increases significantly after testing Terraform.

What might have happened?

### Answer

Possible causes:

- EC2 instances left running
- RDS databases still active
- EKS clusters not deleted
- Unused load balancers
- Elastic IP addresses
- Storage volumes
- Snapshots

Remove unnecessary resources using:

```bash
terraform destroy
```

when they are no longer needed.

---

# Scenario 14: Provider Upgrade

### Question

Your project suddenly starts failing after a provider update.

What is the likely cause?

### Answer

The latest provider version may include breaking changes.

Pin provider versions.

Example:

```hcl
version = "~> 5.0"
```

Upgrade intentionally and test before deploying.

---

# Scenario 15: CI/CD Deployment

### Question

Your company wants every Terraform deployment to be automated.

How would you build the pipeline?

### Answer

Typical workflow:

```text
Git Push

↓

terraform fmt

↓

terraform validate

↓

Security Scan

↓

terraform plan

↓

Manual Approval (Production)

↓

terraform apply
```

This improves consistency and reduces manual errors.

---

# Scenario 16: Existing AWS Resources

### Question

Your company already has infrastructure created manually. How can Terraform manage it?

### Answer

Import existing resources into Terraform state.

Example:

```bash
terraform import
```

After importing, update the Terraform configuration so it accurately represents the existing infrastructure.

---

# Scenario 17: Team Accidentally Edits State File

### Question

A teammate manually edits the `terraform.tfstate` file.

What are the risks?

### Answer

Manual changes can corrupt the state, causing incorrect plans, deployment failures, or loss of resource tracking.

Terraform state should only be modified using Terraform commands when necessary.

---

# Scenario 18: Production Deployment

### Question

Before deploying to production, what checks should you perform?

### Answer

- Run `terraform fmt`
- Run `terraform validate`
- Execute security scans (Checkov, tfsec)
- Review `terraform plan`
- Verify variables
- Confirm provider versions
- Ensure remote state is healthy
- Obtain required approvals

---

# Quick Revision Table

| Scenario | Recommended Solution |
|----------|----------------------|
| Infrastructure drift | Run `terraform plan` and reconcile changes |
| Missing state | Restore remote state or import resources |
| Concurrent applies | Use state locking |
| Hardcoded secrets | Use secret management |
| Repeated code | Create reusable modules |
| Large project | Split into logical files and modules |
| Unexpected costs | Remove unused resources |
| Provider issues | Pin provider versions |
| Existing infrastructure | Use `terraform import` |
| Team collaboration | Remote backend + Git + CI/CD |

---

# Interview Tips

- Explain **why** you choose a solution, not just **what** command you would run.
- Mention security, scalability, and maintainability whenever relevant.
- Relate answers to real-world projects if you have experience.
- Emphasize Infrastructure as Code best practices.

---

# Summary

Scenario-based questions assess how you apply Terraform in practical situations. Strong answers demonstrate technical knowledge, structured thinking, and an understanding of production best practices.

Key topics include:

- Infrastructure Drift
- Remote State
- State Locking
- Modules
- Security
- Secret Management
- CI/CD
- Team Collaboration
- Cost Optimization
- Production Deployments
- `terraform import`
- `terraform plan`
- `terraform apply`

Mastering these scenarios will prepare you for the majority of Terraform interview questions asked in DevOps, Cloud Engineer, and Site Reliability Engineer (SRE) roles. 1 2 3 4 5