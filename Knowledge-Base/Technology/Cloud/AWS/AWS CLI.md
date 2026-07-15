Source: #source/internet_resources 
Project: #project/devops 
Areas: #area/work
Subject: #subject/cloud
Type: #type/learning
Learning priority: #priority/P1 
Status: #status/learning 
Related: [[Cloud Computing]]
[[AWS]]

---

# Prerequisites

Before using the AWS CLI, make sure it is installed.

Check your installation:

```bash
aws --version
```

Example:

```text
aws-cli/2.27.0 Python/3.13.0 Windows/11 exe/x86_64
```

---

# Configure your first AWS account

If this is your first time using the AWS CLI, run:

```bash
aws configure
```

You will be prompted for:

* AWS Access Key ID
* AWS Secret Access Key
* Default AWS Region (for example: `us-east-1`)
* Default output format (`json`, `yaml`, or `text`)

Example:

```text
AWS Access Key ID [None]: AKIA...
AWS Secret Access Key [None]: ****************
Default region name [None]: us-east-1
Default output format [None]: json
```

This creates the **default profile**.

---

# Create additional AWS profiles

If you work with multiple AWS accounts, create a named profile.

Example:

```bash
aws configure --profile development
```

Or:

```bash
aws configure --profile production
```

Repeat the process for every AWS account you need.

Example:

```text
default
development
production
sandbox
```

---
# List configured profiles

Display all configured profiles.

```bash
aws configure list-profiles
```

Example:

```text
default
development
production
sandbox
```

---
# Switching Profiles

```bash
aws configure list-profiles
```

---

# Authenticate using AWS IAM Identity Center (AWS SSO)

If your organization uses AWS IAM Identity Center (formerly AWS SSO), configure a profile with:

```bash
aws configure sso
```

You will be asked for information such as:

* SSO Start URL
* SSO Region
* AWS Account
* IAM Role
* Profile name

After the profile is created, authenticate by running:

```bash
aws sso login --profile development
```

This opens your browser so you can complete the login.

To log out:

```bash
aws sso logout
```

---


# Check who is currently authenticated

Display the identity currently being used.

```bash
aws sts get-caller-identity
```

Example:

```json
{
  "UserId": "AIDAXXXXXXXXXXXXX",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/john"
}
```

If using AWS SSO or AssumeRole, the ARN will indicate the role instead of an IAM user.

---

# Switch between AWS accounts

## Run a single command with another profile

```bash
aws s3 ls --profile production
```

---

## Change the active profile for your terminal

### Linux/macOS

```bash
export AWS_PROFILE=production
```

### Windows PowerShell

```powershell
$env:AWS_PROFILE="production"
```

Verify the active account:

```bash
aws sts get-caller-identity
```

---

# Check your current AWS CLI configuration

```bash
aws configure list
```

Example:

```text
      Name                    Value             Type    Location
      ----                    -----             ----    --------
   profile                default              manual    --profile
access_key     ****************ABCD shared-credentials-file
secret_key     ****************WXYZ shared-credentials-file
    region               us-east-1      config-file
```

---

# View the current region

```bash
aws configure get region
```

Or:

### Linux/macOS

```bash
echo $AWS_REGION
```

### Windows PowerShell

```powershell
$env:AWS_REGION
```

---

# List Amazon S3 buckets

```bash
aws s3 ls
```

---

# List EC2 instances

```bash
aws ec2 describe-instances
```

---

# List IAM users

Requires appropriate permissions.

```bash
aws iam list-users
```

---

# List available AWS regions

```bash
aws ec2 describe-regions
```

---

# Verify your AWS CLI version

```bash
aws --version
```

---

# Get help

General help:

```bash
aws help
```

Help for a service:

```bash
aws ec2 help
```

Help for a specific command:

```bash
aws ec2 describe-instances help
```

---

# Useful File Locations

The AWS CLI stores your configuration in two files.

### Linux/macOS

```text
~/.aws/config
~/.aws/credentials
```

### Windows

```text
C:\Users\<username>\.aws\config
C:\Users\<username>\.aws\credentials
```

---

# Quick Reference

| Task                       | Command                                                    |
| -------------------------- | ---------------------------------------------------------- |
| Configure first account    | `aws configure`                                            |
| Configure another profile  | `aws configure --profile <name>`                           |
| Configure AWS SSO          | `aws configure sso`                                        |
| Login with AWS SSO         | `aws sso login --profile <name>`                           |
| Logout from AWS SSO        | `aws sso logout`                                           |
| List profiles              | `aws configure list-profiles`                              |
| Check current identity     | `aws sts get-caller-identity`                              |
| Use another profile        | `aws <command> --profile <name>`                           |
| Set active profile         | `export AWS_PROFILE=<name>` or `$env:AWS_PROFILE="<name>"` |
| Show current configuration | `aws configure list`                                       |
| Show configured region     | `aws configure get region`                                 |
| List S3 buckets            | `aws s3 ls`                                                |
| List EC2 instances         | `aws ec2 describe-instances`                               |
| List AWS regions           | `aws ec2 describe-regions`                                 |
| List IAM users             | `aws iam list-users`                                       |
| Show CLI version           | `aws --version`                                            |
| Get help                   | `aws help`                                                 |

This guide covers the commands most AWS users interact with during their first days using the AWS CLI, while introducing good practices for managing multiple AWS accounts through profiles.
