Source: #source/internet_resources 
Project: #project/devops
Areas: #area/work
Subject: #subject/kubernetes
Type: #type/learning 
Learning priority: #priority/P1 
Status: #status/to_learning 
Related: [[Cloud Computing]]
[[Kubernetes Roadmap]]
[[GKE - Google Kubernetes Engine]]
[[Courses Platforms]]

---

[Introduction - External Secrets Operator](https://external-secrets.io/latest/)

The external secret operator is a [[Kubernetes Roadmap]] operator that synchronizes secrets from external secret management systems into native [[Kubernetes Roadmap]]

With ESO we can use AWS Secret Manager, Google Secret Manager, HashiCorp Vault, Azure Key Vault, etc.

---
# GKE Installing External Secrets Operator

To install the `External Secrets Operator` on your GKE environment you need to access:

👉👉👉 [[GKE - Installing External Secrets Operator]] tutorial.

---
# Expected Result

Kubernetes will now perform:

```text id="s5m1pq"
Kubernetes -> Artifact Registry -> ALLOWED
```

And the Pods should start successfully.

Validate with:

```bash id="n8r4wc"
kubectl get pods -n external-secrets
```

---

# Concepts Learned

During this process, several important enterprise Kubernetes concepts were covered:

* Helm
* CRDs
* OCI Images
* Registries
* Artifact Registry
* Cloud Build
* Binary Authorization
* Supply Chain Security
* Image Promotion
* GKE Autopilot
* Container Security
* Private Registries
* `crane`

---

# Final Architecture

```text id="c2v9mx"
Internet (GHCR)
        ↓
Cloud Build + crane
        ↓
Internal Artifact Registry
        ↓
Kubernetes (GKE)
```

This is a very common pattern in modern enterprise Kubernetes environments.

---
# Next Evolution (Production Grade)

After mastering this lab, move to:

## Phase 2

Replace static AWS credentials with:

- GKE Workload Identity Federation
- AWS OIDC federation
- AssumeRoleWithWebIdentity

---

# Recommended Next Topics

## Immediate Next Step

Learn:

- `SecretStore` vs `ClusterSecretStore`

## Then

Learn:

- IAM least privilege
- IRSA concepts
- OIDC federation
- ArgoCD integration
- Stakater Reloader

---
# Self documentation

# GKE + External Secrets Operator (ESO) + AWS Secrets Manager

## Overview

This document describes how the integration between Google Kubernetes Engine (GKE), External Secrets Operator (ESO), and AWS Secrets Manager (AWS SM) works.

The purpose of this integration is to centralize secret management in AWS Secrets Manager while allowing Kubernetes workloads running in GKE to consume those secrets through native Kubernetes Secrets.

---

# Architecture

## Components

### GKE

Responsible for running Kubernetes workloads and applications.

### External Secrets Operator (ESO)

External Secrets Operator

ESO continuously synchronizes secrets stored in AWS Secrets Manager into Kubernetes Secrets.

It acts as a bridge between Kubernetes and AWS Secrets Manager.

### AWS Secrets Manager

AWS Secrets Manager

Centralized secret storage solution used as the source of truth for credentials, tokens, API keys, certificates, and sensitive values.

---

# Secret Lifecycle

## Secret Creation

All secrets must be created in AWS Secrets Manager through an internal request/ticket process.

The developer does not create secrets directly in Kubernetes.

After the secret is created in AWS Secrets Manager, the developer only needs to create the corresponding `ExternalSecret` manifest in Kubernetes.

---

# Naming Convention

The following naming convention must be used for secrets stored in AWS Secrets Manager:

```text
/<cluster_name>/<namespace>/<secret_name>
```

## Example

```text
/prod-gke/payments/api-credentials
```

Where:

| Segment           | Description          |
| ----------------- | -------------------- |
| `prod-gke`        | Cluster name         |
| `payments`        | Kubernetes namespace |
| `api-credentials` | Secret name          |

---

# How ESO Works

## Synchronization Flow

The synchronization process works as follows:

1. A secret is created in AWS Secrets Manager
2. An `ExternalSecret` resource is created in Kubernetes
3. ESO reads the `ExternalSecret`
4. ESO authenticates against AWS Secrets Manager
5. ESO fetches the secret values
6. ESO creates or updates a native Kubernetes Secret
7. Applications consume the Kubernetes Secret normally

---

# Secret Synchronization Model

## Continuous Reconciliation

ESO continuously reconciles secrets.

This means:

* Changes made in AWS Secrets Manager are automatically synchronized
* The Kubernetes Secret is periodically updated
* Applications continue consuming Kubernetes-native secrets
* AWS Secrets Manager becomes the single source of truth

---

# Recommended Pattern: `dataFrom`

The recommended approach is using `dataFrom`.

This allows ESO to import all keys from a secret stored in AWS Secrets Manager automatically.

This is especially useful because:

* New keys added in AWS Secrets Manager are automatically synchronized
* Existing keys updated in AWS Secrets Manager are refreshed automatically
* No need to explicitly map every key individually
* Lower maintenance effort

---

# Example ExternalSecret

```yaml
apiVersion: external-secrets.io/v1
kind: ExternalSecret
metadata:
  name: api-credentials
  namespace: payments

spec:
  refreshInterval: 1h

  secretStoreRef:
    name: aws-secretsmanager
    kind: SecretStore

  target:
    name: api-credentials
    creationPolicy: Owner

  dataFrom:
    - extract:
        key: /prod-gke/payments/api-credentials
```

---

# Understanding the Manifest

## `refreshInterval`

Defines how often ESO checks AWS Secrets Manager for updates.

Example:

```yaml
refreshInterval: 1h
```

ESO will re-sync the secret every hour.

---

## `secretStoreRef`

Defines which SecretStore or ClusterSecretStore contains the AWS authentication and provider configuration.

Example:

```yaml
secretStoreRef:
  name: aws-secretsmanager
  kind: SecretStore
```

---

## `target.name`

Defines the name of the Kubernetes Secret that ESO will create.

Example:

```yaml
target:
  name: api-credentials
```

This will create:

```text
Secret/api-credentials
```

inside the namespace.

---

## `dataFrom.extract.key`

Defines which secret should be fetched from AWS Secrets Manager.

Example:

```yaml
dataFrom:
  - extract:
      key: /prod-gke/payments/api-credentials
```

ESO will import all key/value pairs from this AWS secret.

---

# AWS Secret Structure Example

## Example Secret in AWS Secrets Manager

Secret name:

```text
/prod-gke/payments/api-credentials
```

Secret content:

```json
{
  "username": "admin",
  "password": "my-password",
  "endpoint": "https://api.example.com"
}
```

---

# Resulting Kubernetes Secret

ESO will automatically create:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: api-credentials
  namespace: payments
type: Opaque
data:
  username: <base64>
  password: <base64>
  endpoint: <base64>
```

---

# Automatic Synchronization Behavior

## Scenario: Key Updated in AWS Secrets Manager

If:

```json
{
  "password": "new-password"
}
```

is updated in AWS Secrets Manager, ESO will automatically update the Kubernetes Secret during the next reconciliation cycle.

---

## Scenario: New Key Added

If AWS Secrets Manager changes from:

```json
{
  "username": "admin"
}
```

to:

```json
{
  "username": "admin",
  "token": "abc123"
}
```

The new `token` key will automatically appear in the Kubernetes Secret when using `dataFrom`.

No Kubernetes manifest changes are required.

---

# Secret Consumption

Applications consume the synchronized Kubernetes Secret normally, using:

* Environment variables
* Mounted volumes
* Secret references in manifests

Since developers are already familiar with Kubernetes patterns, those consumption methods are not covered in detail here.

---

# Viewing Secret Values

## Using Kubectl

### List Secrets

```bash
kubectl get secrets -n payments
```

---

## View Secret YAML

```bash
kubectl get secret api-credentials -n payments -o yaml
```

---

## Decode Specific Key

Example:

```bash
kubectl get secret api-credentials \
  -n payments \
  -o jsonpath="{.data.password}" | base64 -d
```

---

## List All Keys

```bash
kubectl get secret api-credentials \
  -n payments \
  -o json | jq -r '.data | keys[]'
```

---

# Viewing Secrets Using Lens

Lens

Secrets can also be inspected through Lens.

Typical flow:

1. Open the namespace
2. Navigate to `Workloads` or `Configuration`
3. Open `Secrets`
4. Select the synchronized secret
5. View or decode the available keys

---

# Operational Recommendations

## Use AWS Secrets Manager as Source of Truth

Avoid manually editing Kubernetes Secrets created by ESO.

Any manual changes may be overwritten during reconciliation.

---

## Prefer `dataFrom`

Use `dataFrom` whenever possible to reduce maintenance and automatically synchronize new keys.

---

## Keep Namespace Isolation

Secrets should remain isolated by namespace following the convention:

```text
/<cluster_name>/<namespace>/<secret_name>
```

---

# Benefits of This Model

## Centralized Secret Management

Secrets are managed centrally in AWS Secrets Manager.

---

## Automatic Synchronization

ESO continuously synchronizes updates into Kubernetes.

---

## Reduced Secret Duplication

No need to manually create Kubernetes Secrets.

---

## Native Kubernetes Consumption

Applications continue using standard Kubernetes Secret patterns.

---

# Summary

This integration provides:

* Centralized secret governance through AWS Secrets Manager
* Automated synchronization using ESO
* Namespace-oriented organization
* Reduced operational overhead
* Standard Kubernetes-native secret consumption

The recommended implementation pattern is:

* Secret creation in AWS Secrets Manager via internal request
* ESO synchronization using `dataFrom`
* Kubernetes Secret consumption by workloads
* AWS Secrets Manager as the single source of truth

# Official References

## ESO

[External Secrets Operator Documentation](https://external-secrets.io/latest/?utm_source=chatgpt.com)

## AWS Secrets Manager

[AWS Secrets Manager Docs](https://docs.aws.amazon.com/secretsmanager/?utm_source=chatgpt.com)

## GKE Workload Identity

[GKE Workload Identity Federation Docs](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity?utm_source=chatgpt.com)

## Helm

[Helm Documentation](https://helm.sh/docs/?utm_source=chatgpt.com)