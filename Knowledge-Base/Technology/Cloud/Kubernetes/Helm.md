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

[Docs Home | Helm](https://helm.sh/docs/)
[What is Helm in Kubernetes? Helm and Helm Charts explained | Kubernetes Tutorial 23](https://www.youtube.com/watch?v=-ykwb1d0DXU)

[Artifact Hub](https://artifacthub.io/)

# HELM

Helm is a package manager as `apt`, `yum` or `npm`, but focused on Kubernetes resources.

The principal idea of Helm is:

- Reuse the charts as packs of manifests
- Allows installation, rollbacks, upgrade and parametrization of applications
- To permit standardization and `automation`

# Found problems 

## Using a local repository on GCP.

When i was trying to install the External Secrets Operator in the Kubernetes, I found a limitation cause by a secret rule over my environment. This kind of rule disallow installation of images from the intenet.

To try to solve it I follow the steps `5` onward of my tutorial [[GKE - Installing External Secrets Operator]] creating  Artifact Registry Repository, a Cloud Build Trigger, Configuring a Yaml to use Crane and running the trigger from GCP.

The problem is that when we try to do that the same problem happens because the helm is still trying to install the dependencies from the internet. Then the solution is force the Helm looking for dependencies installations from the [[Artifact Registry Repository]].
