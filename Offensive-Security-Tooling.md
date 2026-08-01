# 📂 Offensive Security Tooling

A technical reference guide detailing common offensive security tooling, execution mechanics, log telemetry generation, and defensive countermeasures for credential attacks, web enumeration, database injection, and persistent shell detection.

---

## 🗺️ Module Overview & Roadmap

| Topic / Room | Focus Area | Core Concepts & Telemetry Indicators |
| :--- | :--- | :--- |
| **Hydra** | Online Credential Attacks | Protocol brute-forcing (SSH, FTP, HTTP, SMB), rate-limiting detection, Event ID 4625 |
| **Gobuster: The Basics** | Web Resource Enumeration | High-speed directory/subdomain/VHost brute-forcing, HTTP access log signature analysis |
| **Shells Overview** | Payload Execution Mechanics | Reverse vs. Bind vs. Web shells, process lineage anomalies (`w3wp.exe` spawning `cmd.exe`) |
| **SQLMap: The Basics** | Automated SQL Injection | Parameterized query bypass, automated payload detection, WAF rules, database log spikes |
| **ToolsRus** | Integrated Auditing & Hardening | Multi-stage web reconnaissance, default Tomcat manager hardening, account lockout |

---

## 📑 Detailed Topics & Technical Reference

### 1. Online Credential Attacks: Hydra
* **Mechanics:** Automated parallel credential testing tool supporting multi-protocol authentication endpoints (SSH, FTP, RDP, HTTP POST/GET).
* **Detection & Telemetry Artifacts:**
  * **Windows Event Log (Security):** Rapid bursts of Event ID **4625** (*An account failed to log on*) originating from a single foreign IP.
  * **Linux Log Audit:** Spikes in `Failed password for...` entries within `/var/log/auth.log` or `/var/log/secure`.
  * **Network Telemetry:** Unusually high TCP connection attempt velocity targeting authentication ports (TCP 22, 21, 3389, 80/443).

---

### 2. Web Resource Enumeration: Gobuster
* **Mechanics:** Go-based brute-forcing tool used to discover hidden web directories, sensitive files (`.env`, `.git`, `backup.zip`), subdomains, and Virtual Hosts (VHosts).
* **Detection & Telemetry Artifacts:**
  * **Web Access Logs (Apache / Nginx / IIS):** Spikes in `404 Not Found` or `403 Forbidden` response codes within a compressed time frame.
  * **User-Agent Monitoring:** Default header strings (`Gobuster/vX.X`) visible in web server logs if not overridden by the actor.
  * **Rate-Limiting Mitigation:** Enforce Web Application Firewall (WAF) rate limits (e.g., ModSecurity, Cloudflare) and deploy Honeypot directories (`/admin_backup/`) to trigger automated IP shunning.

---

### 3. Shell Mechanics & Detection
* **Payload Classifications:**
  * **Reverse Shell:** Target connects back to an attacker's listener (bypasses inbound firewall rules).
  * **Bind Shell:** Target opens a listening port and waits for an incoming attacker connection.
  * **Web Shell:** Script uploaded to a web directory (`.php`, `.aspx`, `.jsp`) executing system commands via HTTP requests.
* **Process Lineage & Behavior Anomaly Detection:**
  * **Suspicious Child Processes:** EDR alerts triggering when web server daemons (`www-data`, `apache2`, `w3wp.exe`) spawn command interpreters (`/bin/sh`, `/bin/bash`, `cmd.exe`, `powershell.exe`).
  * **Egress Filtering:** Block unauthorized outbound traffic on non-standard ports; restrict server outbound connections to trusted destinations only.

---

### 4. Automated SQL Injection: SQLMap
* **Mechanics:** Automated tool that identifies and exploits SQL injection vulnerabilities across HTTP request parameters, leveraging dynamic payload generation (Boolean-based, Time-based blind, Error-based, UNION query).
* **Detection & Telemetry Artifacts:**
  * **Access Logs & WAF Triggers:** Requests containing database functions (`CAST()`, `CONCAT()`, `SLEEP()`, `UNION SELECT`) or SQLMap signatures in `User-Agent`.
  * **Database Log Anomalies:** High volume of malformed SQL query errors recorded in database server error logs (`mysqld.log`, PostgreSQL logs).
  * **Remediation:** Enforce **Parameterized Queries (Prepared Statements)** across all application codebases; avoid dynamic SQL query string concatenation.

---

### 5. Integrated Auditing: ToolsRus
* **Environment Analysis:** Synthetic target environment demonstrating multi-stage reconnaissance, web directory enumeration, and administrative web panel exposure.
* **Audit Methodology & Remediation:**
  * **Exposure:** Unprotected administrative interfaces (e.g., Apache Tomcat `/manager/html`) combined with weak/default administrative credentials.
  * **Hardening Standard:** Enforce strict access control lists (ACLs) restricting management endpoints to internal management subnets; mandate Multi-Factor Authentication (MFA) and dynamic account lockouts on repeated login failures.

---

## 🛡️ Defensive Engineering Standards

* **Credential Hardening:** Deploy `fail2ban` or host firewalls to dynamically block host IPs after exceeding failed login thresholds.
* **Code Quality & Injection Defense:** Enforce static application security testing (SAST) in CI/CD pipelines to catch unsterilized database input before deployment.
* **File Integrity Monitoring (FIM):** Monitor web root directories (`/var/www/html`, `C:\inetpub\wwwroot`) for unauthorized script additions or modifications.
