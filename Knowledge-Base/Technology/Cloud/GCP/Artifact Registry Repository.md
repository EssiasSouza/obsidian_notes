Source: #source/internet_resources 
Project: #project/devops
Areas: #area/work
Subject: #subject/kubernetes
Type: #type/learning 
Learning priority: #priority/P1 
Status: #status/learned
Related: [[Kubernetes Roadmap]]
[[Helm]]

---
# Artifact Registry Repository

It is a service provided by [Google Cloud Platform (GCP)](https://cloud.google.com/?utm_source=chatgpt.com) for storing and managing software artifacts in general.

Artifact Registry can be used by several GCP services and resources, such as:

* Google Kubernetes Engine (GKE)
  * Pulling Docker/OCI images
  * Storing Helm charts

* Cloud Run
  * Deploying containers directly from the registry

* Cloud Build
  * Building and publishing images/packages

* Compute Engine
  * Virtual machines can pull container images

* Vertex AI
  * Storing training and inference images

* External CI/CD platforms
  * GitHub Actions
  * GitLab
  * Jenkins

Artifact Registry is also not limited to Docker images. It supports multiple artifact types, including:

* Docker / OCI Images
* Helm Charts
* Maven
* npm
* Python packages
* Apt
* Yum

In practice, Artifact Registry works as a centralized artifact repository for GCP, similar to:

* Docker Hub
* JFrog Artifactory
* Sonatype Nexus Repository

From an operational perspective:

* Kubernetes/GKE uses Artifact Registry to pull images
* Cloud Run uses it to execute containers
* Cloud Build uses it to publish artifacts
* Developers can publish npm, Maven, and Python packages

It is important to note that External Secrets Operator (ESO) does not directly depend on Artifact Registry. ESO is typically integrated with secret management services such as:

* Google Secret Manager
* AWS Secrets Manager
* AWS Systems Manager Parameter Store
* HashiCorp Vault

Artifact Registry is primarily focused on artifact and image distribution rather than secret management.
