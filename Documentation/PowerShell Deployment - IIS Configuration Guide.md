# IIS Configuration Guide

---

## Introduction

To enable OS deployments via HTTPS, several configuration steps must be completed. This document outlines the required settings and procedures known to work with PSD, including configuration steps for **server-side logging via BITS upload** (available in PSD version 0.2.2.8 and later).

> **NOTE:** Your security team or environment may require additional hardening or lockdown measures.

---

## Supported Operating Systems (HTTP/HTTPS Imaging)

The IIS setup for PSD has been tested on the following server operating systems:

- Windows Server 2016  
- Windows Server 2019  
- Windows Server 2022  

---

# Install IIS and Configure WebDAV

To install IIS and configure WebDAV for OS deployment, you must run two scripts — one for setup and one for configuration — with a reboot in between.

---

## Step 1 – Install IIS

Run the first script (`New-PSDWebInstance.ps1`) without parameters. After completion, reboot the server.

The script is located in the **Tools** folder of PSD.

```powershell
.\New-PSDWebInstance.ps1
```

> **NOTE:** The IIS setup script does NOT support servers where IIS is already installed. It must be run on a clean Windows Server installation.

---

## Step 2 – Configure IIS

After rebooting, run the second script (`Set-PSDWebInstance.ps1`), specifying:

- The deployment folder
- The name of the virtual directory to create

The script is also located in the **Tools** folder.

Example:

```powershell
.\Set-PSDWebInstance.ps1 -psDeploymentFolder "E:\PSDProduction" -psVirtualDirectory "PSDProduction"
```

---

# HTTPS and Certificates

To enable HTTPS communication:

1. Install a valid web server certificate.
2. Ensure the Root CA certificate is injected into WinPE.

If you export your Root CA certificate to:

```
PSDResources\Certificates
```

PSD will automatically inject it into WinPE when updating the deployment share.

---

## Self-Signed Certificates (Lab Use Only)

For lab environments, PSD provides two scripts:

- `New-PSDRootCACert.ps1` – Creates a local Root CA and exports it to the PSDResources\Certificates folder.
- `New-PSDServerCert.ps1` – Creates a server certificate and binds it to IIS **Default Web Site**.

> These scripts replace the legacy `New-PSDSelfSignedCert.ps1` from earlier releases.

### Example – Create Root CA

```powershell
.\New-PSDRootCACert.ps1 -RootCAName PSDRootCA -ValidityPeriod 20 -psDeploymentFolder "E:\PSDProduction"
```

### Example – Create Server Certificate

```powershell
.\New-PSDServerCert.ps1 -DNSName mdt01.corp.viamonstra.com -FriendlyName mdt01.corp.viamonstra.com -ValidityPeriod 5 -RootCACertFriendlyName PSDRootCA
```

---

# Server-Side Logging via BITS Upload

To enable server-side logging using BITS upload, IIS must be configured accordingly.

Run the following script to create the BITS upload folder and virtual directory:

```powershell
.\Set-PSDLogInstance.ps1 -psLogFolder "E:\PSDProductionLogs" -psVirtualDirectory "PSDProductionLogs"
```

Additionally, add the following settings to **CustomSettings.ini**:

```ini
LogUserDomain=ServernameOrDomain
LogUserID=AccountName
LogUserPassword=Password
SLShare=https://mdt01.corp.viamonstra.com/PSDProductionLogs
```

The specified account must exist either:

- In the local SAM database  
- Or in Active Directory  

(depending on your environment)

---

# Firewall Ports

In addition to IIS configuration, the following firewall ports must be open:

- **Port 443** – HTTPS
- **Port 9800 and 9801** – MDT Event Monitoring (optional, disabled by default)

---

# IIS Setup Reference

This section lists all IIS components installed by the setup script and describes the configuration changes applied.

---

## Required IIS Components

The following IIS components are required for PSD imaging via HTTP/HTTPS:

### Common HTTP Features
- Default Document  
- Directory Browsing  
- HTTP Errors  
- Static Content  
- HTTP Redirection  
- WebDAV Publishing  

### Health and Diagnostics
- HTTP Logging  
- Custom Logging  
- Logging Tools  
- Request Monitor  
- Tracing  

### Performance
- Static Content Compression  

### Security
- Request Filtering  
- Basic Authentication  
- Digest Authentication  
- URL Authorization  
- Windows Authentication  

### Management Tools
- IIS Management Compatibility  
- IIS 6 Management Compatibility  
  - IIS 6 Metabase Compatibility  

---

# IIS Configuration Changes

PSD requires several IIS configuration adjustments, primarily to ensure proper WebDAV functionality.

If you use the configuration script, these changes are applied automatically.

Required settings include:

- Create a new Virtual Directory
- Enable Directory Browsing
- Disable Anonymous Authentication
- Enable Windows Authentication
- Create and register required MIME types

---

# WebDAV Configuration Changes

PSD also requires specific WebDAV configuration adjustments. These are automatically applied if you use the configuration script.

Required settings include:

- Enable WebDAV
- Create a new WebDAV Authoring Rule
- Modify WebDAV settings:
  - Allow file extension filtering
  - Allow hidden segment filtering
  - Allow verb filtering
- Modify default MIME types

---

# Next Steps

To enable optional **BranchCache (P2P)** support, continue with:

**PowerShell Deployment – BranchCache Installation Guide**
