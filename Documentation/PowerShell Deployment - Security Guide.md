# Security Guide – PowerShell Deployment Extension Kit

# Security Overview

Security for PSD-enabled MDT solutions is fundamentally similar to that of a traditional MDT environment.

Key considerations:

- Passwords are hidden and obfuscated in logs.
- Credentials may still be transmitted in clear text if using HTTP.
- HTTPS is strongly recommended for all production deployments.
- Proper network segmentation and least-privilege access should be enforced.

---

# Accounts and Permissions

## PSD Installation

Installing PSD requires:

- Local administrative privileges
- Running `Install-PSD.ps1` from an elevated PowerShell session

---

# PSD / MDT Shares and Content

For PSD-enabled task sequences to function, the following are required:

- A PSD deployment share (optionally hidden, e.g., `share$`)
- Account credentials defined in:
  - `Bootstrap.ini`
  - `CustomSettings.ini`

Required properties:

- `UserID`
- `UserPassword`
- `UserDomain`

Required permissions:

- Share permissions: **READ**
- NTFS file permissions: **READ**

> 🔐 Follow the principle of least privilege.  
> Only grant additional rights if explicitly required.

---

# Progress and Status Logs

Deployment progress and logging rely on:

- Account specified in `Bootstrap.ini` / `CustomSettings.ini`
  - `UserID`
  - `UserPassword`
  - `UserDomain`
- `SLShare` property (server-side logging location)

Ensure:

- Write permissions are granted only to the logging share
- Log share access is restricted to required systems

---

# Active Directory Integration

If joining devices to Active Directory, configure the following in `CustomSettings.ini`:

- `JoinDomain` – Target domain name
- `DomainAdmin` – Account with rights to create/delete computer objects
- `DomainAdminPassword` – Account password
- `DomainAdminDomain` – Domain of the service account

> ⚠ Best Practice:
>
> - Use a dedicated service account.
> - Delegate permissions only to the required OU.
> - Do not use Domain Administrator accounts.

---

# Event Monitoring

If MDT Event Monitoring is enabled:

- Ensure the monitoring ports are accessible.
- Restrict access via firewall rules where possible.
- Monitor logs for unauthorized access attempts.

---

# Firewall Ports

In addition to IIS configuration, the following firewall ports may be required:

- **Port 80** – HTTP (Not recommended)
- **Port 443** – HTTPS (Recommended)
- **Port 9080** – MDT Event Monitoring (if enabled; disabled by default)

> 🔐 Production environments should:
>
> - Disable HTTP (Port 80)
> - Enforce HTTPS only
> - Restrict monitoring ports to trusted subnets

---

# Security Recommendations

- Use HTTPS for all deployments.
- Store certificates securely and maintain the full certificate chain.
- Remove unused scripts and legacy components.
- Regularly rotate service account passwords.
- Restrict share access to deployment service accounts only.
- Monitor deployment and event logs for anomalies.
- Use network segmentation for deployment infrastructure.

---

PSD security posture largely mirrors MDT, but modern deployment scenarios (HTTP/HTTPS, REST, cloud integration) require additional attention to certificate management, endpoint exposure, and service account governance.
