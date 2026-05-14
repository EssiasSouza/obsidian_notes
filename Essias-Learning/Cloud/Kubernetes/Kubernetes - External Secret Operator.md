Source: #source/internet_resources
Project: #project/sem_parar
Areas: #area/work
Subject: #subject/IAM
Type: #type/idea
Related: 

The external secret operator is a [[Kubernetes]] operator that synchronizes secrets from external secret management systems into native [[Kubernetes]]

With ESO we can use AWS Secret Manager, Google Secret Manager, HashiCorp Vault, Azure Key Vault, etc.

# Simple Tutorial - External Secrets Operator + AWS Secrets Manager + GKE

We’ll build this flow:

```text
AWS Secrets Manager
        ↓
External Secrets Operator (GKE)
        ↓
Kubernetes Secret
        ↓
Application Pod
```

This tutorial uses:

- Google Cloud GKE
- Amazon Web Services Secrets Manager
- Kubernetes
- Helm
- External Secrets Operator

We’ll start SIMPLE first:

- Using static AWS credentials
- Easier for learning
- Later you can migrate to Workload Identity Federation

---

# Architecture

## What we are going to create

```text
AWS Secret
   ↓
ClusterSecretStore
   ↓
ExternalSecret
   ↓
Kubernetes Secret
   ↓
Pod
```

ClusterSecretStore is not a cluster. This is a CRD resource created by External Secret Operator to be used by all of namespaces.

It stocks:
- provider
- region
- type of authentication
- credentials
- ARN role
- Endpoint

---

# STEP 1 - Create a Secret in AWS

## Install AWS CLI

Official docs:

[AWS CLI Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html?utm_source=chatgpt.com)

---

## Configure AWS CLI

```bash
aws configure
```

Provide:

```text
AWS Access Key ID
AWS Secret Access Key
Region
```

---

## Create a test secret

```bash
aws secretsmanager create-secret \
  --name demo/database \
  --secret-string '{"username":"admin","password":"123456"}'
```

---

## Validate

```bash
aws secretsmanager get-secret-value \
  --secret-id demo/database
```

You should see the JSON secret.

---

# STEP 2 - Create a GKE Cluster

If you already have one, skip this.

## Create cluster

```bash
gcloud container clusters create eso-demo \
  --region us-central1 \
  --num-nodes 1
```

---

## Connect kubectl

```bash
gcloud container clusters get-credentials eso-demo --region us-central1
```

---

## Validate

```bash
kubectl get nodes
```

---

# STEP 3 — Install Helm

Official website:

[Helm Official Website](https://helm.sh/?utm_source=chatgpt.com)

---

## Validate installation

```bash
helm version
```

---

# STEP 4 — Install External Secrets Operator

Official docs:

[External Secrets Operator Docs](https://external-secrets.io/latest/?utm_source=chatgpt.com)

---

## Add Helm repo

```bash
helm repo add external-secrets https://charts.external-secrets.io

helm repo update
```

---

## Install ESO

```bash
helm install external-secrets \
  external-secrets/external-secrets \
  -n external-secrets \
  --create-namespace \
  --set installCRDs=true
```

---

## Validate pods

```bash
kubectl get pods -n external-secrets
```

You should see:

- operator pod
    
- webhook pod
    
- cert-controller pod
    

---

# STEP 5 — Create AWS Credentials Secret in Kubernetes

For learning purposes only.

⚠️ In production, prefer Workload Identity Federation.

---

## Create namespace

```bash
kubectl create namespace demo
```

---

## Create AWS credentials secret

```bash
kubectl create secret generic awssm-secret \
  -n external-secrets \
  --from-literal=access-key=YOUR_AWS_ACCESS_KEY \
  --from-literal=secret-access-key=YOUR_AWS_SECRET_KEY
```

---

# STEP 6 — Create ClusterSecretStore

Create file:

```yaml
apiVersion: external-secrets.io/v1
kind: ClusterSecretStore
metadata:
  name: aws-secretsmanager
spec:
  provider:
    aws:
      service: SecretsManager
      region: us-east-1

      auth:
        secretRef:
          accessKeyIDSecretRef:
            name: awssm-secret
            key: access-key
            namespace: external-secrets

          secretAccessKeySecretRef:
            name: awssm-secret
            key: secret-access-key
            namespace: external-secrets
```

---

## Apply

```bash
kubectl apply -f clustersecretstore.yaml
```

---

## Validate

```bash
kubectl get clustersecretstore
```

---

# STEP 7 — Create ExternalSecret

Create file:

```yaml
apiVersion: external-secrets.io/v1
kind: ExternalSecret
metadata:
  name: demo-secret
  namespace: demo

spec:
  refreshInterval: 1m

  secretStoreRef:
    name: aws-secretsmanager
    kind: ClusterSecretStore

  target:
    name: app-secret
    creationPolicy: Owner

  data:
    - secretKey: username
      remoteRef:
        key: demo/database
        property: username

    - secretKey: password
      remoteRef:
        key: demo/database
        property: password
```

---

## Apply

```bash
kubectl apply -f externalsecret.yaml
```

---

# STEP 8 — Validate Generated Kubernetes Secret

## Check ExternalSecret

```bash
kubectl get externalsecret -n demo
```

---

## Check generated Secret

```bash
kubectl get secret -n demo
```

You should see:

```text
app-secret
```

---

## Inspect Secret

```bash
kubectl get secret app-secret \
  -n demo \
  -o yaml
```

---

# STEP 9 — Consume Secret in a Pod

Create file:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-secret-demo
  namespace: demo

spec:
  containers:
    - name: nginx
      image: nginx

      env:
        - name: DB_USERNAME
          valueFrom:
            secretKeyRef:
              name: app-secret
              key: username

        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: app-secret
              key: password
```

---

## Apply

```bash
kubectl apply -f pod.yaml
```

---

## Validate env vars

```bash
kubectl exec -it nginx-secret-demo -n demo -- env
```

You should see:

```text
DB_USERNAME=admin
DB_PASSWORD=123456
```

---

# STEP 10 — Test Secret Rotation

## Update secret in AWS

```bash
aws secretsmanager update-secret \
  --secret-id demo/database \
  --secret-string '{"username":"admin","password":"NEWPASSWORD"}'
```

---

## Wait 1 minute

ESO refreshes based on:

```yaml
refreshInterval: 1m
```

---

## Check Kubernetes Secret again

```bash
kubectl get secret app-secret \
  -n demo \
  -o jsonpath='{.data.password}' | base64 -d
```

You should see:

```text
NEWPASSWORD
```

---

# What You Learned

You now understand:

- AWS Secrets Manager
    
- ESO architecture
    
- Secret synchronization
    
- ClusterSecretStore
    
- ExternalSecret
    
- Kubernetes Secret consumption
    
- Secret rotation
    

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

# Official References

## ESO

[External Secrets Operator Documentation](https://external-secrets.io/latest/?utm_source=chatgpt.com)

## AWS Secrets Manager

[AWS Secrets Manager Docs](https://docs.aws.amazon.com/secretsmanager/?utm_source=chatgpt.com)

## GKE Workload Identity

[GKE Workload Identity Federation Docs](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity?utm_source=chatgpt.com)

## Helm

[Helm Documentation](https://helm.sh/docs/?utm_source=chatgpt.com)