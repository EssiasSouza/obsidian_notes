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
Besides External Secrets Operator, there is a huge ecosystem of projects commonly deployed in mature Kubernetes environments. These are usually introduced according to needs such as observability, security, networking, GitOps, CI/CD, cost optimization, autoscaling, and governance.

Here are some of the most common and relevant projects in the market today:

---

# Observability

## Metrics and Monitoring

* Prometheus
  Collects metrics from the cluster, applications, and infrastructure.

* Grafana
  Dashboards and metrics visualization.

* Kube State Metrics
  Exposes metrics about Kubernetes object states.

* Metrics Server
  Required for HPA and basic resource metrics.

---

## Logging

* Loki
  Lightweight logging solution integrated with Grafana.

* Fluent Bit
  Collects and forwards logs.

* Elasticsearch + Kibana
  Traditional ELK stack.

---

## Tracing / APM

* Jaeger
* Tempo
* OpenTelemetry

Widely used in microservices environments.

---

# Ingress / Networking

## Ingress Controllers

* NGINX Ingress Controller
* Traefik
* HAProxy Ingress

Responsible for exposing HTTP/HTTPS applications.

---

## Service Mesh

* Istio
* Linkerd
* Cilium

Commonly used for:

* mTLS
* advanced observability
* traffic control
* retries
* circuit breaking

---

# Security

## Policy / Governance

* Kyverno
* OPA Gatekeeper

Frequently used to:

* enforce labels
* block privileged containers
* validate standards and policies

---

## Container Security

* Falco
* Trivy
* Kubescape

---

## Certificates

* cert-manager

Automates:

* TLS
* Let’s Encrypt
* certificate renewal

---

# GitOps / Deployment

## GitOps

* Argo CD
* Flux

Practically a standard in enterprise environments today.

---

## Package Management

* Helm
* Kustomize

---

# Autoscaling

* Karpenter
* Cluster Autoscaler
* Vertical Pod Autoscaler

Very popular in AWS EKS environments today.

---

# Secrets Management

Besides ESO:

* HashiCorp Vault
* Sealed Secrets
* SOPS

---

# Backup and Disaster Recovery

* Velero

Very important and often overlooked.

---

# Cost Management / FinOps

* Kubecost
* OpenCost

---

# Developer Platform / Internal Platform

* Backstage
* Crossplane

Crossplane is growing rapidly for:

* platform engineering
* self-service infrastructure
* Infrastructure as Code through Kubernetes APIs

---

# Databases and Messaging

Some teams also operate stateful services inside Kubernetes:

* PostgreSQL Operator
* Strimzi
* RabbitMQ Cluster Operator

---

# Common “Enterprise Standard” Stack

A modern Kubernetes environment often includes something like:

* EKS / GKE / AKS
* Helm
* ArgoCD
* External Secrets Operator
* Prometheus
* Grafana
* Loki
* Fluent Bit
* cert-manager
* Kyverno
* Karpenter
* Trivy
* Velero
* Ingress NGINX
* OpenTelemetry

---

# What Is Growing the Most Today

The highest-demand areas in the market currently are:

1. GitOps
2. Kubernetes Security
3. Platform Engineering
4. Observability
5. FinOps
6. Service Mesh
7. Crossplane
8. eBPF with Cilium
9. Multi-cluster Management
10. AI workloads on Kubernetes

---

# Projects That Match Your Profile Very Well

Based on your cloud/devops background, I would strongly recommend focusing on:

* Argo CD
* Crossplane
* Kyverno
* Cilium
* OpenTelemetry
* Karpenter

These technologies are extremely valuable in enterprise environments today.

## Kubernetes Core — P3

### Pods

### Deployments

### Statefulsets

### Daemonsets

### Services

### Ingress

### Configmaps

### Secrets

### Namespaces

### RBAC

### Service Accounts

### Network Policies

### StorageClasses

### Persistent Volumes



Depois podemos aprofundar em:

- IRSA (melhor prática no EKS)
- ClusterSecretStore vs SecretStore
- PushSecret
- Refresh automático
- Versionamento de secrets
- Multi-account AWS
- Multi-tenant
- Helm + ESO
- GitOps com ArgoCD/Flux
- Segurança/RBAC
- DecodingStrategy
- Templating
- Secrets binários/certificados
- Debugging do ESO
- Falhas comuns de permissão IAM