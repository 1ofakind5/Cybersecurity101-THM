# 📂 Introduction to Phishing Simulation

A technical reference guide documenting a completed phishing-detection simulation, including alert triage, true-positive classification, incident response timing, security telemetry, and defensive countermeasures for malicious links delivered through email or accessed from external resources.

> **Simulation outcome:** Victory — the simulated security breach was prevented. All true-positive alerts were identified, with a recorded score of **205 points** and a **60% true-positive rate**. The results also indicated that mean time to resolve (MTTR) and dwell time were longer than in previous runs, with the Firewall alert requiring the most time to resolve.

---

## 🗺️ Module Overview & Roadmap

| Topic / Stage | Focus Area | Core Concepts & Telemetry Indicators |
| :--- | :--- | :--- |
| **Phishing Simulation Fundamentals** | Email- and link-based attack lifecycle | Social engineering, malicious URLs, delivery and user interaction |
| **Alert Triage** | Initial SOC investigation | Alert severity, event correlation, sender and URL analysis |
| **True-Positive Classification** | Detection accuracy | Distinguishing malicious activity from benign or suspicious activity |
| **Containment & Resolution** | Response execution | Blocking URLs, isolating affected users, removing messages, escalation |
| **Metrics & Improvement** | Performance measurement | MTTR, dwell time, true-positive rate, documentation quality |
| **Defensive Hardening** | Long-term risk reduction | Email security, awareness training, browser controls, and monitoring |

---

## 📑 Detailed Topics & Technical Reference

### 1. Phishing Simulation Fundamentals

* **Purpose:** A phishing simulation safely reproduces realistic phishing activity so defenders can practice identifying, classifying, and responding to suspicious messages or links without exposing production systems to an actual compromise.
* **Typical Attack Flow:**
  1. An attacker or simulation platform delivers a convincing email or message.
  2. The message uses a malicious, redirected, or suspicious external URL.
  3. A recipient may click the link, submit credentials, or download a payload.
  4. Email, endpoint, firewall, DNS, proxy, and identity telemetry records the activity.
  5. The SOC investigates the alert, confirms its classification, and performs containment.
* **Primary Risk:** Phishing can lead to credential theft, malware execution, business-email compromise, unauthorized access, or lateral movement.

---

### 2. Alert Triage & Investigation

* **Initial Questions:**
  * Who sent the message, and is the sender identity trustworthy?
  * Does the sender domain resemble a legitimate domain through typosquatting or impersonation?
  * Where does the URL redirect, and is the destination consistent with the claimed sender?
  * Did a user click the link, authenticate, download a file, or enter credentials?
  * Are there related DNS, proxy, firewall, endpoint, or identity events?
* **Useful Telemetry:**
  * **Email Security Logs:** Sender, recipient, message ID, attachment metadata, URL reputation, delivery status, and quarantine actions.
  * **DNS and Proxy Logs:** Domain lookups, URL requests, redirects, newly registered domains, and blocked destinations.
  * **Firewall Logs:** Outbound connections to suspicious external hosts, denied requests, destination IPs, ports, and connection timestamps.
  * **Identity Logs:** Unusual sign-ins, impossible travel, MFA prompts, password changes, and authentication failures after a suspected click.
  * **Endpoint Telemetry:** Browser launches, downloaded files, Office child processes, script interpreters, and credential-access behavior.

---

### 3. True-Positive Classification

* **True Positive:** The alert represents genuine malicious or unauthorized activity, such as access to a known phishing destination or a suspicious external link associated with an attack.
* **Benign Positive:** The activity is unusual but authorized or harmless, such as a legitimate security test, approved vendor link, or expected business workflow.
* **False Positive:** The detection fires even though no suspicious activity occurred.
* **False Negative:** Malicious activity occurs but is not detected by the security controls.
* **Classification Standard:** Do not classify an alert from the URL or message alone. Correlate the alert with sender reputation, destination intelligence, user activity, timestamps, endpoint evidence, and identity events.

---

### 4. Alerts Identified in the Completed Simulation

The completed exercise recorded the following true-positive alerts:

| Alert ID | Alert Rule | Severity | Type | Time to Resolve | Classification |
| :--- | :--- | :---: | :--- | :---: | :---: |
| **8816** | Access to Blacklisted External URL Blocked by Firewall | High | Firewall | 6.15 minutes | **Correct** |
| **8817** | Inbound Email Containing Suspicious External Link | Medium | Phishing | 4.57 minutes | **Correct** |

#### Alert 8816 — Blacklisted External URL Blocked by Firewall

* **Meaning:** A connection attempt to an external URL listed as malicious or prohibited was blocked by a firewall control.
* **Investigation Priorities:** Identify the originating host and user, confirm the destination reputation, review the initiating process or browser session, and check for related connections before and after the block.
* **Response Actions:** Preserve the event, confirm the block remained effective, search for other affected hosts, and investigate whether the user interacted with the destination before enforcement occurred.
* **Why It Matters:** The firewall prevented the simulated connection, but the event may still indicate that a user clicked a phishing link or that an endpoint was already attempting suspicious communication.

#### Alert 8817 — Inbound Email Containing Suspicious External Link

* **Meaning:** An inbound email contained an external link identified as suspicious by phishing or email-security controls.
* **Investigation Priorities:** Review the sender and recipient, inspect the URL and redirect chain, identify similar messages, and determine whether the email was delivered, clicked, or quarantined.
* **Response Actions:** Quarantine or remove matching messages, block confirmed malicious domains, search mailboxes for related campaigns, and notify affected users when appropriate.
* **Why It Matters:** Email is often the initial access vector. Early identification can prevent credential theft or malware delivery before a user reaches the destination.

---

### 5. Containment, Resolution & Evidence Handling

* **Containment Measures:**
  * Block malicious domains, URLs, and IP addresses at the email gateway, DNS layer, proxy, and firewall where appropriate.
  * Quarantine the original message and remove matching copies from other mailboxes.
  * Revoke suspicious sessions and reset credentials if a user submitted authentication information.
  * Isolate an endpoint when there is evidence of malware execution or post-click compromise.
  * Search for related recipients, URLs, hashes, domains, and authentication events.
* **Evidence to Record:** Alert ID, timestamps, user and host, sender and recipient, URL and redirect chain, security-control action, analyst decision, containment steps, and final disposition.
* **Resolution Requirement:** An alert should not be marked resolved merely because a firewall blocked a connection. Confirm the scope of activity and document why no further compromise was identified.

---

### 6. Metrics & Lessons Learned

* **True-Positive Rate:** The simulation recorded a **60% true-positive rate**. This indicates that detection and classification improved compared with earlier runs, while also showing room for better consistency and accuracy.
* **Mean Time to Resolve (MTTR):** MTTR and dwell time were longer than in previous runs. This suggests that investigation, correlation, or response execution should be streamlined.
* **Slowest Resolution:** The Firewall alert took the most time to resolve at **6.15 minutes**, compared with **4.57 minutes** for the suspicious-email alert.
* **Documentation Quality:** Reports should consistently include exact timestamps, user and host context, URL or domain details, supporting evidence, analyst reasoning, containment actions, and final impact assessment.
* **Improvement Goal:** Reduce investigation delays without lowering classification quality by using standardized triage checklists, enrichment tools, and clear escalation criteria.

---

## 🛡️ Defensive Engineering Standards

* **Email Security:** Use URL reputation analysis, attachment sandboxing, impersonation protection, SPF, DKIM, and DMARC enforcement.
* **Link Protection:** Apply time-of-click URL scanning, safe-link rewriting, DNS filtering, and blocking for confirmed phishing infrastructure.
* **Identity Protection:** Require phishing-resistant MFA where possible, monitor anomalous sign-ins, revoke sessions after suspected credential exposure, and enforce conditional access.
* **Endpoint Hardening:** Restrict macros and risky script execution, use application control, and monitor browsers or Office applications spawning shells or download tools.
* **Network Controls:** Apply egress filtering, maintain threat-intelligence blocklists, log DNS and proxy activity, and alert on repeated connections to suspicious destinations.
* **Security Awareness:** Conduct recurring, ethical phishing simulations with clear learning objectives. Avoid collecting real passwords, minimize personal data, and provide immediate educational feedback.
* **Operational Readiness:** Maintain a phishing-response playbook with ownership, severity criteria, evidence requirements, containment procedures, and escalation paths.
* **Continuous Improvement:** Review false positives, false negatives, MTTR, dwell time, click rates, reporting rates, and true-positive performance after every exercise.

---

## ✅ Completion Summary

The phishing simulation was completed successfully: the simulated breach was prevented, the relevant true-positive alerts were correctly identified, and both the firewall and email-security controls contributed to detection and containment. The principal improvement areas are reducing MTTR and dwell time, increasing classification accuracy beyond 60%, and recording more complete timing and contextual evidence in future reports.
