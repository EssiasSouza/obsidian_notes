Source: #source/internet_resources  
Project: #project/devops 
Areas: #area/work
Subject: #subject/cloud
Type: #type/learning
Learning priority: #priority/P2 
Status: #status/to_learning 
Related: [[External Secret Operator]]
[[Cloud Computing]]
[[Kubernetes Roadmap]]

---
# Tutorial - Configuring External Secrets Operator with AWS Secrets Manager Using ClusterSecretStore

This tutorial demonstrates how to configure the External Secrets Operator using a cluster-wide `ClusterSecretStore`.

In this approach:

* AWS credentials are centralized
* One `ClusterSecretStore` can be reused across multiple namespaces
* Each application namespace controls its own `ExternalSecret`

---

# Architecture Overview

```text id="css001"
AWS Secrets Manager
        ↓
ClusterSecretStore
        ↓
ExternalSecret
        ↓
Kubernetes Secret
        ↓
Application / Pod
```

---

# Namespace Structure

This tutorial uses the following structure:

| Namespace          | Purpose                                     |
| ------------------ | ------------------------------------------- |
| `external-secrets` | Stores AWS credentials                      |
| `ns-secret-test`   | Stores ExternalSecret and generated secrets |

---

# Prerequisites

This tutorial assumes:

* ESO is already installed
* AWS Secrets Manager secret already exists
* You already have:

  * AWS_ACCESS_KEY_ID
  * AWS_SECRET_ACCESS_KEY

AWS Region:

```text id="css002"
sa-east-1
```

AWS Secret name:

```text id="css003"
test/app
```

---

# 1. Create Namespaces

## Create namespace for AWS credentials

```bash id="css004"
kubectl create namespace external-secrets
```

---

## Create application namespace

```bash id="css005"
kubectl create namespace ns-secret-test
```

---

# 2. Create Kubernetes Secret with AWS Credentials

Create the Secret that stores AWS credentials.

This Secret will live in the `external-secrets` namespace.

```bash id="css006"
kubectl create secret generic aws-credentials \
  -n external-secrets \
  --from-literal=access-key-id=YOUR_ACCESS_KEY_ID \
  --from-literal=secret-access-key=YOUR_SECRET_ACCESS_KEY
```

Example:

```bash id="css007"
kubectl create secret generic aws-credentials \
  -n external-secrets \
  --from-literal=access-key-id=AKIAXXXXXXXXX \
  --from-literal=secret-access-key=xxxxxxxxxxxxxxxx
```

---

# 3. Validate AWS Credentials Secret

```bash id="css008"
kubectl get secret aws-credentials -n external-secrets
```

---

# 4. Create ClusterSecretStore

The `ClusterSecretStore` defines:

* AWS provider
* authentication method
* AWS region
* centralized access to AWS Secrets Manager

---

## Create File

Suggested filename:

```text id="css009"
clustersecretstore.yaml
```

---

## File Content

```yaml id="css010"
apiVersion: external-secrets.io/v1
kind: ClusterSecretStore

metadata:
  name: aws-secretsmanager

spec:
  provider:
    aws:
      service: SecretsManager
      region: sa-east-1

      auth:
        secretRef:
          accessKeyIDSecretRef:
            name: aws-credentials
            key: access-key-id
            namespace: external-secrets

          secretAccessKeySecretRef:
            name: aws-credentials
            key: secret-access-key
            namespace: external-secrets
```

---

# 5. Apply ClusterSecretStore

```bash id="css011"
kubectl apply -f clustersecretstore.yaml
```

---

# 6. Validate ClusterSecretStore

```bash id="css012"
kubectl get clustersecretstore
```

Expected result:

```text id="css013"
READY   STATUS
True    Valid
```

---

# 7. Inspect ClusterSecretStore Details

```bash id="css014"
kubectl describe clustersecretstore aws-secretsmanager
```

---

# 8. Create ExternalSecret

The `ExternalSecret` controls:

* which AWS secret will be consumed
* which properties will be mapped
* which Kubernetes Secret will be created
* target namespace

---

## Create File

Suggested filename:

```text id="css015"
externalsecret.yaml
```

---

## File Content

```yaml id="css016"
apiVersion: external-secrets.io/v1
kind: ExternalSecret

metadata:
  name: app-secret
  namespace: ns-secret-test

spec:
  refreshInterval: 1h

  secretStoreRef:
    name: aws-secretsmanager
    kind: ClusterSecretStore

  target:
    name: app-secret

  data:
    - secretKey: username
      remoteRef:
        key: test/app
        property: username

    - secretKey: password
      remoteRef:
        key: test/app
        property: password
```

---

# 9. AWS Secret Example

Secret name in AWS Secrets Manager:

```text id="css017"
test/app
```

Example JSON content:

```json id="css018"
{
  "username": "admin",
  "password": "123456"
}
```

---

# 10. Apply ExternalSecret

```bash id="css019"
kubectl apply -f externalsecret.yaml
```

---

# 11. Validate ExternalSecret

```bash id="css020"
kubectl get externalsecret -n ns-secret-test
```

Expected:

```text id="css021"
STATUS
SecretSynced
```

---

# 12. Inspect ExternalSecret Details

```bash id="css022"
kubectl describe externalsecret app-secret -n ns-secret-test
```

---

# 13. Validate Generated Kubernetes Secret

List generated Secret:

```bash id="css023"
kubectl get secret app-secret -n ns-secret-test
```

---

# 14. Read Secret Values

## Read username

```bash id="css024"
kubectl get secret app-secret \
  -n ns-secret-test \
  -o jsonpath='{.data.username}' | base64 -d
```

---

## Read password

```bash id="css025"
kubectl get secret app-secret \
  -n ns-secret-test \
  -o jsonpath='{.data.password}' | base64 -d
```

---

# Final Resource Structure

```text id="css026"
external-secrets
 └── aws-credentials

ns-secret-test
 ├── ExternalSecret
 └── app-secret
```

---

# ESO Synchronization Flow

```text id="css027"
AWS Secrets Manager
        ↓
ClusterSecretStore
        ↓
ExternalSecret
        ↓
Kubernetes Secret
        ↓
Deployment / Pod
```

---

# Multi-Namespace Reuse Example

One `ClusterSecretStore` can be reused across many namespaces:

```text id="css028"
external-secrets
 └── aws-credentials

backend
 └── ExternalSecret

finance
 └── ExternalSecret

monitoring
 └── ExternalSecret
```

---

# Useful Audit Commands

## List ClusterSecretStores

```bash id="css029"
kubectl get clustersecretstore
```

---

## List ExternalSecrets

```bash id="css030"
kubectl get externalsecret -A
```

---

## List Kubernetes Secrets

```bash id="css031"
kubectl get secret -A
```

---

## Inspect ClusterSecretStore YAML

```bash id="css032"
kubectl get clustersecretstore aws-secretsmanager -o yaml
```

---

## Inspect ExternalSecret YAML

```bash id="css033"
kubectl get externalsecret app-secret \
  -n ns-secret-test \
  -o yaml
```

---

## Inspect ClusterSecretStore Details

```bash id="css034"
kubectl describe clustersecretstore aws-secretsmanager
```

---

## Inspect ExternalSecret Details

```bash id="css035"
kubectl describe externalsecret app-secret -n ns-secret-test
```

---

## View ESO Logs

```bash id="css036"
kubectl logs -n external-secrets deploy/external-secrets
```

---

# Cleanup Commands

## Remove ExternalSecret

```bash id="css037"
kubectl delete externalsecret app-secret -n ns-secret-test
```

---

## Remove ClusterSecretStore

```bash id="css038"
kubectl delete clustersecretstore aws-secretsmanager
```

---

## Remove Generated Kubernetes Secret

```bash id="css039"
kubectl delete secret app-secret -n ns-secret-test
```

---

## Remove AWS Credentials Secret

```bash id="css040"
kubectl delete secret aws-credentials -n external-secrets
```
