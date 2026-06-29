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
# Tutorial - Configuring External Secrets Operator with AWS Secrets Manager Using SecretStore

This tutorial demonstrates how to configure the External Secrets Operator using a namespace-scoped `SecretStore`.

In this approach, the following resources will live in the same namespace:

* AWS credentials Secret
* SecretStore
* ExternalSecret
* Generated Kubernetes Secret

Namespace used in this tutorial:

```text id="jlwm100"
ns-secret-test
```

---

# Architecture Overview

```text id="jlwm101"
AWS Secrets Manager
        ↓
SecretStore
        ↓
ExternalSecret
        ↓
Kubernetes Secret
        ↓
Application / Pod
```

---

# Prerequisites

This tutorial assumes:

* ESO is already installed
* AWS Secrets Manager secret already exists
* You already have:

  * AWS_ACCESS_KEY_ID
  * AWS_SECRET_ACCESS_KEY

AWS Region used in this tutorial:

```text id="jlwm102"
sa-east-1
```

AWS Secret name:

```text id="jlwm103"
test/app
```

---

# 1. Create Namespace

Create the namespace that will contain all resources.

```bash id="jlwm104"
kubectl create namespace ns-secret-test
```

---

# 2. Create Kubernetes Secret with AWS Credentials

Create a Secret containing AWS credentials used by ESO.

```bash id="jlwm105"
kubectl create secret generic aws-credentials \
  -n ns-secret-test \
  --from-literal=access-key-id=YOUR_ACCESS_KEY_ID \
  --from-literal=secret-access-key=YOUR_SECRET_ACCESS_KEY
```

Example:

```bash id="jlwm106"
kubectl create secret generic aws-credentials \
  -n ns-secret-test \
  --from-literal=access-key-id=AKIAXXXXXXXXX \
  --from-literal=secret-access-key=xxxxxxxxxxxxxxxx
```

---

# 3. Validate Kubernetes Secret

```bash id="jlwm107"
kubectl get secret aws-credentials -n ns-secret-test
```

---

# 4. Create SecretStore

The `SecretStore` defines:

* AWS provider
* authentication method
* AWS region
* how ESO connects to AWS Secrets Manager

---

## Create File

Suggested filename:

```text id="jlwm108"
secretstore.yaml
```

---

## File Content

```yaml id="jlwm109"
apiVersion: external-secrets.io/v1
kind: SecretStore

metadata:
  name: aws-secretsmanager
  namespace: ns-secret-test

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

          secretAccessKeySecretRef:
            name: aws-credentials
            key: secret-access-key
```

---

# 5. Apply SecretStore

```bash id="’wini110"
kubectl apply -f secretstore.yaml
```

---

# 6. Validate SecretStore

```bash id="’wini111"
kubectl get secretstore -n ns-secret-test
```

Expected result:

```text id="’wini112"
READY   STATUS
True    Valid
```

---

# 7. Inspect SecretStore Details

```bash id="’wini113"
kubectl describe secretstore aws-secretsmanager -n ns-secret-test
```

---

# 8. Create ExternalSecret

The `ExternalSecret` is responsible for synchronizing data from AWS Secrets Manager into Kubernetes.

---

## Create File

Suggested filename:

```text id="’wini114"
externalsecret.yaml
```

---

## File Content

```yaml id="’wini115"
apiVersion: external-secrets.io/v1
kind: ExternalSecret

metadata:
  name: app-secret
  namespace: ns-secret-test

spec:
  refreshInterval: 1h

  secretStoreRef:
    name: aws-secretsmanager
    kind: SecretStore

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

```text id="’wini116"
test/app
```

Example JSON content:

```json id="’wini117"
{
  "username": "admin",
  "password": "123456"
}
```

---

# 10. Apply ExternalSecret

```bash id="’wini118"
kubectl apply -f externalsecret.yaml
```

---

# 11. Validate ExternalSecret

```bash id="’wini119"
kubectl get externalsecret -n ns-secret-test
```

Expected:

```text id="’wini120"
STATUS
SecretSynced
```

---

# 12. Inspect ExternalSecret Details

```bash id="’wini121"
kubectl describe externalsecret app-secret -n ns-secret-test
```

---

# 13. Validate Generated Kubernetes Secret

List the generated Secret:

```bash id="’wini122"
kubectl get secret app-secret -n ns-secret-test
```

---

# 14. Read Secret Values

## Read username

```bash id="’wini123"
kubectl get secret app-secret \
  -n ns-secret-test \
  -o jsonpath='{.data.username}' | base64 -d
```

---

## Read password

```bash id="’wini124"
kubectl get secret app-secret \
  -n ns-secret-test \
  -o jsonpath='{.data.password}' | base64 -d
```

---

# Final Resource Structure

```text id="’wini125"
ns-secret-test
 ├── aws-credentials
 ├── SecretStore
 ├── ExternalSecret
 └── app-secret
```

---

# ESO Synchronization Flow

```text id="’wini126"
AWS Secrets Manager
        ↓
SecretStore
        ↓
ExternalSecret
        ↓
Kubernetes Secret
        ↓
Deployment / Pod
```

---

# Useful Audit Commands

## List SecretStores

```bash id="’wini127"
kubectl get secretstore -A
```

---

## List ExternalSecrets

```bash id="’wini128"
kubectl get externalsecret -A
```

---

## List Kubernetes Secrets

```bash id="’wini129"
kubectl get secret -A
```

---

## Inspect SecretStore YAML

```bash id="’wini130"
kubectl get secretstore aws-secretsmanager \
  -n ns-secret-test \
  -o yaml
```

---

## Inspect ExternalSecret YAML

```bash id="’wini131"
kubectl get externalsecret app-secret \
  -n ns-secret-test \
  -o yaml
```

---

## Inspect SecretStore Details

```bash id="’wini132"
kubectl describe secretstore aws-secretsmanager \
  -n ns-secret-test
```

---

## Inspect ExternalSecret Details

```bash id="’wini133"
kubectl describe externalsecret app-secret \
  -n ns-secret-test
```

---

## View ESO Logs

```bash id="’wini134"
kubectl logs -n external-secrets deploy/external-secrets
```

---

# Cleanup Commands

## Remove ExternalSecret

```bash id="’wini135"
kubectl delete externalsecret app-secret -n ns-secret-test
```

---

## Remove SecretStore

```bash id="’wini136"
kubectl delete secretstore aws-secretsmanager -n ns-secret-test
```

---

## Remove Generated Kubernetes Secret

```bash id="’wini137"
kubectl delete secret app-secret -n ns-secret-test
```

---

## Remove AWS Credentials Secret

```bash id="’wini138"
kubectl delete secret aws-credentials -n ns-secret-test
```
