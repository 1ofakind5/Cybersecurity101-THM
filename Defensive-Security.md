# 📂 Defensive Security

A technical reference guide covering the foundational pillars of defensive security operations — SOC workflows, digital forensics, incident response, and log analysis — as introduced in TryHackMe's Defensive Security module.

---

## 🗺️ Module Overview & Roadmap

| Topic / Room | Focus Area | Core Concepts & Telemetry Indicators |
| :--- | :--- | :--- |
| **Defensive Security Intro** | Blue Team Foundations | Defensive vs. offensive mindset, threat intelligence lifecycle, CIA triad application |
| **SOC Fundamentals** | Security Operations Center Structure | SOC tiering (L1/L2/L3), alert triage workflow, escalation paths, SIEM-centric monitoring |
| **Digital Forensics Fundamentals** | Evidence Acquisition & Analysis | Chain of custody, volatile vs. non-volatile data, disk/memory imaging, forensic artifact analysis |
| **Incident Response Fundamentals** | Structured IR Lifecycle | NIST/SANS IR phases (Prep, Detection, Containment, Eradication, Recovery, Lessons Learned) |
| **Logs Fundamentals** | Log Source Analysis | Log types (system, application, security), centralized logging, log correlation |
| **Mystery Chest** | Applied Practical Challenge | Hands-on synthesis of the above concepts in a scenario-driven exercise |

---

## 📑 Detailed Topics & Technical Reference

### 1. Defensive Security Intro
* **Mechanics:** Introduces the blue team's core objective — protecting, detecting, and responding — as the counterpart to offensive (red team) operations.
* **Key Concepts & Artifacts:**
  * **CIA Triad:** Confidentiality, Integrity, and Availability as the foundation for prioritizing defensive controls.
  * **Threat Intelligence Lifecycle:** Direction → Collection → Processing → Analysis → Dissemination → Feedback, feeding proactive defense.
  * **Defensive Disciplines Overview:** Umbrella view of SOC operations, threat intel, digital forensics, malware analysis, and incident response as interlocking functions.

---

### 2. SOC Structure & Operations: SOC Fundamentals
* **Mechanics:** Defines the Security Operations Center as the centralized function responsible for continuous monitoring, detection, and initial response to security events.
* **Detection & Telemetry Artifacts:**
  * **Analyst Tiering:** L1 (triage/alert validation) → L2 (deep investigation) → L3 (threat hunting/advanced response).
  * **SIEM Correlation:** Aggregated log ingestion from endpoints, network devices, and applications feeding correlation rules that generate actionable alerts.
  * **Escalation Workflow:** Documented handoff criteria between tiers to prevent alert fatigue and ensure critical events reach the right analyst.

---

### 3. Evidence Handling: Digital Forensics Fundamentals
* **Mechanics:** Structured process for identifying, preserving, and analyzing digital evidence in a manner that remains legally and technically sound.
* **Detection & Telemetry Artifacts:**
  * **Chain of Custody:** Documented record of evidence handling (who, what, when, where) to preserve admissibility and integrity.
  * **Volatile vs. Non-Volatile Data:** RAM, running processes, and network connections (volatile) captured before disk images and log files (non-volatile) to avoid evidence loss.
  * **Imaging Standards:** Bit-for-bit forensic disk/memory imaging (e.g., `dd`, FTK Imager) with hash verification (MD5/SHA256) to prove integrity.

---

### 4. Structured Response: Incident Response Fundamentals
* **Mechanics:** Applies a repeatable, phased methodology (aligned with NIST SP 800-61 / SANS PICERL) to contain and recover from security incidents.
* **Detection & Telemetry Artifacts:**
  * **Preparation:** IR playbooks, contact trees, and tooling readiness established before an incident occurs.
  * **Detection & Analysis:** Correlating alerts and indicators of compromise (IOCs) to confirm and scope an incident.
  * **Containment, Eradication & Recovery:** Isolating affected hosts, removing the root cause, and restoring systems to a known-good state.
  * **Lessons Learned:** Post-incident review documenting root cause, response gaps, and control improvements.

---

### 5. Telemetry Sources: Logs Fundamentals
* **Mechanics:** Examines the categories and value of log data generated across an environment as the primary evidence source for detection and investigation.
* **Detection & Telemetry Artifacts:**
  * **Log Categories:** System logs (OS-level events), application logs (service-specific activity), and security logs (authentication, access control).
  * **Centralization:** Forwarding logs to a SIEM or log management platform to enable cross-source correlation and long-term retention.
  * **Correlation Value:** Combining disparate log sources (e.g., firewall + authentication logs) to reconstruct an attack timeline that a single source can't reveal alone.

---

## 🛡️ Defensive Engineering Standards

* **Process Discipline:** Formalize IR playbooks and escalation paths so response isn't improvised mid-incident.
* **Evidence Integrity:** Always hash and document evidence at acquisition time to preserve chain of custody.
* **Centralized Visibility:** Consolidate logs into a SIEM to enable correlation that individual log sources can't provide on their own.
