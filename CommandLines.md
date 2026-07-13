# 💻 Windows Command Line, PowerShell, & Linux Shells

A comprehensive technical reference guide covering command execution environments across both Windows and Linux operating systems, with a focus on system auditing, pipeline redirection, and defensive triage automation.

---

## 1. Windows Command Prompt (CMD) vs. PowerShell

While traditional Windows Command Prompt (`cmd.exe`) processes and outputs data strictly as plain text strings, **PowerShell** processes data as structured **Objects**. This allows a defensive analyst to programmatically filter, sort, and isolate system attributes without relying on complex text-parsing utilities.

### 🔍 Practical Case Study: Incident Triage Tracking
During a live system auditing exercise in PowerShell, the objective was to isolate a specific persistent background service amidst hundreds of running system tasks using automated string filtering.

* **Objective:** Identify the system name and display name of a hidden service containing a specific keyword variation.
* **Threat Hunting Command Utilized:**
  ```powershell
  Get-Service | Where-Object { $_.DisplayName -like "*merry*" } | Select-Object Name, DisplayName
