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
  
---

## 2. Linux Shells & Scripting Automation

In Linux, the **Shell** acts as the direct command-line interpreter facilitating interaction with the kernel. Beyond running singular commands, shells support full automation scripts (`.sh` files).

### Core Scripting Components
A shell script is a combined sequence of commands executed together to automate repetitive administrative tasks. 

* **The Shebang (`#!/bin/bash`)**: Added to the first line of a script file to define which shell interpreter should execute the code.
* **Execution Permissions (`chmod +x`)**: Before a custom script can be run, it must be granted executable rights on the filesystem.
* **Control Structures (Loops)**: Configures iterative logic to parse text or verify system variables repeatedly.

### Practical Script Template (`first_script.sh`)
An example of constructing a basic interactive Bash automation script to gather user inputs and log directory details:

```bash
#!/bin/bash
# A simple script to audit the current folder contents

echo "Who is running this audit?"
read username
echo "Audit initiated by: $username"

# List directory contents and print system uptime
ls -la
uptime
