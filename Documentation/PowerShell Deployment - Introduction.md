# Introduction to PowerShell Deployment (PSD)

---

## PSD Background

PowerShell Deployment (PSD) is an extension for Microsoft Deployment Toolkit (MDT) that adds support for deployment via HTTP/HTTPS, enabling modern cloud imaging and peer-to-peer (P2P) scenarios.

PSD modernizes the traditional MDT deployment framework by replacing legacy scripting components with PowerShell-based functionality while maintaining compatibility with the MDT Workbench structure.

---

## PSD Workflow Changes

### Standard MDT Behavior

In a standard MDT deployment share:

- VBScript is used on the client side to launch and control the task sequence.
- Task sequence actions are executed using legacy scripting components.
- UNC paths (SMB) are used for network-based deployments.

---

### PSD-Extended Behavior

When MDT is extended with PSD:

- VBScript components are replaced with PowerShell scripts.
- Task sequences are launched and executed entirely through PowerShell.
- The connection to the deployment share is established using a **PowerShell PSDrive**.
- Task sequences are simplified and modernized compared to legacy MDT task sequences.

---

## Driver Handling Changes

Driver management has also been redesigned.

To support efficient HTTPS-based deployments:

- Drivers for each hardware model are packaged into **WIM or ZIP archives**.
- Driver packages can be automatically generated from drivers already imported into the Deployment Workbench.
- Alternatively, prebuilt driver archives can be copied from external sources.

This approach significantly improves download efficiency and scalability in HTTP/HTTPS and cloud-based deployment scenarios.

---

> 💡 **Fun Fact:**  
> Standard MDT task sequences originated from the OSD Feature Pack for SMS 2003.  
> We felt it was time to modernize them.

---

# The Team

PSD is developed and maintained by:

- Mikael Nystrom (@mikael_nystrom)  
- Johan Arwidmark (@jarwidmark)  
- Michael Niehaus (@mniehaus)  
- Steve Campbell (@SoupAtWork)  
- Jordan Benzing (@JordanTheItGuy)  
- Andreas Hammarskjold (@AndHammarskjold)  
- Richard "Dick" Tracy (@PowerShellCrack)  
- George Simos (@GSimos)  
- Elias Markelis (@emarkelis)  

---

PSD continues to evolve with contributions from the broader deployment and modern workplace community.
