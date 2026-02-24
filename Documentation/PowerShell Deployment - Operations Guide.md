# Operations Guide

This guide covers standard operational procedures when using PSD.  
For the latest updates, refer to the:

**PowerShell Deployment – Latest Release Setup Guide**

---

# Introduction

PSD-enabled deployments function similarly to standard MDT Lite Touch deployments.

They can be initiated via:

- PXE
- Boot Media (ISO / USB)

Below is a list of common operational tasks when working with PSD.

---

# Import Operating Systems

Within the MDT Deployment Workbench, on the PSD deployment share:

- Import
- Create
- Or copy desired operating systems

Follow standard MDT procedures.

> 💡 **Pro Tip:** You can copy operating systems from other MDT deployment shares directly within the Workbench.

---

# Create PSD Task Sequences

You **must** create new task sequences using the **PSD Templates** provided in the Workbench.

PSD will fail if:

- You clone
- Copy
- Import
- Or modify standard MDT task sequences instead of using PSD templates

Some steps in the PSD template are required for proper functionality.

> ⚠ Do not delete PSD-specific task sequence steps.  
> You may disable steps if necessary.

> 💡 **Pro Tip:** After upgrading PSD, expect to recreate task sequences using the updated PSD templates.

---

# Create Applications

Within the PSD deployment share:

- Import
- Create
- Or copy desired applications

Follow standard MDT methods.

Make note of each application's GUID if automating installations via `CustomSettings.ini`.

> 💡 **Pro Tip:** Applications can be copied from other MDT deployment shares.

> ⚠ **Known Issue:**  
> Ensure at least one dummy application entry exists in `CustomSettings.ini`.  
> Without this, application selection in the new PSDWizard may fail.

---

# Import / Add Drivers

Within the PSD deployment share:

- Import drivers using the "Total Control Method"  
  (OS / Make / Model or OS / Model structure)

After importing drivers, run:

```
New-PSDDriverPackage.ps1
```

This generates ZIP or WIM driver archives.  
One archive is created per OS and Model.

---

## Sample Syntax

```powershell
.\New-PSDDriverPackage.ps1 -psDeploymentFolder "E:\PSDProduction" -CompressionType WIM

.\New-PSDDriverPackage.ps1 -psDeploymentFolder "E:\PSDProduction" -CompressionType ZIP
```

> 💡 **Pro Tip:**  
> You can copy drivers from other MDT shares.  
> PSD also supports importing prebuilt WIM or ZIP driver packages.

---

# Check Deployment Share Permissions

The PSD installer:

- Creates the required MDT folder structure
- Adds PSD scripts and templates
- Creates an SMB share (if specified)

Ensure proper permissions are configured.

> 💡 **Pro Tip:**  
> Grant only the **minimum required rights**.  
> PSD/MDT shares require **READ** access only.  
> Grant write access only where logging is required.

---

# Update Windows PE Settings

Review MDT WinPE configuration panels and configure:

- Custom wallpaper
- Extra directory (configured by default)
- ISO file name and generation
- WIM file name and generation

---

# Enable MDT Monitoring

Enable MDT Event Monitoring and configure:

- Server name
- Port numbers

---

# Update CustomSettings.ini

Edit and customize `CustomSettings.ini` to control automation and OS configuration.

These settings typically affect:

- Installed OS configuration
- Application installation
- Domain join
- Task sequence automation

Ensure new PSD properties and variables are configured.

> 💡 **Pro Tip:**  
> Use the latest `CustomSettings.ini` from the PSD repository and migrate your existing settings carefully.

---

# Update Bootstrap.ini

Edit and customize `Bootstrap.ini` to configure:

- Deployment share access
- Authentication
- Pre-OSD environment settings

Ensure new PSD properties are configured.

> 💡 **Pro Tip:**  
> If using `PSDDeployRoots`, remove **all** references to `DeployRoot` from `Bootstrap.ini`.

---

# Update Background Wallpaper

PSD includes a themed background file:

```
PSDBackground.bmp
```

Located in the MDT Samples folder.

Update the MDT WinPE Customization tab to use this file (or your own).

---

# Configure Extra Files

Create an `ExtraFiles` folder to include additional files in WinPE or the deployed OS.

Examples:

- CMTRACE.EXE
- Custom wallpapers
- Scripts
- Tools

> 💡 **Pro Tip:**  
> Mirror the target directory structure.  
> Example:
>
> ```
> ExtraFiles\Windows\System32
> ```

---

# Readiness Checks

PSD runs a default readiness script:

```
PSDResources\Readiness\Computer_Readiness.ps1
```

You may:

- Modify this script
- Replace it with your own

If the Deployment Readiness page is enabled, a valid file path must be configured in `CustomSettings.ini`.

To disable:

```
SkipReadinessCheck=YES
```

---

# Certificates

For HTTPS deployments:

- Export the full certificate chain
- Place certificates in:

```
PSDResources\Certificates
```

This ensures certificates are injected into WinPE.

---

# Configure WinPE Drivers

Use MDT Selection Profiles to define:

- WinPE driver sets
- Applications
- Packages
- Task sequences

Ensure WinPE includes required network and storage drivers.

---

# Generate New Boot Media

Using the MDT Deployment Workbench:

- Update the deployment share
- Generate new boot media

By default, new PSD shares generate:

- `PSDLiteTouch_x64.iso`
- `PSDLiteTouch_x86.iso`

Rename if necessary.

---

# Content Caching and Peer-to-Peer Support

To enable P2P functionality, refer to the:

**PowerShell Deployment – BranchCache Installation Guide**
