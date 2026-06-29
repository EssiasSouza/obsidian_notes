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

[Command line tool (kubectl) | Kubernetes](https://kubernetes.io/docs/reference/kubectl/)

# Utils

#### Clusters
Use contexts to manage clusters
```
kubectl config get-contexts
```
Changing context (cluster)
```
kubectl config use-context CONTEXT_NAME
```
Check current cluster
```
kubectl config current-context
```
Details of current cluster
```
kubectl cluster-info
```
The context are at `~/.kube/config`

---
