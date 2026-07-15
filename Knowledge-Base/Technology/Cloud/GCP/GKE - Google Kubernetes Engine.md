Source: #source/internet_resources 
Project: #project/devops
Areas: #area/work
Subject: #subject/cloud
Type: #type/learning 
Learning priority: #priority/P1 
Status: #status/to_learning 
Related: [[Kubernetes Roadmap]] 
[[GCP - Google Cloud Platform]]
[[Cloud Computing]]

---

# GKE PCI-DSS Compliance & Audit Reference

Applying **PCI-DSS 4.0.1** to Google Kubernetes Engine (GKE) requires verifying GKE-specific security controls under the shared responsibility model.

---

## 1. GKE Autopilot: Shared Responsibility for Patch Management (Items 6.3, 6.5)

In **GKE Autopilot**, Google manages the cluster nodes (worker infrastructure), dividing the patching responsibilities:

*   **Google's Responsibility**:
    *   Proactive patching of host operating system (Container-Optimized OS).
    *   Control plane and kubelet updates to address Kubernetes CVEs.
    *   Automated node upgrades based on selected release channels (Rapid, Regular, Stable).
*   **Customer's Responsibility**:
    *   Updating container images in deployments to resolve application-level dependencies and base-OS vulnerabilities inside user workloads.
    *   Configuring maintenance windows to ensure automated upgrades do not disrupt business operations.
    *   Validating workload security configurations (avoiding privileged mode, restricting host volumes).

---

## 2. Kubernetes Logging Governance (All 10.x Requirements)

Proving that actions on the Kubernetes cluster are tracked requires configuring GCP and GKE audit logs:

### GKE Audit Log Types
1.  **Admin Activity Logs**:
    *   *What*: Records write operations (create, update, delete) on cluster resources (Deployments, Roles, NetworkPolicies).
    *   *Status*: Enabled by default. Saved for 400 days in GCP Cloud Logging.
2.  **Data Access Logs**:
    *   *What*: Records read operations (get, list, watch) on cluster API objects, including reads of `Secrets` and `ConfigMaps`.
    *   *Status*: **Disabled by default** due to volume. Must be explicitly enabled via IAM configuration.
    *   *Auditor Expectation*: Required for PCI compliance to trace who read secrets or accessed sensitive API parameters.

### Log Integrity & Storage (Item 10.3, 10.4)
*   **Centralized Sinks**: Route logs from GCP Cloud Logging to a locked-down **Cloud Storage** bucket (configured with Object Retention Locks for write-once-read-many / WORM compliance) or stream to a SIEM.
*   **Anomaly Alerting**: Configure Cloud Monitoring Log-Based Metrics and Alerts to detect sensitive service account usage or unauthorized access to resources in GCI/GKE.

---

## 3. Network Isolation & Workload Segregation (Items 2.1, 2.3)

Kubernetes environments default to flat networks where all Pods can communicate with each other. PCI compliance mandates strict isolation:

*   **Logical Boundaries (Namespaces)**: Group in-scope PCI workloads (e.g., payment APIs) in dedicated namespaces (`pci-payment`), separating them from non-scope workloads (`marketing`, `backoffice`).
*   **Network Policies (Datapath V2)**:
    *   GKE Autopilot enforces NetworkPolicies using **GKE Datapath V2** (based on eBPF).
    *   Apply a **Default Deny-All** policy for the namespace:
        ```yaml
        apiVersion: networking.k8s.io/v1
        kind: NetworkPolicy
        metadata:
          name: default-deny-all
          namespace: pci-payment
        spec:
          podSelector: {}
          policyTypes:
          - Ingress
          - Egress
        ```
    *   Explicitly authorize required micro-connections (e.g., allowing `payment-api` ingress only from the `api-gateway` pod, and egress only to the database proxy).
*   **Identity Segregation**:
    *   Avoid using the `default` K8s Service Account. Use dedicated Service Accounts for every deployment.
    *   Use **Workload Identity Federation** to bind K8s Service Accounts directly to IAM Service Accounts with least-privilege permissions, completely avoiding static service account keys inside containers.
