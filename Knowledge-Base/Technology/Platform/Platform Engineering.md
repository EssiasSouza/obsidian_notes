## Platform Engineering

Welcome to **Platform Engineering**. In this section, you will learn about the basic concepts of Platform Engineering and how it helps developers create, deploy, and manage applications more easily in AWS environments.

The Cloud Native Computing Foundation (CNCF) defines a platform as "an integrated collection of capabilities defined and presented according to the needs of the platform's users." In simpler terms, a platform brings together all the services and tools needed to support a variety of applications in a consistent way. A good platform provides a simple user experience for accessing and managing these capabilities through web portals, project templates, and APIs.

As businesses grow, having a central platform to manage different application needs becomes more important. Teams often need to build many applications, and these applications may require anything from simple cloud storage and databases to complex environments like Kubernetes clusters for AI models. A platform engineer ensures that the right resources are available and easy to use, whether they are in the cloud or on-premises. This helps other teams focus on their work without worrying about managing complex infrastructure.

Having a dedicated platform engineering team has become common in many organizations. This team defines how the platform works, including the APIs, and makes decisions to ensure the platform meets the needs of different teams. They work closely with developers, operations specialists, and cloud providers to deliver the right tools and services for the job.

![[Pasted image 20260728145406.png]]

## Understanding Internal Developer Platforms (IDPs)

With what we have learned about platform engineering, let's better understand the concept of [Internal Developer Platforms (IDPs)](https://internaldeveloperplatform.org/)  using a simple analogy: a train station.

In a train station, the platform is a raised level where people wait before boarding a train. Just as this platform helps passengers, an IDP helps streamline application delivery. It acts as the foundation for an organization's software processes.

IDPs provide a set of tools, services, and automation that teams use internally. These tools help developers focus on their main work. Like the train platform, an IDP adds value when developers actively use it. With an IDP, development teams can speed up their workflows, boost productivity, and help the business grow.

One key benefit of IDPs is achieving economies of scale. By centralizing tools and processes, organizations save costs when adding new applications. This efficiency means companies can get significant cost savings over time by using well-designed platforms.

### Opinionated Way of Architecting an Internal Developer Platform (IDP)

The following architecture diagram demonstrates an opinionated way to build an IDP on Amazon EKS:

![[Pasted image 20260728162525.png]]

Components of our opinionated architecture of an Internal Developer Platform:

GitOps Hub and Spoke Architecture, with a management cluster and workloads clusters. Using EKS Capabilities for Argo CD, kro and ACK where AWS handles the operational overhead of running those open-source solutions, that come ready directly within your EKS cluster.

- **Argo CD** - A declarative, GitOps-based continuous deployment tool
- **kro** - Kubernetes Resource Operator that simplifies the creation and management of custom Kubernetes resources through declarative configuration
- **ACK** - An open-source Kubernetes add-on that extends the platform's capabilities by providing a unified API for managing infrastructure resources

The workshop will heavily use these 3 capabilities to also handle our applications and infrastructure deployments.
#### How it works

- AWS creates and manages Argo CD components (API server, repo server, application controller, Redis) in their managed account, along with kro controller and the 54 GA [ACK controllers](https://aws-controllers-k8s.github.io/docs/services) 
- CRDs (Application, ApplicationSet, AppProject, ResourceGraphDefinitions (RGD), and all ACK CRDs) get installed in your EKS cluster
- You create and manage your Argo CD projects/applications/applicationsets, your kro RGDs, or ACK CRD instances in your cluster directly

You can find Comparing EKS Capability for Argo CD to self-managed Argo CD [here](https://docs.aws.amazon.com/eks/latest/userguide/argocd-comparison.html)  .
#### Additional tools

Our EKS capabilities for Argo CD is configured to deploy these additional Kubernetes addons:

- **Backstage** - An open-source platform for building developer portals
- **GitLab** - A web-based DevOps platform that provides Git repository management
- **Keycloak** - A robust open-source identity and access management (IAM) solution that is integrated into the IDP
- **Amazon Managed Grafana** - A visualization tool that offers customizable dashboards for monitoring metrics, logs, and performance data across systems and services
- **Argo Workflows** - A powerful workflow engine that enables the orchestration of complex CI/CD and data pipelines
- **Argo Rollouts** - A progressive delivery controller that provides advanced deployment strategies like blue-green and canary deployments for Kubernetes
- **Kargo** - A continuous promotion orchestration layer that complements Argo CD by streamlining multi-stage application promotion using GitOps principles

Also read about [[Cloud Native Operational Excellence (CNOE)]]

---
Source: #source/internet_resources 
Project: #project/platform 
Areas: #area/work
Subject: #subject/infraestructure
Type: #type/idea
Learning priority: #priority/P0 
Status: #status/learning  
Related: 
