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
# Custom Resource Definition

This is an object from Kubernetes used to define:

- New resource name
- API Version
- Schema
- Valid fields
- Behavior

Example:
```
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition

metadata:
  name: externalsecrets.external-secrets.io

spec:
  group: external-secrets.io

  names:
    kind: ExternalSecret
    plural: externalsecrets

  scope: Namespaced

  versions:
    - name: v1
```

# Running commands

To list CRDs on the Kubernetes
```
kubectl get crd
```
or
```
kubectl get customresourcedefinitions
```
or 
```
kubectl get crd
```

---

See details of a CRD.
```
kubectl get crd externalsecrets.external-secrets.io -o yaml
```
or
```
kubectl describe crd externalsecrets.external-secrets.io
```
---
Reading a specific object
```
kubectl get externalsecret my-secret -n ns-test -o yaml
```
or
```
kubectl describe externalsecret my-secret -n ns-test
```
---
See all possible fields in the CRD
```
kubectl explain externalsecret
```
Specific field
```
kubectl explain externalsecret.spec
```
Deeper
```
kubectl explain externalsecret.spec.data
```
---
Exporting whole CRD
```
kubectl get crd externalsecrets.external-secrets.io -o yaml > externalsecret-crd.yaml
```
---
Discovering all of CRD versions
```
kubectl get crd externalsecrets.external-secrets.io -o jsonpath='{.spec.versions[*].name}'
```
