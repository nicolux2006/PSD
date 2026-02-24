# PSD Installation Guide

---

## PSD Installation Overview

The PSD installation script is used to either create a new MDT deployment share or extend an existing one.  

It is also possible to use the Hydration script on a new server to automatically set up a PSD lab environment. For that scenario, refer to the **Hydration Kit Installation Guide**.

> ⚠ **WARNING:** We strongly recommend creating a *new* deployment share for PSD and copying existing resources (applications, drivers, images) into it.  
> Once a deployment share has been extended with PSD, standard MDT task sequences will no longer function in that share.

---

# PSD Installer – Supported Configurations

The PSD installer has been tested with the following configurations:

## Server Operating Systems

- Windows Server 2016  
- Windows Server 2019  
- Windows Server 2022  

## Windows ADK

- Windows ADK for Windows 11 24H2 (Build 26100)  
- WinPE Add-on for Windows ADK for Windows 11 24H2  

## Microsoft Deployment Toolkit (MDT)

- MDT 8456  

---

# PSD Supported Deployment Operating Systems

The following operating systems have been validated for deployment via PSD:

## Server Operating Systems

- Windows Server 2016 Standard & Datacenter
- Windows Server 2019 Standard & Datacenter 
- Windows Server 2022 Standard & Datacenter
- Windows Server 2025 Standard & Datacenter

## Client Operating Systems

- Windows 10 1909 Home, Pro, Education, Enterprise x64
- Windows 10 2004 Home, Pro, Education, Enterprise x64
- Windows 10 20H2 Home, Pro, Education, Enterprise x64
- Windows 10 21H1 Home, Pro, Education, Enterprise x64
- Windows 10 21H2 Home, Pro, Education, Enterprise x64
- Windows 10 22H2 Home, Pro, Education, Enterprise x64
- Windows 11 21H2 Home, Pro, Education, Enterprise x64
- Windows 11 22H2 Home, Pro, Education, Enterprise x64
- Windows 11 23H2 Home, Pro, Education, Enterprise x64
- Windows 11 24H2 Home, Pro, Education, Enterprise x64
- Windows 11 25H2 Home, Pro, Education, Enterprise x64

---

# PSD Installation Checklist

Before installing PSD, ensure the following prerequisites are met:

---

## Windows ADK

Download and install a supported Windows ADK version on the server hosting the MDT Deployment Workbench.

- Latest version:  
  https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install

- Older versions:  
  https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install#other-adk-downloads

---

## MDT

Download and install Microsoft Deployment Toolkit (MDT) on the MDT server.

Additionally, install **KB4564442 HotFix** for MDT 8456.

- MDT Installer:  
  https://www.microsoft.com/en-us/download/details.aspx?id=54259

- MDT 8456 HotFix:  
  https://support.microsoft.com/en-us/topic/windows-10-deployments-fail-with-microsoft-deployment-toolkit-on-computers-with-bios-type-firmware-70557b0b-6be3-81d2-556f-b313e29e2cb7

---

## Source Media

Ensure the following source media are available:

- Windows OS installation media  
- Application installation sources  
- OEM hardware drivers  

---

## Optional Components

- **WDS (Windows Deployment Services)**  
  Required for PXE-based deployments.

---

## Required Accounts

You will need accounts with sufficient permissions for:

- Build Account (accessing PSD/MDT shares)  
- Domain Join Account (joining devices to Active Directory)  
  > Requires line-of-sight connectivity to a domain controller.

---

# Installing PSD

PSD installation requires:

- An existing installation of MDT and Windows ADK  
- Administrative rights on the MDT server  
- The PSD solution downloaded locally  

> ⚠ **WARNING:** Create a new deployment share for PSD whenever possible. Extending an existing MDT share will break standard MDT task sequences in that share.

> ℹ Existing MDT scripts are moved to a backup folder inside the deployment share during installation.

---

## Installation Steps

### 1. Close MDT Workbench

If open, close the **MDT Deployment Workbench**.

---

### 2. Download PSD

Download or clone PSD from:

https://github.com/FriendsOfMDT/PSD

> ℹ If downloading the ZIP archive, right-click the file → **Unblock** before extracting.

---

### 3. Run the Installation Script

Open an elevated PowerShell prompt and run one of the following:

---

## NEW PSD Installation

```powershell
.\Install-PSD.ps1 -psDeploymentFolder "<absolute folder path>" -psDeploymentShare "<share name>"

# Example:
.\Install-PSD.ps1 -psDeploymentFolder "D:\PSD" -psDeploymentShare "dep-psd$"
```

---

## Upgrade Existing MDT/PSD Installation

```powershell
.\Install-PSD.ps1 -psDeploymentFolder "<absolute folder path>" -psDeploymentShare "<share name>" -upgrade

# Example:
.\Install-PSD.ps1 -psDeploymentFolder "D:\PSD" -psDeploymentShare "dep-psd$" -upgrade
```

---

### 4. Review Installation Logs

Review the PSD installation log for errors.

---

### 5. Validate Setup

Review the **Latest Release Setup Guide** to ensure everything is configured correctly.

---

# Next Steps

After completing the PSD installation:

## Enable HTTPS Deployments

Follow:

**PowerShell Deployment – IIS Configuration Guide**

---

## Enable BranchCache (Optional – P2P Support)

If enabling BranchCache, follow these guides **in order**:

1. PowerShell Deployment – IIS Configuration Guide  
2. PowerShell Deployment – BranchCache Installation Guide  

---

PSD is now ready for further configuration and deployment customization.
