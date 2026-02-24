# Toolkit Reference

This document provides developer-oriented information for customizing PSD, along with a reference to the scripts and modules used in the solution.

---

# Enable Debugging

To enable debugging mode, add the following to `Bootstrap.ini`:

```
PSDDebug=YES
```

This forces PSD to run in debug mode.

The legacy method of specifying the `-Debug` parameter for `PSDStart.ps1` still works and is useful for interactive troubleshooting in WinPE or Windows.

---

# Logging via Write-PSDLog

All logging in PSD modules and scripts should be performed using the **Write-PSDLog** function located in:

```
PSDUtility.psm1
```

For PSD development, always use this function to capture output for logging and debugging purposes.

## Recommended Syntax

```powershell
Write-PSDLog -Message "$($MyInvocation.MyCommand.Name): <your message or output>"
```

This ensures:

- The calling script name is logged
- Proper formatting for CMTrace compatibility
- Consistent log structure across all PSD components

## Sample Log Output

```text
<![LOG[Get-PSDLocalDataPath: Return the cached local data path if possible]LOG]!>
<time="20:31:39.462+000" date="03-26-2019" component="PSDUtility.psm1:27" context="" type="1" thread="" file="">
```

---

# Scripts, Modules, and Libraries

PSD includes the following PowerShell scripts and modules.

---

# Main PSD Scripts

| Script | Description | Equivalent MDT Script |
|--------|------------|-----------------------|
| **PSDApplications.ps1** | Installs applications defined in task sequence variables `Applications` and `MandatoryApplications`. Downloads content to the PSD cache, validates platform, and checks existing installations. Supports `msiexec.exe`, `.cmd`, and `cscript`. | ZTIApplications.wsf |
| **PSDApplyOS.ps1** | Sets power configuration, applies the OS image using DISM, injects drivers, and modifies boot configuration. | LTIApply.wsf |
| **PSDConfigure.ps1** | Customizes and configures `Unattend.xml`. | ZTIConfigure.wsf |
| **PSDDrivers.ps1** | Copies drivers to the PSD cache on the target system. | ZTIDrivers.wsf |
| **PSDGather.ps1** | Executes `PSDGather.psm1` to collect environment and device information. | ZTIGather.wsf |
| **PSDPartition.ps1** | Partitions and configures disks. Partition layout is hardcoded. **Do not modify.** | ZTIDiskpart.wsf |
| **PSDSetVariable.ps1** | Sets task sequence variables. | ZTISetVariable.wsf |
| **PSDStart.ps1** | Starts or resumes a PSD-enabled task sequence. | LiteTouch.wsf |
| **PSDTBA.ps1** | Placeholder script for components not yet converted to PowerShell. | Various |
| **PSDTemplate.ps1** | Sample template for PSD script development. | N/A |
| **PSDValidate.ps1** | Validates system requirements prior to deployment. | ZTIValidate.wsf |
| **PSDWindowsUpdate.ps1** | Executes Windows Update during deployment. | ZTIWindowsUpdate.wsf |

---

# PSD Modules

| Module | Description |
|--------|------------|
| **PSDGather.psm1** | Gathers system and environment information (primarily via WMI) and processes rule files (`Bootstrap.ini`, `CustomSettings.ini`). Outputs values as task sequence variables. |
| **PSDUtility.psm1** | General utility functions shared across all PSD scripts. |
| **PSDWizard.psm1** | Module for the legacy PSD Wizard. |
| **PSDWizardNew.psm1** | Module for the modern PSD Wizard implementation. |
| **PSDDeploymentShare.psm1** | Handles connection to deployment shares via HTTP/HTTPS or SMB and content retrieval. |

---

# PSD Wizard Files

| File | Description | MDT Replacement |
|------|------------|-----------------|
| **PSDWizard.xaml** | Legacy PSD Wizard template. | Various XML files |
| **PSDWizard.xaml.initialize.ps1** | Initializes wizard content in PSD. | Wizard.hta |

---

# PSD Helper Scripts (Testing & Development)

| Script | Description |
|--------|------------|
| **DumpVars.ps1** | Enumerates all task sequence variables. |
| **PSDHelper.ps1** | Imports core PSD modules for testing purposes. |

---

# MDT Dependencies

PSD utilizes the following MDT components:

- `ZTIGather.xml`
- `ZTIConfigure.xml`

These files remain part of the underlying MDT infrastructure and are referenced by PSD components.

---

This reference guide is intended for developers and advanced administrators extending or customizing PSD functionality.
