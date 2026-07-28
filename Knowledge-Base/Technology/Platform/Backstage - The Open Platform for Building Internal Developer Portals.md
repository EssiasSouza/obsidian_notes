Source: #source/internet_resources 
Project: #project/platform 
Areas: #area/work
Subject: #subject/infraestructure
Type: #type/idea
Learning priority: #priority/P0 
Status: #status/learning  
Related: [[Platform Engineering]]

---
# Backstage: The Open Platform for Building Internal Developer Portals

Modern engineering teams use dozens, sometimes hundreds, of tools every day. Source code lives in Git repositories, deployments happen through CI/CD pipelines, infrastructure is managed in cloud platforms, documentation is spread across multiple systems, and monitoring relies on several observability tools. As organizations grow, developers spend more time searching for information than building software.

This is exactly the problem that **Backstage** was created to solve.

## What is Backstage?

Backstage is an open source platform for building **Internal Developer Portals (IDPs)**. It was originally created by Spotify to simplify the developer experience and later donated to the Cloud Native Computing Foundation (CNCF).

Rather than replacing your existing tools, Backstage acts as a central hub where developers can access everything they need in one place.

Think of it as the **single entry point** for your engineering organization.

With Backstage, developers can:

* Discover software components and services.
* Access technical documentation.
* Create new projects using predefined templates.
* Monitor deployments and infrastructure.
* View ownership information.
* Integrate with cloud providers and DevOps tools.
* Standardize development workflows.

The goal is simple: reduce cognitive load so developers can focus on delivering business value.

## Why Internal Developer Portals Matter

As organizations adopt microservices, Kubernetes, cloud platforms, Infrastructure as Code, GitOps, and DevOps practices, the number of tools increases rapidly.

Without a unified portal, engineers often ask questions like:

* Which repository contains this service?
* Who owns this application?
* Where is the documentation?
* Which Kubernetes cluster is it running on?
* What CI/CD pipeline deploys it?
* Which cloud project hosts it?
* Is this service still actively maintained?

Finding these answers can take minutes or even hours.

Backstage centralizes this information into a single interface, making software easier to discover, manage, and maintain.

## Core Concepts

### Software Catalog

The Software Catalog is the heart of Backstage.

Every application, API, library, microservice, website, or infrastructure component can be registered as an entity.

Each entity contains metadata such as:

* Name
* Description
* Owner
* Repository
* Documentation
* Dependencies
* APIs
* Lifecycle
* Tags

This creates a living inventory of your engineering ecosystem.

## TechDocs

Documentation is often scattered across multiple platforms and quickly becomes outdated.

TechDocs enables teams to keep documentation close to the source code.

Documentation is typically written in Markdown and automatically published inside Backstage, ensuring it evolves together with the application.

## Software Templates

Creating new projects manually often leads to inconsistencies.

Backstage provides Software Templates that automate project creation.

A template can generate:

* Repository structure
* CI/CD pipelines
* Dockerfiles
* Kubernetes manifests
* Terraform modules
* GitHub Actions
* README files

This ensures every new project follows the organization's engineering standards.

## Plugins

One of Backstage's greatest strengths is its plugin architecture.

Plugins integrate Backstage with existing platforms and services.

Popular integrations include:

* GitHub
* GitLab
* Azure DevOps
* Jenkins
* Argo CD
* Kubernetes
* Grafana
* Prometheus
* Datadog
* PagerDuty
* Jira
* SonarQube
* AWS
* Google Cloud
* Microsoft Azure

Organizations can also build custom plugins for internal systems.

## Kubernetes Integration

For teams running Kubernetes, Backstage provides valuable operational visibility.

Developers can view:

* Cluster resources
* Running workloads
* Pods
* Services
* Deployments
* Health status
* Resource ownership

Instead of navigating multiple dashboards, developers access operational information directly from the application page.

## Golden Paths

One of the most valuable concepts promoted by Backstage is the idea of **Golden Paths**.

A Golden Path is the recommended way to build, deploy, and operate software within an organization.

Rather than forcing developers to learn every internal process, Backstage guides them through standardized workflows.

Examples include:

* Creating a new microservice
* Deploying to Kubernetes
* Publishing an API
* Provisioning cloud infrastructure
* Creating Terraform projects

Golden Paths reduce onboarding time while improving consistency across engineering teams.

## Benefits for Platform Engineering

Backstage has become one of the flagship tools in the Platform Engineering movement.

Platform teams use it to provide self-service capabilities while maintaining governance and security.

Key benefits include:

* Faster developer onboarding
* Improved software discoverability
* Standardized engineering practices
* Better documentation
* Reduced operational overhead
* Increased developer productivity
* Stronger ownership visibility
* Self-service infrastructure
* Improved governance
* Better compliance with engineering standards

## Typical Architecture

A typical Backstage deployment consists of:

* React frontend
* Node.js backend
* PostgreSQL database
* Software Catalog
* Authentication provider
* Plugin ecosystem
* Kubernetes deployment (optional but common)

Backstage can run on Kubernetes, virtual machines, or container platforms, depending on the organization's infrastructure.

## Is Backstage Right for Every Company?

Not necessarily.

Small teams with only a handful of applications may not benefit significantly from an Internal Developer Portal.

However, as organizations grow and manage dozens or hundreds of services, Backstage becomes increasingly valuable by reducing friction and improving the developer experience.

It is particularly beneficial for organizations adopting:

* Microservices
* Kubernetes
* Platform Engineering
* DevOps
* GitOps
* Infrastructure as Code
* Cloud-native architectures

## Final Thoughts

Backstage is much more than a documentation portal. It is a platform that unifies the software development ecosystem into a single, consistent developer experience.

By bringing together documentation, service discovery, templates, infrastructure visibility, and integrations, Backstage helps engineering teams spend less time searching for information and more time building reliable software.

As Platform Engineering continues to evolve, Internal Developer Portals are becoming a foundational component of modern software organizations. Backstage has established itself as the leading open source solution in this space, offering flexibility, extensibility, and a strong community that continues to expand its ecosystem.
