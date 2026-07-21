# 🛡️ My Cybersecurity Journey (Blue Team Edition)

Welcome to my central repository for tracking my progress, hands-on labs, and technical notes as I train to become a Defensive Security Analyst.

---

## 👨‍💻 About Me
* 🎯 **Current Focus:** Developing strong fundamentals in infrastructure security, threat analysis, and digital forensics.
* 🚀 **Objective:** Building a solid foundation on TryHackMe to transition into active Security Operations Center (SOC) engineering and Incident Response.
* 🛠️ **Current Lab Environment:** Windows 11 / Linux (Blackwell Architecture Powered Engine)

---

## 📊 TryHackMe Progress Tracker

| Learning Path | Status | Key Defensive Skills Learned |
| :--- | :---: | :--- |
| **Pre-Security** | ✅ 100% | Network traffic flow, Linux system basics, Web architecture |
| **Cyber Security 101** | 🔄 In Progress | Threat landscapes, OSINT verification, Vulnerability identification |

---

## 📚 Technical Reference Notes

Here is a breakdown of the specific rooms and concepts I have documented so far. Click the links below to view my synthesized notes:

### 🔍 Threat Intelligence & OSINT
* [Search Skills](./Search-Skills.md) – Investigating indicators of compromise (IoCs) using Shodan, VirusTotal, and cross-referencing CVE/NVD databases for patch management.
### 🐧 Linux System Administration & Defense
* [Linux Fundamentals](./Linux-Fundamentals.md) – A defensive cheat sheet covering directory navigation, log filtering with grep, and file permission structures.
### 🪟 Windows System Administration & Defense
* [Windows Fundamentals](./Windows-Fundamentals.md) – An endpoint security reference sheet covering core administrative utilities, Registry persistence, and critical Event Viewer IDs for incident triage.
### 🔍 Core Operating System Environments
* [Module 04: Shells & Command Line](./CommandLines.md) – A consolidated reference guide for Windows CMD, Linux shell redirection, and advanced PowerShell object filtering (`Get-Service`) used during live incident triage.
### 🖥️ Windows Security & Enterprise Incident Response
* [Active Directory Basics](./Active-Directory-Basics.md) – A technical reference guide covering AD architecture, domain controllers, Kerberos authentication principles, and defensive enumeration.
* [Investigating Windows (Forensics Case Study)](./Investigating-Windows.md) – A deep-dive Incident Response write-up tracking a live enterprise server compromise. Documents practical tracking of registry persistence, malicious scheduled tasks (`nc.ps1`), credential dumping artifacts (Mimikatz), and firewall audit logging.
### 🖥️ Windows Security & Enterprise Incident Response (THM Skill Boost)
* [Hacking with PowerShell (Execution Analysis)](./Hacking-with-Powershell.md) – An auditing reference guide analyzing file transfer cmdlets (`Invoke-WebRequest`), in-memory execution via `Invoke-Expression` (IEX), and the high-priority Event IDs (4104/4103) required to hunt for malicious PowerShell scripts.
### 🌐 Network Fundamentals & Packet Analysis
* [Networking & Traffic Analysis](./Networking.md) – A technical guide covering TCP/IP vs OSI layers, secure protocol transitions (HTTP/HTTPS, Telnet/SSH), traffic analysis with Wireshark and Tcpdump, and network discovery using Nmap.

---

## ⚙️ Core Toolset & Technologies
Below are the defensive tools and platforms I am getting hands-on experience with during my training:

* **Threat Intel & Scouting:** VirusTotal (Malware/Hash analysis), Shodan, NVD/CVE Databases
* **Network Defense & Analysis:** Wireshark (Traffic analysis), Nmap (Asset discovery)
* **Environments:** Learn on Demand Labs, TryHackMe Blue Team Labs, Local Virtual Machines
