# PSD vs MDT

Although PSD is an extension of MDT and enhances many of its capabilities, it also changes or replaces certain MDT behaviors.

This document clarifies supported features and known differences between PSD and MDT. While the goal is to bring as much MDT functionality as possible into PSD, some features may not be required in a modern deployment model.

---

# Support Matrix

| Feature | PSD | MDT | On Roadmap | Comment |
|----------|-----|-----|------------|----------|
| Windows 10 & 11 OSD | ✔ | ✔ |  | |
| Windows Server 20XX OSD | ✔ | ✔ |  | |
| HTTP(S) Deployments | ✔ |  |  | |
| UNC Deployments |  | ✔ | No | PSD aims to move away from UNC toward modern HTTP-based deployments. |
| WinPE Deployments | ✔ | ✔ |  | |
| Desktop Deployments |  | ✔ | Yes | |
| Capture Deployments |  | ✔ | No | |
| Pause Deployments |  | ✔ | Yes | Similar to LTISuspend. |
| Profile Selection |  |  | Yes | Wizard-based profile pre-selection. |
| LiteTouch OSD | ✔ | ✔ |  | |
| ZeroTouch OSD | ✔ | ✔ |  | See *PowerShell Deployment – ZeroTouch Guide*. |
| Linked Deployment Shares |  | ✔ | No | HTTP deployments reduce the need for this feature. |
| Database Support |  | ✔ |  | |
| MDT Logging (SLShare) | ✔ | ✔ |  | |
| Debugging | ✔ |  |  | |
| Deployment Workbench | ✔ | ✔ |  | Discussion ongoing about a future modern workbench. |
| UDI Designer |  | ✔ | Maybe | |
| Wizard Themes | ✔ |  |  | Currently supports Light and Dark themes. |
| Wizard Custom Pages | ✔ | ✔ |  | PSD uses XAML (fully customizable). MDT uses UDI Designer (limited). |
| Multiple Language Support |  |  | Yes | Currently en-US only; additional languages planned. |
| Organizational Branding | ✔ |  |  | Branding via PSDWizard background and assets. |
| User State Migration |  | ✔ | No | |
| UserExit Scripts |  | ✔ | Yes | |
| Precheck Readiness | ✔ |  |  | Introduced in PSD 2.30. |
| Prestart Menu (Disk wipe, Static IP, etc.) | ✔ |  |  | Introduced in PSD 2.30. |
| BranchCache | ✔ | ✔ |  | |
| Custom Properties | ✔ | ✔ |  | |
| DriverGroups |  | ✔ | Yes | `DriverGroup001` not supported. Use `New-PSDDriverPackage.ps1` instead. |
| Packages |  | ✔ | No | |
| Domain Join | ✔ | ✔ |  | Requires LAN connectivity. Offline Domain Join (ODJ) not supported yet. |
| Event Monitoring | ✔ | ✔ |  | Some limitations apply. |
| WinPE DaRT Support | ✔ |  |  | |
| Workbench DaRT Remote Control |  | ✔ |  | See GitHub Issue #83. |
| Offline Media |  | ✔ | Yes | On the TODO list. |
| Application Bundles |  | ✔ | Yes | |
| Application Deployments Only |  | ✔ | Yes | Part of Desktop Deployment development. |
| Autopilot Support |  |  | Yes | |
| Azure Image Builder Support |  |  | Maybe | Under discussion. |
| Reverse Proxy Support |  |  | Maybe | |
| 802.1X Support |  |  | Maybe | |
| PowerShell 7+ Support |  |  | Unknown | PSD currently uses Windows/WinPE native PowerShell 5.1. Limited testing done with PowerShell 7. |

---

> ⚠ **Note:**  
> Even though some items are listed on the roadmap, there are currently no estimated release dates.
