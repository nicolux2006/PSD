# 🚀 PowerShell Deployment (PSD) – Extension Kit for MDT

**Version:** 0.2.3.1  
**Last Update:** December 2025  

PowerShell Deployment (PSD) is a modern deployment framework built on PowerShell, designed to provide the same level of automation as Microsoft Deployment Toolkit (MDT) while moving away from legacy scripting components toward a PowerShell-native architecture.

PSD leverages the familiar MDT Workbench structure but replaces core logic with PowerShell-based components, enabling improved extensibility, maintainability, and modernization.

> ⚠ PSD is actively evolving and considered a work in progress.

---

## ✨ What’s New
### February 2026 (v0.2.3.2)
- General documentation corrections (spelling and minor text fixes)
- Documentation Redesign

### December 2025 (v0.2.3.1)
- Added missing **ZTIGather.xml**
- Updated INI configuration files
- Updated **PSDStart**
  - Removed Legacy Wizard
  - Added support for **UserExitScripts**
- Updated **PSDUtility** to support new PSDStart functions
- Added **UserExitScripts sample folder** (PSDResource)

### September 2024 (v0.2.3.0)
- Updated Deployment Wizard  
  - New panes for **Intune integration**
  - Device role selection
- Improved Task Sequence templates
- Updated installer
- Customized ZTIGather.xml
- Updated Prestart menu

### February 2024 (v0.2.2.9)
- Standardized VMware model alias  
  → All VMware models now defined as `"VMware"`

### September 2022 (v0.2.2.8)
- New Wizard (controlled via `PSDWizard` and `PSDWizardTheme`)
- Improved disk handling
- Extended logging
- Performance improvements
- Code cleanup
- Support for driver packages in **WIM format**
- Server-side logging via **BITS upload** (`SLShare`)
- Major architectural improvements

---

## 🎯 Target Audience

PSD is designed for:

- Infrastructure Architects  
- Solution Architects  
- Modern Workplace / Modern Device Engineers  
- Enterprise Deployment Specialists  

---

## 🏗 Architecture Overview

PSD:

- Is built primarily on **PowerShell**
- Retains MDT Workbench compatibility
- Supports both:
  - PSD-enabled deployment shares
  - Legacy MDT deployment shares

---

## 🌐 Supported Deployment Scenarios

Content can be delivered from:

- **IIS over HTTPS**
  - Using native PowerShell WebClient
- **IIS over HTTPS with BITS & BranchCache**
  - Using 2Pint Software's OSD Toolkit

This enables secure, scalable, and bandwidth-efficient enterprise deployments.

---

## 📚 Documentation

Follow the documentation in this order:

1. Introduction  
2. Pre-Installation (Beginners Guide)  
3. PSD Installation  
4. IIS Configuration (Post-Installation)  
5. Operations Guide  

### Advanced Guides

- Latest Release Setup Guide  
- Security Guide  
- BranchCache Installation Guide  
- Hydration Kit Guide  
- PSD Wizard Guide  
- RestPS Integration Guide  
- ZeroTouch Deployment Guide  

### Reference Documentation

- Toolkit Reference  
- PSD vs MDT Support Matrix  
- FAQ  

> If you encounter issues, review the reference documentation before opening inquiries.

---

## 🔐 Security & Logging

PSD supports:

- Secure HTTPS-based deployments
- BranchCache integration
- Server-side logging via BITS uploads
- Customizable logging framework
- Extended operational diagnostics

---

## ❤️ Credits

This major release was made possible by extensive contributions from:

- Elias Markelis  
- George Simos  
- Dick Tracy  

Hundreds of hours of engineering and testing went into this evolution of PSD.

---

## 🧩 Contributing & Development

This repository serves as a **distribution/download repository**.

The active development repository is private and available by invitation only.

If you are interested in contributing, please contact:

- johan@2pintsoftware.com  
- Mikael.nystrom@truesec.se  

---

## 📌 Project Vision

PSD aims to modernize enterprise deployment by:

- Reducing reliance on legacy scripting
- Increasing transparency through PowerShell
- Enabling modern integration scenarios (Intune, REST, automation)
- Maintaining compatibility with existing MDT investments
