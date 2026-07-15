Source: #source/internet_resources  
Project: #project/devops 
Areas: #area/work
Subject: #subject/cloud
Type: #type/learning
Learning priority: #priority/P1
Status: #status/to_learning 
Related: [[gcp]]
[[Cloud Computing]]

---

# Cloud SQL Security & Compliance Auditing

Managed database instances in Google Cloud must be audited periodically to satisfy security frameworks, particularly **PCI-DSS 4.0.1 (Requirements 2.1, 2.3)**.

---

## 1. Critical Database Compliance Flags

When auditing Cloud SQL instances, verify the following configuration states:

| Database Engine | Security Flag | Compliant Value | Purpose |
| :--- | :--- | :--- | :--- |
| **All Engines** | `Enforce SSL/TLS` | `On` / `Enforced` | Encrypts database transit data (PCI-DSS 2.3). |
| **All Engines** | `Authorized Networks` | `None (Empty)` | Restricts direct public access; access must use Cloud SQL Auth Proxy. |
| **PostgreSQL** | `log_connections` | `on` | Logs all database connection attempts (PCI-DSS 10.x). |
| **PostgreSQL** | `log_disconnections` | `on` | Logs connection termination (PCI-DSS 10.x). |
| **PostgreSQL** | `log_min_messages` | `warning` or lower | Prevents log flooding while maintaining security logs. |
| **MySQL** | `local_infile` | `off` | Prevents local file loading exploits. |
| **SQL Server** | `external scripts enabled` | `off` | Disables execution of external unverified scripts. |

---

## 2. Automating Audits with Python

To make audits repeatable and prevent console-based manual errors, use a modular Python script to query the GCP Cloud SQL Admin API.

### Script Design Goals
*   **Decoupled & Single-Purpose**: Build small, single-purpose functions instead of monolithic blocks.
*   **Audit Trail Logs**: Export results in structured format (JSON/CSV) to feed directly into audit evidence storage.

### Core Automation Logic (Conceptual Example)

```python
import googleapiclient.discovery
from google.auth import compute_engine

def get_cloudsql_client():
    # Authenticate and construct API client
    return googleapiclient.discovery.build('sqladmin', 'v1beta4')

def list_instances(client, project_id):
    # Retrieve all instances in the project
    request = client.instances().list(project=project_id)
    response = request.execute()
    return response.get('items', [])

def audit_database_flags(instance):
    instance_name = instance['name']
    settings = instance.get('settings', {})
    ip_configuration = settings.get('ipConfiguration', {})
    
    # 1. Check TLS Enforcement
    ssl_enforced = ip_configuration.get('requireSsl', False)
    
    # 2. Check Public IP Exposure
    public_ip_enabled = any(ip['type'] == 'PRIMARY' for ip in instance.get('ipAddresses', []))
    
    # 3. Check Database Flags
    flags = settings.get('databaseFlags', [])
    flag_dict = {f['name']: f['value'] for f in flags}
    
    return {
        "instance": instance_name,
        "ssl_enforced": ssl_enforced,
        "public_ip_enabled": public_ip_enabled,
        "flags": flag_dict
    }
```
