Source: #source/internet_resources 
Project: #project/devops
Areas: #area/work
Subject: #subject/kubernetes
Type: #type/tutorial
Status: #status/learning 
Related: [[Cloud Computing]]
[[GCP - Google Cloud Platform]]

---
# Migrating Secrets from Google Cloud Secret Manager to Delinea Secret Server

This tutorial explains how to migrate secrets stored in **Google Cloud Secret Manager** to **Delinea Secret Server** using a Python script and the Delinea REST API.

## Prerequisites

Before starting, make sure you have:

- Python 3.9 or later
- Access to Google Cloud Secret Manager (to export the secrets)
- Access to a Delinea Secret Server instance
- API credentials for Delinea
- Permission to create secrets in the target folder

Install the required Python package:

```bash
pip install requests
```

---

# Step 1. Export Secrets from Google Cloud

Export your secrets into a JSON file similar to the following:

```json
{
  "refi-service-api-secret": [
    {
      "version": "2",
      "aliases": [],
      "status": "ENABLED",
      "scheduled_for_destruction": null,
      "encryption": "Google-managed",
      "created_on": "2022-08-15T19:01:00.123456Z",
      "value": "newuser\nMyPassword123"
    },
    {
      "version": "1",
      "aliases": [],
      "status": "DISABLED",
      "scheduled_for_destruction": null,
      "encryption": "Google-managed",
      "created_on": "2022-08-15T18:01:00.123456Z",
      "value": "olduser\nOldPassword"
    }
  ]
}
```

Each secret contains one or more versions.

The migration script can use either:

- Only the latest version
- Every version 
- Only enabled versions

Choose the option that best fits your migration strategy.

---

# Step 2. Understand the Secret Format

In this example, each secret value contains two lines:

```
username
password
```

For example:

```
newuser
MyPassword123
```

The migration script interprets these values as:

|Line|Destination|
|---|---|
|First line|Username|
|Second line|Password|

If your secrets use another format (JSON, YAML, key/value pairs, certificates, etc.), modify the parsing logic accordingly.

---

# Step 3. Authenticate with Delinea

Obtain an OAuth access token from the Delinea Secret Server.

Example endpoint:

```
POST /oauth2/token
```

The token returned by this endpoint must be included in the Authorization header of subsequent API requests.

---

# Step 4. Configure the Migration Script

Configure the following values:

```python
DELINEA_URL = "https://your-secretserver.company.com/SecretServer"

USERNAME = "api-user"
PASSWORD = "api-password"

FOLDER_ID = 1

TEMPLATE_ID = 6001

JSON_FILE = "secrets.json"
```

Replace these values with those from your environment.

---

# Step 5. Read the JSON File

Load the exported file:

```python
with open("secrets.json", encoding="utf-8") as f:
    secrets = json.load(f)
```

Each entry contains:

- Secret name
    
- Version information
    
- Secret value
    
- Metadata
    

---

# Step 6. Parse the Secret Value

Split the secret value into username and password:

```python
lines = value.splitlines()

username = lines[0]
password = lines[1]
```

You may adapt this logic if your secrets follow a different structure.

---

# Step 7. Create the Secret

Send a POST request to:

```
POST /api/v1/secrets
```

Example payload:

```json
{
  "folderId": 1,
  "secretTemplateId": 6001,
  "name": "refi-service-api-secret",
  "items": [
    {
      "slug": "username",
      "itemValue": "newuser"
    },
    {
      "slug": "password",
      "itemValue": "MyPassword123"
    }
  ]
}
```

If the request succeeds, the new secret will be created in Delinea.

---

# Step 8. Process Every Secret

Loop through every secret in the JSON file:

```python
for secret_name, versions in secrets.items():

    latest = versions[0]

    value = latest["value"]

    ...
```

For each secret:

1. Read the latest version.
2. Parse the value.
3. Create the secret in Delinea.
4. Log success or failure.

---

# Optional Improvements

A production-ready migration tool should include additional features such as:

- Check whether a secret already exists.
- Update existing secrets instead of creating duplicates.
- Preserve metadata such as:
    - Version
    - Status
    - Creation date
    - Encryption type
    - Aliases

- Create folders automatically if they do not exist.
- Generate a migration report.
- Retry failed API calls.
- Log all operations to a file.
- Validate secret formats before importing.
- Support multiple Delinea Secret Templates.

---

# Important Notes

The following values are environment-specific and may differ between Secret Server installations:

- Folder IDs
- Secret Template IDs
- Field slugs

For example, one installation may define:

```json
{
  "slug": "username"
}
```

while another uses:

```json
{
  "slug": "User Name"
}
```

Always verify your Secret Template configuration before running the migration.

---

# Expected Workflow

```text
Google Cloud Secret Manager
            │
            │
            ▼
Export Secrets to JSON
            │
            ▼
Python Migration Script
            │
            ├── Read JSON
            ├── Parse secret values
            ├── Authenticate to Delinea
            ├── Create or update secrets
            └── Generate migration report
            │
            ▼
Delinea Secret Server
```

This approach provides a repeatable, automated migration process that can be extended to handle hundreds or thousands of secrets with minimal manual intervention.