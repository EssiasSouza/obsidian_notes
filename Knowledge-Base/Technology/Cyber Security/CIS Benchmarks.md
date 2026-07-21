Source: #source/internet_resources
Project: #project/cyber_security
Areas: #area/work
Subject: #subject/cis
Type: #type/article
Learning priority: #priority/P2 
Status: #status/learning 
Related: [[Cyber Security]]

---
# CIS Benchmarks: Understanding the Center for Internet Security and Why Benchmarks Matter

## Introduction

Organizations are constantly exposed to cybersecurity threats, making secure system configuration an essential part of any security program. While many products are delivered with functional default settings, these configurations are not always optimized for security.

To address this challenge, the **Center for Internet Security (CIS)** publishes internationally recognized security best practices that help organizations harden their infrastructure, applications, and cloud environments. Among its most valuable resources are the **CIS Benchmarks** and the **CIS Controls**.

Although these terms are often used together, they serve different purposes. Understanding their differences is fundamental for security professionals, cloud engineers, auditors, and compliance teams.

---

# What is the Center for Internet Security (CIS)?

The **Center for Internet Security (CIS)** is a nonprofit organization dedicated to improving cybersecurity readiness worldwide. It develops consensus-based security standards and best practices through collaboration with cybersecurity experts, government agencies, vendors, and industry professionals.

Its recommendations are widely adopted across both public and private sectors and are referenced by numerous security and compliance frameworks.

The two most recognized CIS publications are:

- **CIS Benchmarks**
    
- **CIS Controls**
    

Each addresses a different aspect of cybersecurity.

---

# What Are CIS Benchmarks?

A **CIS Benchmark** is a detailed technical guide that describes how to securely configure a specific operating system, application, database, cloud platform, or network device.

Instead of explaining _what_ security policies an organization should have, a benchmark explains _how_ to configure a technology securely.

Examples of technologies covered by CIS Benchmarks include:

- Microsoft Windows Server
- Ubuntu Linux
- Red Hat Enterprise Linux
- Kubernetes
- Docker
- PostgreSQL
- MySQL
- Oracle Database
- Apache HTTP Server
- NGINX
- Google Cloud Platform (GCP)
- Amazon Web Services (AWS)
- Microsoft Azure


Each benchmark contains hundreds of individual recommendations designed to reduce the attack surface and improve the overall security posture.

---

# What Does "Benchmark" Mean?

In cybersecurity, a **benchmark** is a reference standard used to evaluate whether a system has been configured according to recognized security best practices.

Rather than creating security settings from scratch, organizations compare their current configurations against the benchmark.

For example:

Current SSH configuration:

```text
PasswordAuthentication yes
```

CIS Benchmark recommendation:

```text
PasswordAuthentication no
```

Result:

**Non-compliant**

The benchmark provides an objective standard that organizations can use to measure compliance and identify configuration gaps.

---

# What Does a CIS Benchmark Include?

Each recommendation typically contains several sections, including:

- Description of the recommendation
- Security rationale
- Potential operational impact
- Audit procedure
- Remediation procedure
- References


For example, a recommendation might state:

**Recommendation**

> Ensure SSH PermitRootLogin is disabled.

**Audit**

```bash
grep PermitRootLogin /etc/ssh/sshd_config
```

**Expected Result**

```text
PermitRootLogin no
```

**Remediation**

Modify the SSH configuration file and restart the SSH service.

This standardized structure allows both system administrators and auditors to verify compliance consistently.

---

# CIS Benchmark Levels

Most CIS Benchmarks define two implementation levels.

## Level 1

Level 1 recommendations are considered safe for nearly all production environments.

They improve security without significantly affecting system functionality or application compatibility.

Typical Level 1 recommendations include:

- Enable logging
- Disable direct root login
- Enforce strong password policies
- Configure secure protocols
- Remove unnecessary services

Organizations beginning their security hardening journey usually start with Level 1.

---

## Level 2

Level 2 recommendations provide a higher level of security but may introduce compatibility issues or require additional operational planning.

Examples include:

- Restricting legacy cryptographic algorithms
- Enforcing stricter password policies
- Disabling older protocols
- Applying more restrictive authentication settings

Level 2 is generally recommended for environments with elevated security requirements.

---

# What Are CIS Controls?

Unlike CIS Benchmarks, which focus on technical configuration, **CIS Controls** focus on organizational cybersecurity practices.

The CIS Controls define **what security capabilities an organization should implement** rather than how to configure individual systems.

Examples include:

- Asset Inventory and Control
- Vulnerability Management
- Identity and Access Management
- Data Protection
- Security Monitoring
- Incident Response
- Security Awareness Training
- Backup and Recovery

The current version contains **18 security controls**, each organized into safeguards that help organizations build a comprehensive cybersecurity program.

---

# CIS Benchmarks vs. CIS Controls

Although closely related, these resources address different security objectives.

|CIS Benchmarks|CIS Controls|
|---|---|
|Technical configuration guidance|Organizational cybersecurity framework|
|Focus on individual technologies|Focus on enterprise security processes|
|Explain how to securely configure systems|Explain which security capabilities organizations should implement|
|Used primarily by system administrators, cloud engineers, and auditors|Used primarily by security managers, CISOs, and governance teams|

In practice, organizations often implement both together.

The CIS Controls define the overall security strategy, while CIS Benchmarks provide the technical implementation guidance.

---

# CIS Benchmarks and PCI DSS

CIS Benchmarks are widely recognized as supporting evidence during **PCI DSS** assessments.

For example, PCI DSS Requirement **2.2** requires organizations to securely configure system components.

Although PCI DSS does not explicitly require the use of CIS Benchmarks, organizations commonly use them to demonstrate that their systems follow industry-accepted hardening practices.

Examples include applying:

- CIS Ubuntu Benchmark
- CIS Kubernetes Benchmark
- CIS PostgreSQL Benchmark
- CIS Google Cloud Platform Foundations Benchmark

Using these benchmarks helps auditors understand that secure configuration standards have been systematically applied.

---

# CIS Benchmarks in Google Cloud

For organizations running workloads on Google Cloud Platform, CIS Benchmarks play an important role in validating cloud security configurations.

The **CIS Google Cloud Platform Foundations Benchmark** defines best practices for securing cloud resources such as:

- Identity and Access Management (IAM)
- Cloud Logging
- Cloud Storage
- Virtual Private Cloud (VPC)
- Cloud SQL
- Networking
- Encryption
- Monitoring
- Organization Policies

Many findings generated by **Security Command Center** and **Security Health Analytics** correspond to recommendations found in the CIS GCP Foundations Benchmark.

Examples include:

- Publicly accessible Cloud Storage buckets
- Excessively permissive firewall rules
- Missing audit logging
- Weak IAM permissions
- Disabled security features
- Improper Cloud SQL configurations

Because of this alignment, organizations frequently use Security Command Center reports as a starting point for assessing CIS Benchmark compliance.

---

# Benefits of Using CIS Benchmarks

Organizations adopting CIS Benchmarks gain several advantages:

- Reduced attack surface
- Standardized security configurations
- Easier compliance with regulatory frameworks
- Consistent hardening across environments
- Improved audit readiness
- Better operational security
- Industry-recognized best practices

Since the benchmarks are developed through community consensus and continuously updated, they represent one of the most trusted sources of technical security guidance available today.

---

# Conclusion

The Center for Internet Security has become one of the world's leading sources of practical cybersecurity guidance.

While **CIS Controls** help organizations build a comprehensive cybersecurity program, **CIS Benchmarks** provide detailed technical instructions for securely configuring systems, applications, databases, and cloud platforms.

For cloud engineers, security professionals, and PCI DSS auditors, CIS Benchmarks serve as a trusted reference for evaluating security posture, implementing system hardening, and demonstrating compliance with recognized industry standards.

Whether securing an on premises data center, a Kubernetes cluster, or a Google Cloud environment, CIS Benchmarks provide a proven foundation for establishing secure and repeatable configurations that reduce risk and strengthen overall cybersecurity.

Donwload the documents:
[CIS Downloads](https://downloads.cisecurity.org/#/)