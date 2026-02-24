# Frequently Asked Questions – PowerShell Deployment Extension Kit

This document highlights known issues and limitations of PSD (as of the published date above).

---

# Frequently Asked Questions

### Q: Does the installer copy my existing MDT Deployment Share content (e.g., applications, drivers, task sequences) to a new PSD share?

> **A:** No. Users and administrators must copy or export existing components to *new* PSD shares using the built-in content management features of MDT.  
> MDT shares that have been PSD-upgraded will continue to have access to existing objects and artifacts.

---

### Q: Can the installer (PSD_Install.ps1) be executed remotely?

> **A:** No. `PSD_Install.ps1` must be run locally with administrative rights on the target MDT installation.

---

### Q: Does the installer copy my existing Bootstrap.ini or CustomSettings.ini files to the PSD repositories?

> **A:** No. If you create a new PSD-enabled deployment share, you must manually copy or recreate any existing Bootstrap.ini and CustomSettings.ini files.

---

### Q: What are the client/target hardware requirements for bare-metal PSD deployments?

> **A:** PSD requires approximately the same hardware as Windows 10 and MDT deployments:

- Minimum 1.5 GB RAM (WinPE is extended and requires additional memory)
- At least one network adapter
- At least one 50 GB hard drive (for New/Bare-Metal deployments)
- A supported modern processor (Windows 10 requirements apply)

---

### Q: Are system clocks synchronized?

> **A:** Yes. `PSDStart.ps1` attempts to synchronize the time on deployment target computers. Deployment root servers and HTTP/S content servers should be NTP-synchronized.

---

### Q: Does PSD work with 2Pint’s OSD Toolkit?

> **A:** Yes. See the **BranchCache Installation Guide**.

---

### Q: Why does the PowerShell window flash and change size?

> **A:** This behavior is by design. `PSDStart` resizes the PowerShell window during startup. You will notice it change from full screen to approximately one-third of the screen early in the boot process.

---

### Q: What is "Transcript Logging"?

> **A:** Standard logs (e.g., `PSD.log`, `BDD.log`) contain explicitly written log entries.  
> Transcript logs capture everything displayed on screen during execution. PSD transcript logs are extremely useful for troubleshooting but may be visually less structured.

---

### Q: Why do I frequently see "Stopping Transcript Logging"?

> **A:** (TBD)

---

### Q: Do I need to add PowerShell support to my WinPE images?

> **A:** **No.** PSD installation and scripting handle this automatically.  
Even if PowerShell is unchecked in the MDT WinPE Features tab, PSD will still inject PowerShell into the boot image.

---

### Q: Will PSD work on my specific version of MDT or ADK?

> **A:** PSD has only been developed and tested against the versions listed below.  
If you successfully test PSD on additional versions or platforms, please inform the maintainers.

---

### Q: Which MDT components are injected into the PSD Boot Media?

> **A:** As defined by **LiteTouchPE.xml**, the following components are injected by default:

**WinPE Components**
- winpe-dismcmdlets
- winpe-enhancedstorage
- winpe-fmapi
- winpe-hta
- winpe-netfx
- winpe-powershell
- winpe-scripting
- winpe-securebootcmdlets
- winpe-securestartup
- winpe-storagewmi
- winpe-wmi

---

### Q: Which PSD-specific modules and scripts are included in Boot Media?

**Modules**
- PSDUtility.psm1
- PSDGather.psm1
- PSDWizard.psm1
- PSDWizardNew.psm1
- PSDDeploymentShare.psm1
- ZTIGather.xml
- Interop.TSCore.dll
- Microsoft.BDD.TaskSequenceModule.dll
- Microsoft.BDD.TaskSequenceModule.psd1

**Scripts**
- PSDStart.ps1
- PSDHelper.ps1

**Configuration Files**
- Bootstrap.ini
- Unattend.xml
- winpeshl.ini

---

### Q: What scripts or files can safely be deleted from my PSD Deployment Share?

> **A:** This is still being refined. For now:

- Do **NOT** delete any `PSD*.ps1` or `PSD*.psm1` files.
- Do **NOT** delete `ZTIGather.xml` or `ZTIConfigure.xml`.
- If installing applications during a task sequence, do **NOT** delete `ZTIUtility.vbs`, as it may be required.
- Many legacy MDT `.wsf` scripts and wizard files can be manually removed from the Scripts folder if desired.

---

# Documented Platforms and Scenarios

### Tested Components

| Component | Version | Notes |
|------------|----------|--------|
| MDT | 6.3.8456.1000 (8456) | |
| ADK | 10.1.26100.2454 (24H2) | |
| MDT WinPE Add-on | 10.1.26100.2454 (24H2) | |
| Target Client OS | Windows 11 24H2, 25H2 | MSDN media tested |
| Host Server OS | Windows Server 2022 | |
| Virtualization | KVM | Client and host testing performed |

---

### Deployment Scenarios

| Scenario | Status |
|----------|--------|
| BareMetal via UNC | Tested and working |
| BareMetal via HTTP | Tested and working |
| BareMetal via HTTPS | Tested and working |
| Refresh (UNC/HTTP/HTTPS) | Not yet implemented |
| Replace (UNC/HTTP/HTTPS) | Not yet implemented |
| BIOS-to-UEFI | Not yet implemented |
| Generic non-OSD Task Sequence | Not yet implemented |

---

# Installation Observations

- The PSD installer creates the `-psDeploymentShare` exactly as specified.  
  It does **NOT** modify or handle the hidden share `$` character.

- The installer should automatically mount a newly created PSD deployment share.  
  You may need to refresh the MDT Workbench manually.

- The installer does **NOT** copy existing MDT artifacts into a new PSD share.  
  Applications, drivers, and task sequences must be manually re-imported.

---

# Operational Observations

Refer to the **PSD Installation Guide** for additional configuration recommendations.

Important notes:

- Applications referenced in `Bootstrap.ini` or `CustomSettings.ini` **must** include `{}` brackets around their GUID.
- New Task Sequence variables must be explicitly declared in `Bootstrap.ini` or `CustomSettings.ini`.
- During post-OS installation (OOBE), the PowerShell window may briefly flash or refresh. This is expected behavior when scripts execute.
