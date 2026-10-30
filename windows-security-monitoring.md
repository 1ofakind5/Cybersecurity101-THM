# 🖥️ Windows Security Monitoring & Threat Detection

A technical reference guide detailing Windows operating system telemetry, Event Log architecture for Security Operations Centers (SOC), and behavioral detection methodologies spanning initial access, execution, and persistence phases.

---

## 🗺️ Module Overview & Roadmap

| Topic / Room | Focus Area | Core Concepts & Key Event IDs |
| :--- | :--- | :--- |
| **Windows Logging for SOC** | OS Telemetry Architecture | Event channels (Security, System, Application), XML logs, Sysmon integration, auditing policies |
| **Windows Threat Detection 1** | Initial Access Analysis | Phishing payloads, malicious attachments, perimeter defense logs, foothold telemetry |
| **Windows Threat Detection 2** | Post-Breach Execution | Command-line auditing, process creation lineage, PowerShell script block logging |
| **Windows Threat Detection 3** | Persistence Mechanisms | Scheduled task creation, service registration, registry run keys, startup folder monitoring |

---

## 📑 Detailed Topics & Technical Reference

### 1. Windows Logging Architecture for SOC
* **Log Channels & Subsystems:**
  * **Security Log:** Tracks authentication events, privilege use, and object access (`wevtutil qe Security`). Requires strict auditing policy configuration via Group Policy Object (GPO).
  * **System / Application Logs:** Operating system core events, driver loading states, and application faults.
  * **PowerShell Operational Log (`Microsoft-Windows-PowerShell/Operational`):** Captures advanced script execution telemetry.
* **Essential Event IDs for Detection Engineering:**
  * **4624 / 4625:** Successful / Failed logon attempts (distinguishes logon types: Type 3 = Network, Type 10 = RDP, Type 2 = Interactive).
  * **4688:** A new process has been created (enables command-line argument logging when enabled via GPO).
  * **4104:** PowerShell Script Block Logging (captures full code blocks executed in memory, bypassing obfuscation).
  * **7045:** A new service was installed on the system (critical for persistence tracking).
  * **4698:** A scheduled task was created.

---

### 2. Windows Threat Detection 1: Initial Access
* **Attack Vector Analysis:** Focuses on how threat actors establish their initial foothold on Windows endpoints, typically via weaponized documents, malicious links, or exposed remote services.
* **Telemetry Indicators:**
  * **Email & Browser Gateways:** Tracking incoming file attachments with double extensions (`.pdf.exe`, `.docx.js`) or malicious macro-enabled documents (`.xlsm`).
  * **Network Artifacts:** Unexpected outbound connections initiated by local productivity apps (e.g., `winword.exe` spawning cmd.exe or PowerShell).

---

### 3. Windows Threat Detection 2: Post-Breach Execution
* **Behavioral Monitoring:** Analyzes attacker activities immediately following initial compromise, focusing on reconnaissance commands and script execution.
* **Process Lineage Anomalies:**
  * Legitimate administrative tools executed from abnormal paths or spawning unauthorized child processes (e.g., `explorer.exe` $\rightarrow$ `cmd.exe` $\rightarrow$ `whoami.exe`).
* **PowerShell Logging Mechanics:**
  * In-memory script blocks captured via Event ID **4104** reveal obfuscated download cradles (`Invoke-WebRequest`, `Net.WebClient`), base64-encoded command lines, and memory injection routines.

---

### 4. Windows Threat Detection 3: Persistence Mechanisms
* **Threat Actor Persistence Strategies:**
  * **Windows Services:** Registering malicious binaries as automatic startup services to execute under `SYSTEM` privileges before user login. Detected via Event ID **7045** and registry inspection (`HKLM\SYSTEM\CurrentControlSet\Services`).
  * **Scheduled Tasks:** Utilizing `schtasks.exe` or Task Scheduler to maintain long-term access. Monitored via Event ID **4698** and XML task configurations stored in `C:\Windows\System32\Tasks\`.
  * **Registry Run Keys:** Persistence entries in `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run` and user-specific hives.

---

## 🛡️ Defensive Engineering Standards

* **Advanced Auditing Policies:** Enable "Audit Process Creation" with command-line inclusion (`Include command line in process creation events`) via GPO to ensure Event ID 4688 captures arguments.
* **Sysmon Deployment:** Implement **System Monitor (Sysmon)** with a hardened configuration (e.g., SwiftOnSecurity or Olaf Hartong baseline) to capture network connections, process tampering, and driver loads in Event Viewer log channel `Microsoft-Windows-Sysmon/Operational`.
* **Centralized SIEM Forwarding:** Forward structured Windows Event Logs and Sysmon telemetry to a centralized SIEM (Splunk, Elastic) for real-time correlation and alerting.
