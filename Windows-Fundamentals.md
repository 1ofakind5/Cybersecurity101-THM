# 🪟 Windows System Administration & Defense Fundamentals

A technical reference guide for navigating the Windows operating system architecture, auditing system events, and utilizing core administrative utilities from a defensive security perspective.

---

## 1. System Auditing & Monitoring Tools
When an endpoint is suspected of being compromised, these built-in administrative tools are a defender's first line of sight.

* **Task Manager (`taskmgr`)**
  * *Defensive Use:* Monitoring running processes for unauthorized or resource-heavy applications, checking active network connections per process, and identifying persistent startup programs.
* **Services (`services.msc`)**
  * *Defensive Use:* Auditing background processes that run automatically. Attackers often install malicious software as a persistent Windows Service to survive system reboots.
* **System Information (`msinfo32`)**
  * *Defensive Use:* Gathering a rapid, comprehensive snapshot of hardware, system drivers, and environment variables to detect unauthorized system alterations.

---

## 2. Event Viewer & Log Analysis (`eventvwr.msc`)
The Event Viewer is a goldmine for Blue Teamers. It records system, security, and application logs that track exactly what has occurred on a machine.

### Critical Log Categories to Monitor:
* **Security Logs:** Tracks authentication attempts. Analysts review this to spot brute-force attacks or unauthorized privilege escalations.
* **System Logs:** Logs events triggered by Windows system components, such as drivers failing to load or unexpected system shutdowns.
* **Application Logs:** Records events logged by specific software programs, useful for detecting application crashes or exploits.

### Key Event IDs Every Defender Should Know:
* `4624` - Successful Account Logon (Tracks who logged in and how).
* `4625` - Failed Account Logon (Critical for identifying password-spraying or brute-force attempts).
* `4720` - A User Account Was Created (Essential for spotting unauthorized backdoor accounts).

---

## 3. Command Line & PowerShell Utilities
Defenders use the command line for fast data collection and live incident response triage.

* `whoami /priv` - Displays the security privileges assigned to the current user session (helps determine if an attacker has achieved administrative rights).
* `netstat -ano` - Displays all active TCP/UDP network connections, open ports, and the specific Process ID (PID) responsible for the connection.
* `net user` - Lists all user accounts present on the local machine to audit for unauthorized users.
* `systeminfo` - Pulls system properties, OS version, and critically, a list of installed **Hotfixes/Patches** to verify if the system is vulnerable to known exploits.

---

## 4. File Systems & Resource Management
Understanding how Windows restricts data access is fundamental to endpoint hardening.

* **NTFS Permissions (New Technology File System):** Windows uses Access Control Lists (ACLs) to determine which users or groups have read, write, modify, or full control permissions over files and folders.
* **Registry Editor (`regedit`):** A massive database containing settings, configurations, and options for the OS. Blue Teamers audit the Registry to find "Run" keys (`HKLM\Software\Microsoft\Windows\CurrentVersion\Run`) where malware hides to launch itself automatically upon startup.
