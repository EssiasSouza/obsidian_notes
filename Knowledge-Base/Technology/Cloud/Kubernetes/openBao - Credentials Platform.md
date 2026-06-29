Source: #source/internet_resources 
Project: #project/devops 
Areas: #area/work
Subject: #subject/cloud 
Type: #type/learning 
Learning priority: #priority/P3 
Status: #status/learning 
Related: [[Cloud Computing]]


---
Openbao is a fork from HashiCorp Vault. The project now is supported by OpenSSF and Linux Foundation.

The tool solves problems as:

- Secrets in .env files
- Hardcoded passwords
- Shared credentials by teams
- Missing auditoring
- Manual rotation of keys

Principal resources:

#### Secure Secret Storage

Stores cryptographed secrets in backends as:

- Filesystem
- PostgreSQL
- Consul
- S3
- Kubernetes storage

#### Dynamics Secrets

Generates temporary credentials automatically

Example:

- A Kubernetes Pod asks access to the database
- Openbao creates a temporary user
- After a while it revokes automatically

#### Encryption as a Service

It cryptographs data without need to implement cryptograph on the application.

#### Identity-based Access

Access control based on identity:

- LDAP
- OIDC
- GitHub
- [[Kubernetes]]
- AppRole
- JWT
#### Audit logs

Everything is audited.

- Who has accessed to
- When has accessed to
- What secret was used

#### Kubernetes Integration

Is used together:

- [[External Secret Operator]]
- CSI Driver
- Sidecars
- GitOps

| Tool                        | Function                                   |
| --------------------------- | ------------------------------------------ |
| OpenBao                     | Secrets manager open source                |
| HashiCorp Vault             | Original product which was based           |
| External Secrets Operator   | Integrates Kubernetes with secret managers |
| AWS Secrets Manager         | Managed Secret Service                     |
| Google Cloud Secret Manager | Managed service by GCP                     |
#### Links:

- [OpenBao Docs](https://openbao.org/docs/?utm_source=chatgpt.com)
- [GitHub do OpenBao](https://github.com/openbao/openbao?utm_source=chatgpt.com)