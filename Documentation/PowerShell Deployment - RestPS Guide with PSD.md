# RestPS Guide with PSD

For complete RestPS documentation, see:  
https://github.com/jpsider/RestPS

---

# Scripted Installation

A helper script is included in the PSD **Tools** folder to install and configure RestPS automatically.

## Requirements

- A valid SSL certificate
- NSSM (Non-Sucking Service Manager)  
  https://nssm.cc/download

---

## Example – Scripted Setup

```powershell
.\New-PSDRestPS.ps1 `
    -RestPSRootPath "D:\RestPS" `
    -PathtoNSSMexe "D:\NSSM\nssm.exe" `
    -RestPSListenerPort 8080 `
    -SecretKey "SuperSecret" `
    -CertificateFriendlyName "RestPS" `
    -Test
```

---

# Manual Installation

You may install RestPS manually on:

- The PSD server  
- Or another server accessible by deployment clients  

---

## Step 1 – Install the Module

```powershell
Install-Module RestPS -Force
```

---

## Step 2 – Create a Workspace

```powershell
Invoke-DeployRestPS -LocalDir 'C:\RestPS'
```

At this point, RestPS is installed but not yet running.

---

# Starting the RestPS Listener

RestPS only runs while a listener session is active.

Start a listener by running:

```powershell
$RestPSparams = @{
    RoutesFilePath = 'C:\RestPS\endpoints\RestPSRoutes.json'
    Port           = '8080'
}
Start-RestPSListener @RestPSparams
```

---

# Testing RestPS

RestPS includes sample endpoints in:

```
C:\RestPS\endpoints\RestPSRoutes.json
```

Each entry defines:

- `RequestType`
- `RequestURL`
- `RequestCommand`

---

## Example Endpoint – Inline Command

```json
{
  "RequestType": "GET",
  "RequestURL": "/proc",
  "RequestCommand": "Get-Process -ProcessName PowerShell -ErrorAction SilentlyContinue | Select-Object ProcessName,Id"
}
```

Call it from another PowerShell window:

```powershell
Invoke-RestMethod -Uri http://localhost:8080/proc -Method Get
```

The URI structure is:

```
http://<server>:<port>/<RequestURL>
```

---

## Example Endpoint – Script Execution

```json
{
  "RequestType": "GET",
  "RequestURL": "/process",
  "RequestCommand": "C:/RestPS/endpoints/GET/Invoke-GetProcess.ps1"
}
```

This executes the script:

```
C:\RestPS\endpoints\GET\Invoke-GetProcess.ps1
```

The script accepts a parameter named:

```
RequestArgs
```

---

# Using Query Strings

Query strings are appended using:

```
?key=value
```

Multiple parameters use:

```
&key=value
```

---

## Example – Single Parameter

```powershell
Invoke-RestMethod -Uri 'http://localhost:8080/process?name=Powershell' -Method Get
```

---

## Example – Multiple Parameters

```powershell
Invoke-RestMethod -Uri 'http://localhost:8080/process?name=Powershell&MainWindowTitle=RestPS' -Method Get
```

This returns the PowerShell process running the RestPS service.

---

# Security Considerations

⚠ **Highly Recommended:**

- Remove all default example endpoints after testing.
- Use HTTPS instead of HTTP in production.
- Protect endpoints using authentication and secure secret keys.
- Restrict firewall access to trusted systems only.

---

# Useful References

- https://www.deploymentresearch.com/using-restps-to-access-the-mdt-database/
- https://deploymentbunny.com/2020/11/15/nice-to-know-running-restps-as-a-service/
- https://github.com/NopeNix/RestPS
- https://github.com/PowerShellCrack/RestPSExamples

---

# Potential PSD Integration Scenarios

Below are ideas for using RestPS within a PSD task sequence:

## Service & SSL

- Updated guide for running RestPS as a Windows service with SSL

## PSD Task Sequence Integration

- Trigger REST endpoints during a PSD task sequence

## Intune / Microsoft Graph

- Upload device hash to Autopilot
- Assign device to Intune categories
- Onboard device to Microsoft Defender for Endpoint (MDE)

## Azure / Entra ID (Graph API)

- Add device to Azure groups
- Set device extension attributes

## Offline Domain Join

- Trigger `djoin.exe` via REST endpoint

---

RestPS provides powerful automation capabilities when integrated with PSD, particularly in modern cloud-based deployment scenarios.
