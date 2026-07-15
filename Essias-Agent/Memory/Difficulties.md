Source: #source/personal_reflections 
Project: #project/career-reflections
Areas: #area/work
Subject: #subject/professional-log
Type: #type/reflection
Status: #status/active
Related: [[PDP]]
[[phase1-2026-mon2-july]]

---

# Current Difficulties & Challenges (July 2026)

Operational, conceptual, and linguistic friction points encountered in daily work.

---

## 1. Technical / Architectural Challenges

### Kubernetes (GKE) Workload Segregation
*   **Context**: Passing the PCI-DSS 4.0.1 audit requires proving workload isolation.
*   **Friction**: Discovered that many namespace workloads lack microsegmentation and NetworkPolicies. Restricting traffic in active production environments without causing application downtime requires careful traffic flow analysis and planning.

### GCP Security Command Center (SCC) Latency & Findings
*   **Context**: Tuning compliance rules and finding triggers.
*   **Friction**: Finding resolution reports are not reflected instantly in the compliance dashboard. Navigating and mapping the sync latency between configuration changes and compliance status updates (and understanding exactly how GCP generates/clears findings) presents operational delays.

---

## 2. Professional / Delivery Challenges

### Recording Complex Content in Spanish
*   **Context**: Recording the OCI AI Foundations course for Alura.
*   **Friction**: Teaching dense topics like deep learning architectures, transformer structures, and RDMA network setups in a foreign language (Spanish) requires high cognitive energy. Balancing natural delivery, linguistic precision, and technical correctness is challenging.
