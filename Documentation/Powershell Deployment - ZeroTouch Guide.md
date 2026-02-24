# Zero Touch Deployment with PSD

# Prerequisites

Before configuring Zero Touch deployment, ensure:

- PSD is fully installed and functional
- Deployment share is updated
- Boot media or PXE environment is working
- Required task sequence(s) are created using PSD templates

---

# Configuring Zero Touch Deployment

To perform a Zero Touch deployment using the latest PSD release, complete the following steps.

---

## Step 1 – Configure CustomSettings.ini

Edit:

```
Control\CustomSettings.ini
```

Add the following properties:

```ini
SkipBDDWelcome=YES ; or SkipPSDWelcome=YES
SkipDeployReadiness=YES
SkipTaskSequence=YES
SkipDiskSelection=YES
SkipDomainMembership=YES
SkipComputerName=YES
SkipRoleSelection=YES
SkipIntuneGroup=YES
SkipAdminPassword=YES
SkipLocaleSelection=YES
SkipTimeZone=YES
SkipApplications=YES
SkipSummary=YES
SkipPSDWizardSplashScreen=YES
```

### Important

If `SkipPSDWizardSplashScreen=YES` is not included, the splash screen may briefly appear.  
However, the PSD Wizard will still be bypassed and the task sequence will continue automatically.

---

## Step 2 – Define Required Deployment Values

Since all wizard pages are skipped, you must define required properties in `CustomSettings.ini`, such as:

- `TaskSequenceID`
- `ComputerName`
- `JoinDomain` or `JoinWorkgroup`
- `DomainAdmin`, `DomainAdminPassword`, `DomainAdminDomain` (if domain joining)
- `AdminPassword`
- `Applications` (optional)
- Locale settings
- Time zone settings

Without these values defined, deployment may fail.

---

# Known Issues

- There is currently no property to set default values for:
  - **IntuneGroup**
  - **DeviceRole**

  These must be configured manually via the wizard for now.

- The property `SkipWelcome` mentioned in some older documentation is a typo and should not be used.

---

# Tip

Before testing Zero Touch settings:

- Run `diskpart clean`
- Or use the **Prestart Menu** disk wipe option

This ensures no previous partition layout interferes with deployment.

---

# Troubleshooting

If deployment does not behave as expected:

- Review `BDD.log` and `PSD.log`
- Confirm all required properties are defined
- Verify task sequence ID is correct
- Confirm domain join credentials are valid

For additional guidance, review:

**PowerShell Deployment – Latest Release Setup Guide**
