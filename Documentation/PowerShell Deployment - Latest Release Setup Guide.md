# Setting Up PSD 2.30

---

# Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Upgrade Process](#upgrade-process)
   - [Backup Existing Configuration](#backup-existing-configuration)
   - [Download PSD 2.30](#download-psd-230)
   - [Install PSD 2.30](#install-psd-230)
4. [Review PSD Installation](#review-psd-installation)
5. [Editing Configuration Files](#editing-configuration-files)
   - [CustomSettings.ini](#customsettingsini)
   - [Bootstrap.ini](#bootstrapini)
   - [PSDUpdateExit.ps1](#psdupdateexitps1)
6. [Final Steps](#final-steps)
7. [Troubleshooting](#troubleshooting)

---

# Introduction

This guide provides step-by-step instructions for setting up PSD 2.30, including the upgrade process and required configuration file changes.

---

# Prerequisites

- Administrative access to the MDT/PSD server
- Existing PSD installation (for upgrade scenarios)
- Backup of critical deployment share data

---

# Upgrade Process

---

## Backup Existing Configuration

1. Navigate to your existing PSD deployment share.
2. Open the **Control** folder.
3. Back up the following files:

   - `CustomSettings.ini`
   - `Bootstrap.ini`

> ℹ The `Install-PSD.ps1` script with the `-upgrade` switch automatically backs up these files (and additional content) into a `Backup` folder (e.g., `PSD_0001`).

---

## Download PSD 2.30

1. Go to the official PSD GitHub repository:  
   https://github.com/FriendsOfMDT/PSD
2. Click the green **`<> Code`** button.
3. Select **Download ZIP**.
4. Extract the files to your desired directory.

> ℹ If downloading as a ZIP file, right-click → **Unblock** before extracting.

---

## Install PSD 2.30

1. Open **PowerShell as Administrator**.
2. Follow the instructions in the official Installation Guide.

Use the `-upgrade` switch if upgrading an existing deployment share.

---

# Review PSD Installation

Review the `Install-PSD.log` file to confirm all files were properly updated.

### Verify Updated Files

- `Scripts\ZTIGather.xml`
- `Tools\Modules\PSDGather\ZTIGather.xml`
- `Tools\Modules\PSDWizardNew\PSDWizardNew.psm1`

### Verify Required Folders

- `Scripts\PSDWizardNew\Themes\Classic`
- `Scripts\PSDWizardNew\Themes\Dark`

### Verify Required Files

- `PSDResources\Readiness\Computer_Readiness.ps1`

---

# Editing Configuration Files

For upgrade installations, complete the following steps.

---

## CustomSettings.ini

1. Back up `Control\CustomSettings.ini`.
2. Replace the `[Settings]` and `[Default]` sections with the current version from:
   https://github.com/FriendsOfMDT/PSD/blob/master/INIFiles/CustomSettings.ini
3. Reconfigure environment-specific settings.
4. Ensure at least one Applications entry exists:

```ini
Applications001={d7f2f50a-e85f-425e-a2f7-68392b1f31a6}
```

5. Save and close the file.

---

## Bootstrap.ini

1. Back up `Control\Bootstrap.ini`.
2. Replace the `[Settings]` and `[Default]` sections with:
   https://github.com/FriendsOfMDT/PSD/blob/master/INIFiles/Bootstrap.ini
3. Ensure `PSDDeployRoots` reflects your deployment share URL.
4. Save and close the file.

---

## PSDUpdateExit.ps1

If custom modifications were previously added, reapply them after the upgrade.

---

# Final Steps

1. Update the Deployment Share.
2. Select **Completely regenerate the boot images**.
3. Rebuild the ISO if required.
4. Update PXE boot images if applicable.

---

# Troubleshooting

If issues occur:

- Review `Install-PSD.log`
- Review deployment logs (`PSD.log`, `BDD.log`)
- Confirm INI sections were fully replaced
- Verify required folders and files exist

For additional help, review official PSD documentation or open an issue on GitHub.
