# BranchCache Installation Guide

## BranchCache Installation Overview

With support from the free OSD Toolkit by 2Pint Software, you can enable BranchCache (P2P) for OS deployment. This is especially useful when deploying from servers hosted in AWS or Azure, where outbound data transfer costs apply. In addition, P2P can significantly improve deployment speed across local networks.

> **NOTE:** For BranchCache to function properly, your PSD deployment share must be configured for HTTP or HTTPS (HTTPS recommended). Refer to the **PowerShell Deployment – IIS Configuration Guide** for details.

---

## Enable BranchCache on Your Deployment Server

To enable the BranchCache feature on Windows Server, run the following command in an elevated PowerShell prompt:

```powershell
Install-WindowsFeature BranchCache
```

---

## Enable BITS and BranchCache in the PSD Boot Image

For BranchCache to function during OS deployment, the BITS and BranchCache components must be added to the PSD boot image.

### 1. Download and Install OSD Toolkit

Download the OSD Toolkit from 2Pint Software:

https://2pintsoftware.com/products/osd-toolkit

Unpack it to the following folder inside your PSD deployment share:

```
PSDResources\Plugins\OSDToolkit
```

---

### 2. Import a Matching Windows Version

Import a Windows operating system that matches your boot image version.

Example:

If your deployment server uses **Windows ADK 10 2004**, import the `install.wim` from a **Windows 10 2004 x64 ISO (Business Edition)**.

---

### 3. Update CustomSettings.ini

Add the following settings:

```ini
BranchCacheEnabled=YES
SMSTSDownloadProgram=BITSACP.EXE
OSDToolkitImageName=Name (label) of imported operating system
```

---

### 4. Verify PSDUpdateExit.ps1

In the `Scripts` folder of your PSD deployment share, open:

```
PSDUpdateExit.ps1
```

Ensure that the lines under:

```
Added for the OSD Toolkit Plugin
```

are **not commented out**.

---

### 5. Regenerate Boot Images

Update the PSD deployment share and select:

> **Completely regenerate the boot images**

---

### 6. Review the Log File

Check the following log file for errors:

```
PSDResources\Plugins\OSDToolkit\Set-PSDBootImage2PintEnabled.log
```

---

### 7. Update PXE (If Applicable)

If you are using PXE, replace the existing boot image with the updated:

```
LiteTouchPE_x64.wim
```

on your PXE server.

---

# Modify Your Task Sequence

The task sequence must be configured to:

- Enable BITS and BranchCache during WinPE
- Move the BranchCache cache into the full Windows installation at the end of the WinPE phase

---

## Step 1 – Enable BranchCache in WinPE

1. Edit your Task Sequence.
2. After **"Format and Partition Disk (UEFI)"**, create a new group named:

```
2Pint Software
```

3. Inside the group, add a **Run Command Line** action with the following settings:

- **Name:**
```
Enable BranchCache to %_SMSTSMDataPath%
```

- **Command Line:**
```
BCEnabler.exe Enable %_SMSTSMDataPath%\BCCache 2 1337
```

---

## Step 2 – Move BranchCache Cache to Full OS

1. After the **"Configure"** action, create another group named:

```
2Pint Software
```

2. Inside this group, add a **Run Command Line** action with the following settings:

- **Name:**
```
Move BranchCache Cache
```

- **Command Line:**
```
BCEnabler.exe Move %OSVolume%:
```
