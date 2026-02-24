# Beginner’s Guide for ADK and MDT

Welcome to this beginner-friendly guide for installing the Windows ADK (Assessment and Deployment Kit) and Microsoft Deployment Toolkit (MDT).

If you're new to OS deployment – you're in the right place. By following this guide, you'll establish a solid foundation for working with MDT and PSD.

---

# Prerequisites

Before starting, ensure you have:

- A server running a supported Windows Server operating system
- Administrative privileges on the server
- An internet connection for downloading required components

---

# Step 1 – Download and Install Windows ADK

## 1. Download Windows ADK

- Go to the official Microsoft ADK page:  
  https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install

- Download the installer for the latest supported version.

> 💡 **Tip:** Look for a link similar to:  
> *Download the Windows ADK \<version\> (\<date\>)*

---

## 2. Run the ADK Installer

1. Launch `adksetup.exe`.
2. Choose the installation path.
3. Select the required features.

### Recommended Features for MDT

- ✅ Deployment Tools  
- ✅ Imaging and Configuration Designer (ICD)  
- ✅ Configuration Designer  
- ✅ User State Migration Tool (USMT)

4. Click **Install** and wait for completion.

---

# Step 2 – Install the WinPE Add-on

WinPE is required for boot image creation and deployment.

## 1. Download the WinPE Add-on

- From the same ADK page:  
  https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install

- Download the matching WinPE Add-on for your ADK version.

> 💡 **Tip:** Look for:  
> *Download the Windows PE add-on for the Windows ADK \<version\> (\<date\>)*

---

## 2. Run the WinPE Installer

1. Launch `adkwinpesetup.exe`.
2. Accept defaults unless customization is required.
3. Click **Install** and wait for completion.

---

# Step 3 – Install Microsoft Deployment Toolkit (MDT)

## 1. Download MDT

- Official download page:  
  https://www.microsoft.com/en-us/download/details.aspx?id=54259

Download the latest available version (MDT 8456).

---

## 2. Run the MDT Installer

1. Launch `MicrosoftDeploymentToolkit_x64.msi`.
2. Follow the on-screen prompts.
3. Complete the installation.

---

# Step 4 – Important Notes Before Launching MDT

⚠ **Do NOT launch the Deployment Workbench yet**  
If you plan to use PSD, the PSD installation process will create and configure the deployment share for you.

---

## Important Support Notes

- MDT is no longer officially supported by Microsoft.
- x86 deployments are deprecated.
- Windows 11 deployments require additional adjustments.
- PSD extends MDT functionality and enables continued Windows 11 deployment support.

---

# Recommended Post-Installation Preparation

To avoid common issues:

---

## Create Missing WinPE Folder (If Required)

Run the following command as Administrator on the MDT server:

```cmd
mkdir "C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\x86\WinPE_OCs"
```

---

## Download Required Content

Before creating a deployment share, gather:

- OEM hardware drivers for your target devices
- Windows OS ISO files  
  Official source:  
  https://www.microsoft.com/en-us/software-download/windows11

- Application installers required for deployment

> ℹ Application setup procedures are outside the scope of this guide.  
> Many step-by-step MDT application packaging guides are available online.

---

# Helpful Reference

- https://www.deploymentresearch.com/building-a-windows-11-24h2-reference-image-using-microsoft-deployment-toolkit-mdt/

---

# Conclusion

You have successfully installed:

- Windows ADK  
- WinPE Add-on  
- Microsoft Deployment Toolkit (MDT)  

You are now ready to proceed with deployment configuration.

---

# Next Step

To continue with PowerShell Deployment (PSD) setup, refer to:

**PowerShell Deployment – Installation Guide**
