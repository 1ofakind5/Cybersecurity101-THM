# 🐧 Linux Security Monitoring

A learning module covering how Security Operations Center (SOC) analysts monitor Linux systems, utilise system logs for triage, and detect common Linux attacks through real-world examples and hands-on threat detection labs.

## 🎯 Learning Objectives

By completing this module, I developed an understanding of how SOC teams:

- Identify key Linux log sources and understand how to use them effectively during SOC triage.
- Explain how attackers gain initial access to Linux systems and detect these break-in attempts in the logs.
- Recognise the first actions attackers take after breaching a Linux server (such as persistence or privilege escalation) and learn how to detect them.
- Cover the last stages of an attack on Linux systems, including lateral movement and data exfiltration, and understand how they appear in system logs.

## 🗺️ Module Roadmap

| Topic | Main Focus | Practical SOC Value |
| :--- | :--- | :--- |
| **Linux Logging for SOC** | Key log sources and their use in triage | Know where to look and which log answers which question |
| **Linux Threat Detection 1** | Initial access and exploitation | Detect break-in attempts and successful compromises in logs |
| **Linux Threat Detection 2** | Post-exploitation and persistence | Identify attacker actions immediately following a breach |
| **Linux Threat Detection 3** | Final stages and exfiltration | Spot lateral movement and data theft in system logs |

---

## 📑 Detailed Topics

### 1. Linux Logging for SOC

**Linux Logging for SOC** establishes the foundation of Linux visibility: where the operating system stores logs, what each log source contains, and how a SOC analyst navigates them during an investigation. It focuses on the transition from raw log files to actionable triage data.

#### Key Log Sources

| Log Source | What It Provides | Analyst Note |
| :--- | :--- | :--- |
| **Authentication Logs** (`/var/log/auth.log`, `/var/log/secure`) | Successful and failed logins, `sudo` usage, SSH sessions, and PAM events. | The first place to check for brute-force or unauthorized access. |
| **System Logs** (`/var/log/syslog`, `/var/log/messages`) | General system activity, service starts/stops, and kernel messages. | Useful for establishing a baseline of normal system behavior. |
| **Audit Logs** (`auditd`, `/var/log/audit/audit.log`) | Detailed security-relevant events, including command execution and file access. | Crucial for deep forensics, though often noisy; requires specific rules. |
| **Web and App Logs** | Application-specific logs (e.g., Nginx, Apache) that may indicate web exploitation. | Correlate with system logs to see the impact of a web attack. |
| **Journald** (`journalctl`) | Centralized logging system that captures `stdout`/`stderr` from services. | Useful for viewing logs in a structured, queryable format. |

#### Defensive Application

- Filtering and parsing logs with tools like `grep`, `awk`, and `journalctl` to isolate malicious activity from background noise.
- Understanding log rotation and retention policies ensures critical evidence isn't lost.
- Establishing a baseline of legitimate `sudo` usage helps spot privilege escalation attempts.

---

### 2. Linux Threat Detection 1

**Linux Threat Detection 1** focuses on the initial stages of an attack: how attackers break into Linux systems and how to spot those break-ins. It covers common entry vectors such as SSH brute-forcing, exploiting vulnerable services, and web application vulnerabilities.

#### Attack Vectors and Log Signatures

| Attack Method | Log Indicator | Details |
| :--- | :--- | :--- |
| **SSH Brute Force** | Multiple `Failed password` entries in `auth.log` followed by an `Accepted password` from the same IP. | Look for rapid, repeated failed attempts from a single source. |
| **Credential Stuffing** | Failed logins across different usernames from the same IP. | Often followed by a successful login if a valid credential is found. |
| **Web Exploitation** | `GET` or `POST` requests with malicious payloads (e.g., shell commands) in web server logs. | Correlate with system logs to see if a shell was spawned. |
| **Service Exploitation** | Unexpected service crashes or errors followed by unusual child processes. | Requires knowledge of vulnerable services (e.g., outdated FTP or SSH versions). |

#### Defensive Application

- Correlating source IPs from `auth.log` with threat intelligence feeds to identify known bad actors.
- Monitoring for successful logins from unusual geographic locations or at odd hours.
- Detecting `sudo` abuse by looking for unusual commands executed by compromised accounts.

---

### 3. Linux Threat Detection 2

**Linux Threat Detection 2** explores the first actions attackers take after breaching a Linux server. This phase is critical for detecting persistence mechanisms and privilege escalation, as attackers attempt to secure their access and elevate their permissions.

#### Post-Exploitation Detection

| Attacker Action | Log/Telemetry Indicator | Why It Matters |
| :--- | :--- | :--- |
| **Persistence (Cron)** | New cron jobs or modified `/etc/cron*` files that execute reverse shells or malicious scripts. | Allows the attacker to regain access even after a reboot. |
| **Persistence (SSH Keys)** | Unauthorized public keys added to `/root/.ssh/authorized_keys` or user `.ssh` directories. | A stealthy backdoor that bypasses password authentication. |
| **Privilege Escalation** | Unexpected `sudo` commands, unusual setuid binaries, or kernel exploits in `syslog`. | Attackers use this to gain `root` access from a lower-privileged account. |
| **User Creation** | New user accounts created via `useradd` or `adduser` with suspicious names. | Often used as a secondary backdoor or for lateral movement. |

#### Defensive Application

- Auditing file integrity on `/etc/cron*` and `.ssh` directories using tools like `AIDE` or `Tripwire`.
- Monitoring `auth.log` for `useradd` or `usermod` events outside of change management windows.
- Using `auditd` to track the execution of binaries that modify system configurations.

---

### 4. Linux Threat Detection 3

**Linux Threat Detection 3** covers the final stages of an attack: lateral movement, data exfiltration, and cleanup. It teaches how these activities manifest in system logs and how to identify them before the attacker completes their objectives.

#### Advanced Threat Detection

| Attack Stage | Log Indicator | Analyst Focus |
| :--- | :--- | :--- |
| **Lateral Movement** | SSH connections to internal hosts, `scp` or `rsync` transfers to unknown IPs. | Look for spikes in outbound traffic or unusual internal connections. |
| **Data Exfiltration** | Large outbound transfers, `curl` or `wget` to external domains, or archive creation (`tar`, `zip`). | Correlate with network traffic logs to confirm data volume. |
| **Log Tampering** | Deletion of logs (`rm /var/log/*`), clearing of `bash_history`, or stopping the `syslog` service. | A classic sign of an attacker covering their tracks. |
| **Anti-Forensics** | Changes to file timestamps (`touch -t`), use of `history -c`, or disabling `auditd`. | Indicates a sophisticated attacker who is aware of forensic techniques. |

#### Defensive Application

- Setting alerts for large outbound data transfers from web servers or database servers.
- Centralizing logs to a remote server ensures they remain intact even if the local system is compromised and logs are cleared.
- Monitoring for the stopping or disabling of critical logging services (`systemctl stop rsyslog`).
