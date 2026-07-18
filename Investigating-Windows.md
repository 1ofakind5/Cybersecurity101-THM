# 🕵️‍♂️ Investigating Windows & Digital Forensics

A hands-on Incident Response (IR) case study detailing the forensic analysis of a compromised Windows Server 2016 machine. This guide outlines the timeline, tools, and defensive methodologies used to uncover an attacker's footprints, tools, and persistence mechanisms.

---

## 1. Case Overview & Initial Triage

An enterprise Windows Server 2016 environment was flagged for potential compromise. As the defensive analyst, the goal was to perform a forensic deep-dive into the live system state to identify the scope of the breach, entry methods, and active persistence.

### Subsystem & OS Identification
Initial discovery commands were executed via PowerShell to isolate system variables:
* **Command:** `Get-ComputerInfo -Property "Os*"`
* **Discovery:** The endpoint was identified running **Windows Server 2016 Standard**.

---

## 2. User Account & Authentication Auditing

Attackers often target user profiles to move laterally or create persistent backdoors. 

### Audit Commands & Discovery
* **Enumerating Local Users:** `Get-LocalUser` was used to map out the system accounts.
* **Tracking Login History:** `net user <username> | findstr "Last"` was run against active profiles.
  * **Administrator:** Identified as the last active user session on the machine.
  * **John:** Forensic logs indicate an older interactive session matching a suspicious timestamp (**03/02/2019 at 5:48:32 PM**).
  * **Jenny:** Profile status showed `Never` logged in, highlighting it as an inactive target.
* **Privileged Group Audit:** `Get-LocalGroupMember -Group "Administrators"` was run to inspect the local admin group.
  * **Discovery:** Aside from the root administrator account, both `Jenny` and `Guest` were granted active local administrative privileges, representing a significant privilege configuration flaw or backdoor implementation.

---

## 3. Threat Hunting: Malware Persistence & Artifacts

Malware modification of the system registry and scheduler ensures code execution survives a system reboot.

### Registry Run Key Analysis
The Windows Registry was audited for automatic startup values under:
`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
* **Artifact Uncovered:** A highly suspicious registry entry named `UpdateSvc` was pointing directly to an outbound connection script hardcoded to the internal IP address **`10.34.2.3`**.

### Malicious Scheduled Tasks
The task scheduler path was swept to identify tasks hiding outside standard system directories.
* **Command:** `Get-ScheduledTask | where {$_.TaskPath -eq "\"}`
* **Uncovered Task:** A task named **`Clean file system`** was found masquerading as routine maintenance.
* **Execution Logic:** Analyzing the properties of the task (`$task.Actions`) revealed it was configured to execute a malicious PowerShell script daily: **`nc.ps1`** (Netcat script wrapper).
* **Network Impact:** Checking the script execution arguments revealed that the file was listening locally on port **`1348`**, establishing an unauthenticated bind shell backdoor.

---

## 4. Timeline Analysis & Event Log Forensics

Establishing the exact timeline of events is crucial for identifying how deep the attacker penetrated the network.

### Forensic Timeline Analysis
* **Compromise Date:** File system analysis of newly generated directories on `C:\` established the core breach occurred on **03/02/2019**.
* **Privilege Escalation Signature:** Event Viewer security log auditing was restricted using a custom time window around the compromise event. Under the Security Log, a critical entry marked the point where Windows first assigned elevated privileges to a new logon: **03/02/2019 at 4:04:47 PM**.

---

## 5. Attacker Tooling & Infrastructure Mapping

A deep dive into system temp directories and system configurations revealed the threat actor's toolset and command-and-control (C2) servers.

### Credential Dumping Tools
* **Location:** `C:\TMP\`
* **Artifact:** A hidden binary `mim.exe` and a matching text output dump `mim-out` were recovered.
* **Analysis:** Parsing the output file confirmed the threat actor successfully executed **Mimikatz** to dump Windows plain-text passwords from memory.

### Infrastructure & Network Backdoors
* **C2 Server Mapping & DNS Poisoning:** Inspection of the local `C:\Windows\System32\drivers\etc\hosts` file revealed active **DNS Poisoning**. The attacker bound `google.com` directly to their external Command & Control server IP: **`76.32.97.132`**.
* **Webshell Uploads:** Inspection of the local IIS web root directory (`C:\inetpub\wwwroot\`) revealed the presence of unauthorized **`.jsp`** (Java Server Pages) files, proving the initial entry vector was an uploaded malicious web shell.
* **Firewall Exceptions:** Reviewing inbound firewall rules filtering for unassociated items ("Rules without a Group") exposed an active rule named `Allow outside connections for development`. Inspecting the rule properties showed the attacker opened port **`1337`** to ensure external traffic could bypass the host firewall.

---

## 6. Incident Response Summary Table

| Artifact Category | Value / Indicator | Defensive Finding |
| :--- | :--- | :--- |
| **Operating System** | Windows Server 2016 | Target OS environment. |
| **Backdoor Admin Accounts** | Jenny, Guest | Privilege escalation persistence vector. |
| **Malicious Registry Key** | `UpdateSvc` -> `10.34.2.3` | Registry Run key persistence. |
| **Malicious Scheduled Task**| `Clean file system` | Daily execution trigger. |
| **Backdoor Script & Port** | `nc.ps1` listening on port `1348` | Netcat listening socket. |
| **Credential Dumping Tool** | Mimikatz (`mim.exe`) | Credential theft validation. |
| **C2 Infrastructure** | `76.32.97.132` | Attacker external control IP. |
| **Initial Access Vector** | `.jsp` Web Shell | File upload vulnerability exploitation. |
| **Open Firewall Socket** | Port `1337` | Unauthorized persistent network access. |
