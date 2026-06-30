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

# AWS CLI Quick Start for Beginners

This guide covers the most common AWS CLI commands for beginners.

## Check who is logged in

Display the current AWS identity.

```bash
aws sts get-caller-identity
```

Example output:

```json
{
  "UserId": "AIDAXXXXXXXXXXXXX",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/john"
}
```

## List configured profiles

Show all AWS CLI profiles configured on your computer.

```bash
aws configure list-profiles
```

Example:

```text
default
development
production
```

## Switch to another account (profile)

Use a different profile for a single command:

```bash
aws s3 ls --profile development
```

Or set the profile for your current terminal session.

### Linux/macOS

```bash
export AWS_PROFILE=development
```

### Windows PowerShell

```powershell
$env:AWS_PROFILE="development"
```

Verify the active account:

```bash
aws sts get-caller-identity
```

## Check the current configuration

Display where your credentials and region are coming from.

```bash
aws configure list
```

## Configure a new profile

Create a new AWS CLI profile.

```bash
aws configure --profile development
```

You will be prompted for:

- Access Key ID
- Secret Access Key
- Default Region
- Output format (json, yaml, or text)

## List S3 buckets

```bash
aws s3 ls
```

## List EC2 instances

```bash
aws ec2 describe-instances
```

## Check the configured region

### Linux/macOS

```bash
echo $AWS_REGION
```

### Windows PowerShell

```powershell
$env:AWS_REGION
```

Or check the CLI configuration:

```bash
aws configure get region
```

## View the AWS CLI version

```bash
aws --version
```

Example:

```text
aws-cli/2.27.0 Python/3.13.0 Windows/11 exe/x86_64
```

## Get help

General help:

```bash
aws help
```

Help for a specific service:

```bash
aws s3 help
```

Help for a specific command:

```bash
aws ec2 describe-instances help
```

## Common Beginner Commands

|Task|Command|
|---|---|
|Check current identity|`aws sts get-caller-identity`|
|List profiles|`aws configure list-profiles`|
|View current configuration|`aws configure list`|
|Create a new profile|`aws configure --profile <profile-name>`|
|List S3 buckets|`aws s3 ls`|
|List EC2 instances|`aws ec2 describe-instances`|
|Check CLI version|`aws --version`|
|Get help|`aws help`|