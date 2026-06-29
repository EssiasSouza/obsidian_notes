# OpenVPN SAML Certificate Renewal

**Date:** 2026-06-24
**People Involved:**
- Ian Karlo Torres Dos Santos (DevOps)
- Essias Alves Souza (Analista Cloud)

## Problem Description
Users were unable to access the VPN via OpenVPN. The issue appeared to be related to a malformed or invalid certificate.
The specific error message displayed was:
```text
Google
400. That's an error.

Error: malformed_certificate

Invalid Certificate:
MIDdC.......
```

## Analysis Methods (Troubleshooting)
While trying to map and understand the complete access flow for the VPN, Ian reviewed the Google Workspace panel. He noticed that under **Google Workspace > Apps > Apps da Web e para dispositivos móveis**, all the certificates were listed as expired. This confirmed the root cause of the SAML authentication failure.

## Solution Execution

**Part 1: Generating and downloading the new certificate in Google Workspace**
1. Navigate to **Google Workspace > Segurança > Autenticação > SSO com o Google como provedor de identidade SAML**.
2. Click the **"ADICIONAR CERTIFICADO"** button. This automatically generates a new valid certificate.
3. Scroll to the **"Metadados do IdP"** section.
4. Click **"FAZER DOWNLOAD DOS METADADOS"** to download the new metadata file (this will save a file named `GoogleIDPMetadata.xml`).

**Part 2: Applying the new certificate in OpenVPN**
1. Access the OpenVPN admin panel at: `https://vpn.gringo.com.vc/admin/`
2. Navigate to the menu: **AUTHENTICATION > SAML**.
3. Expand the section **"Configure Identity Provider (IdP) Automatically via Metadata"**.
4. In the field **IdP Metadata URL**, enter the SSO URL from Google Workspaces.
5. Click **"choose file"** and select the downloaded `GoogleIDPMetadata.xml` file.
6. Click **"Upload"**.
7. Click **"Save Settings"**.
8. Finally, click **"Update Running Server"** to apply the changes and restore VPN access.
