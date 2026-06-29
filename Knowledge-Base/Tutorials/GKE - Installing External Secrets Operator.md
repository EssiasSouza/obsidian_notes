Source: #source/internet_resources 
Project: #project/devops
Areas: #area/work
Subject: #subject/kubernetes
Type: #type/tutorial
Status: #status/learning 
Related: [[Cloud Computing]]
[[Kubernetes Roadmap]]
[[External Secret Operator]]
[[GKE - Google Kubernetes Engine]]

---

# Tutorial - How to Install External Secrets Operator on GKE with Binary Authorization Enabled

This tutorial explains how to install External Secrets Operator on a Google Kubernetes Engine cluster when the environment blocks public container images through Binary Authorization policies.

The issue encountered was:

```text id="v9x4tp"
Denied by Binary Authorization default admission rule
```

This means the cluster does not allow pulling images directly from public registries such as `ghcr.io`.

---

# Goal

Transform this flow:

```text id="g4m8rc"
Kubernetes -> GHCR -> BLOCKED
```

into this:

```text id="k2p7nx"
Kubernetes -> Artifact Registry -> ALLOWED
```

---

# Prerequisites

You need:

* `kubectl`
* `helm`
* `gcloud`
* access to the Google Cloud project
* permissions for:

  * Cloud Build
  * Artifact Registry
  * GKE

---

# Step 1 - Add the Helm Repository

Add the ESO Helm repository:

```bash id="w6q2dz"
helm repo add external-secrets https://charts.external-secrets.io
```

Update repositories:

```bash id="r3m9vk"
helm repo update
```

---

# Step 2 - Attempt to Install ESO

Initial installation:

```bash id="h8n4sx"
helm install external-secrets external-secrets/external-secrets -n external-secrets --create-namespace --set installCRDs=true
```

---

# Step 3 - Validate the Pods

Check whether Pods were created:

```bash id="y5t1qp"
kubectl get pods -n external-secrets
```

In our case, the result was:

```text id="m7k2cv"
No resources found
```

---

# Step 4 - Investigate Cluster Events

Check namespace events:

```bash id="f2v8ld"
kubectl get events -n external-secrets --sort-by=.metadata.creationTimestamp
```

Error found:

```text id="q4d7wu"
Denied by Binary Authorization default admission rule
```

Blocked image:

```text id="z1p5xr"
ghcr.io/external-secrets/external-secrets:v2.5.0
```

---

# Understanding the Problem

The cluster had a security policy preventing direct downloads from public registries.

Blocked flow:

```text id="t8m3sy"
Kubernetes -> ghcr.io -> BLOCKED
```

Solution:

```text id="n6x1cq"
GHCR -> Artifact Registry -> Kubernetes
```

---

# Step 5 - Create an Artifact Registry Repository

Open:

[Artifact Registry Console](https://console.cloud.google.com/artifacts?utm_source=chatgpt.com)

Create a repository with:

| Field  | Value                |
| ------ | -------------------- |
| Name   | `containers`         |
| Format | `Docker`             |
| Mode   | `Standard`           |
| Region | `southamerica-east1` |

Then click:

```text id="p3r8vd"
Create
```

---

# Step 6 - Create a Cloud Build Trigger

Open:

[Cloud Build Triggers](https://console.cloud.google.com/cloud-build/triggers?utm_source=chatgpt.com)

Create a trigger with:

| Field | Value                   |
| ----- | ----------------------- |
| Name  | `copy-external-secrets` |
| Event | `Manual invocation`     |

---

# Step 7 - Configure Inline YAML

In:

```text id="x5k1dt"
Configuration -> Location
```

Select:

```text id="v7n4ps"
Inline
```

Then paste:

```yaml id="d2w8my"
steps:
  - name: 'gcr.io/go-containerregistry/crane'
    args:
      - 'copy'
      - 'ghcr.io/external-secrets/external-secrets:v2.5.0'
      - 'southamerica-east1-docker.pkg.dev/PROJECT_ID/containers/external-secrets:v2.5.0'
```

Replace:

```text id="u9c6qx"
PROJECT_ID
```

with your actual project ID.

---

# Understanding `crane`

crane is a tool used to copy container images between registries without requiring Docker.

Cloud Build executes a temporary container that already contains the `crane` binary.

Internal flow:

```text id="j4v9kn"
Cloud Build -> Temporary crane container -> copy image
```

---

# Step 8 - Run the Trigger

After creating the trigger:

1. Go back to:

   ```text id="b8q5ms"
   Cloud Build -> Triggers
   ```

2. Click:

   ```text id="w1d7rc"
   Run
   ```

3. Execute the trigger manually.

---

# Step 9 - Validate the Image in Artifact Registry

After the build finishes, verify the image inside Artifact Registry.

You should see:

```text id="r6n2tz"
external-secrets:v2.5.0
```

---

# Step 10 - Install ESO Using the Internal Image

Now reinstall Helm pointing to the internal registry image:

```bash id="k3x8fy"
helm install external-secrets external-secrets/external-secrets \
-n external-secrets \
--create-namespace \
--set installCRDs=true \
--set image.repository=southamerica-east1-docker.pkg.dev/PROJECT_ID/containers/external-secrets \
--set image.tag=v2.5.0 \
--set webhook.image.repository=southamerica-east1-docker.pkg.dev/PROJECT_ID/containers/external-secrets \
--set webhook.image.tag=v2.5.0 \
--set certController.image.repository=southamerica-east1-docker.pkg.dev/PROJECT_ID/containers/external-secrets \
--set certController.image.tag=v2.5.0
```

---
