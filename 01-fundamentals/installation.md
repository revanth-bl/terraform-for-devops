# 💻 Installing Terraform

## 📖 Introduction

Before creating infrastructure with Terraform, you need to install the Terraform CLI on your operating system.

Once installed, you can use commands like:

```bash
terraform init
terraform plan
terraform apply
terraform destroy
```

Terraform is a single executable binary, making installation simple and lightweight.

---

# System Requirements

- Windows 10/11
- Linux (Ubuntu, Debian, CentOS, Fedora, etc.)
- macOS
- Internet connection (to download providers)

---

# Install on Windows

## Method 1 – Using Winget (Recommended)

Open **PowerShell** as Administrator and run:

```powershell
winget install Hashicorp.Terraform
```

Verify the installation:

```powershell
terraform version
```

Example Output:

```text
Terraform v1.x.x
on windows_amd64
```

---

## Method 2 – Manual Installation

### Step 1

Download Terraform from the official website.

https://developer.hashicorp.com/terraform/downloads

---

### Step 2

Extract the ZIP file.

Example:

```
terraform.exe
```

---

### Step 3

Move `terraform.exe` to a folder such as:

```
C:\Terraform
```

---

### Step 4

Add the folder to the Windows **PATH** environment variable.

---

### Step 5

Open a new terminal and verify:

```powershell
terraform version
```

---

# Install on Ubuntu / Debian

Update package lists:

```bash
sudo apt update
```

Install required packages:

```bash
sudo apt install -y gnupg software-properties-common curl
```

Add the HashiCorp GPG key:

```bash
wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg >/dev/null
```

Add the official repository:

```bash
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list
```

Update package lists again:

```bash
sudo apt update
```

Install Terraform:

```bash
sudo apt install terraform
```

Verify:

```bash
terraform version
```

---

# Install on Fedora

```bash
sudo dnf install dnf-plugins-core
```

```bash
sudo dnf config-manager addrepo \
https://rpm.releases.hashicorp.com/fedora/hashicorp.repo
```

```bash
sudo dnf install terraform
```

Verify:

```bash
terraform version
```

---

# Install on CentOS / RHEL

```bash
sudo yum install -y yum-utils
```

```bash
sudo yum-config-manager --add-repo \
https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
```

```bash
sudo yum install terraform
```

Verify:

```bash
terraform version
```

---

# Install on macOS

Using Homebrew:

```bash
brew tap hashicorp/tap
```

```bash
brew install hashicorp/tap/terraform
```

Verify:

```bash
terraform version
```

---

# Verify Installation

Run:

```bash
terraform version
```

Expected output:

```text
Terraform v1.x.x
```

You can also check where Terraform is installed.

Windows:

```powershell
where terraform
```

Linux/macOS:

```bash
which terraform
```

---

# Enable Command Auto-Completion (Optional)

### Bash

```bash
terraform -install-autocomplete
```

Restart your terminal.

---

### PowerShell

```powershell
terraform -install-autocomplete
```

Restart PowerShell.

---

# Update Terraform

### Windows (Winget)

```powershell
winget upgrade Hashicorp.Terraform
```

---

### Ubuntu

```bash
sudo apt update
sudo apt upgrade terraform
```

---

### macOS

```bash
brew upgrade terraform
```

---

# Uninstall Terraform

### Windows

Using Winget:

```powershell
winget uninstall Hashicorp.Terraform
```

Or delete `terraform.exe` and remove it from the PATH.

---

### Ubuntu

```bash
sudo apt remove terraform
```

---

### macOS

```bash
brew uninstall terraform
```

---

# Common Installation Errors

## ❌ terraform: command not found

**Cause**

Terraform is not installed or is not in the system PATH.

**Solution**

- Verify installation.
- Add Terraform to the PATH.
- Restart the terminal.

---

## ❌ terraform is not recognized as an internal or external command

**Cause**

Windows cannot locate `terraform.exe`.

**Solution**

- Ensure the installation folder is added to the PATH.
- Restart PowerShell or Command Prompt.

---

## ❌ Permission denied

**Cause**

Insufficient permissions during installation.

**Solution**

Run the installation command with administrator/root privileges.

---

# Best Practices

- Install Terraform from the official HashiCorp repository.
- Keep Terraform updated.
- Verify the version after installation.
- Avoid downloading binaries from unofficial websites.
- Use version control for your Terraform configuration files.

---

# Interview Questions

### What is the easiest way to install Terraform on Windows?

Using:

```powershell
winget install Hashicorp.Terraform
```

---

### How do you verify that Terraform is installed?

```bash
terraform version
```

---

### Why is Terraform often distributed as a single binary?

Because it is lightweight, portable, and easy to install without additional dependencies.

---

### What should you do if `terraform` is not recognized?

Ensure Terraform is installed correctly, added to the system PATH, and restart the terminal.

---

# Summary

After completing this guide, you should be able to:

- Install Terraform on Windows, Linux, or macOS.
- Verify the installation.
- Enable command auto-completion.
- Update or uninstall Terraform.
- Troubleshoot common installation issues.

You're now ready to start writing your first Terraform configuration.