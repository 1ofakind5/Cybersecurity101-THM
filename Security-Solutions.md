# Security Solutions

> A structured defensive-security reference covering the core detection and prevention technologies every SOC analyst relies on: firewalls, SIEM, IDS, vulnerability scanning, and Snort.

---

## Overview

This module maps the "Security Solutions" learning path, which builds a layered defense model — starting at the network perimeter (firewalls), moving into visibility and correlation (SIEM), then into active threat detection (IDS/Snort) and proactive weakness discovery (vulnerability scanning).

| # | Room | Focus Area |
|---|------|------------|
| 1 | Firewall Fundamentals | Perimeter defense, traffic filtering |
| 2 | Introduction to SIEM | Log aggregation, correlation, alerting |
| 3 | IDS Fundamentals | Intrusion detection concepts |
| 4 | Vulnerability Scanner Overview | Weakness identification |
| 5 | Snort Challenge – The Basics | Hands-on rule writing & detection |

---

## 1. Firewall Fundamentals

**Purpose:** First line of defense — controls what traffic is allowed in and out of a network based on defined rules.

**Key Concepts:**
- **Packet filtering** — inspects headers (IP, port, protocol) against an access control list (ACL)
- **Stateful inspection** — tracks the state of active connections, not just individual packets
- **Application-layer (proxy) firewalls** — inspect traffic content at Layer 7, not just headers
- **Next-Generation Firewalls (NGFW)** — combine stateful inspection with intrusion prevention, deep packet inspection, and application awareness

**Defensive Notes:**
- Default-deny is the safest baseline: block everything, explicitly allow only what's needed
- Rule order matters — most specific/restrictive rules should be evaluated first
- Logging denied traffic is as important as logging allowed traffic for detecting scan/probe activity

---

## 2. Introduction to SIEM

**Purpose:** Centralizes log collection from across the environment (endpoints, firewalls, servers, applications) to enable correlation, alerting, and investigation.

**Key Concepts:**
- **Log aggregation** — normalizes data from disparate sources into a common format
- **Correlation rules** — link related events across sources to surface incidents that a single log source would miss
- **Dashboards & alerting** — give analysts real-time visibility and automated notification of suspicious activity
- **Common platforms:** Splunk, Elastic (ELK), Microsoft Sentinel, QRadar

**Defensive Notes:**
- A SIEM is only as good as its log sources and tuning — poor tuning leads to alert fatigue
- Retention policy matters for forensic investigations after the fact
- Use case development (mapping detections to MITRE ATT&CK) improves signal quality over raw log dumping

---

## 3. IDS Fundamentals

**Purpose:** Monitors network or host activity for signs of malicious behavior and generates alerts (detection, not prevention, unless deployed as IPS).

**Key Concepts:**
- **NIDS (Network-based)** — monitors traffic at a network segment/chokepoint
- **HIDS (Host-based)** — monitors activity on an individual system (file integrity, process behavior)
- **Signature-based detection** — matches traffic against known-bad patterns; fast but blind to novel threats
- **Anomaly-based detection** — flags deviations from an established baseline; catches unknowns but has higher false-positive rates

**Defensive Notes:**
- Placement determines visibility — a NIDS behind a firewall sees different traffic than one in front of it
- IDS/IPS effectiveness depends heavily on signature/ruleset freshness
- Pairs naturally with SIEM — IDS generates the alert, SIEM provides the context

---

## 4. Vulnerability Scanner Overview

**Purpose:** Proactively identifies known weaknesses (missing patches, misconfigurations, outdated software) before an attacker can exploit them.

**Key Concepts:**
- **Authenticated vs. unauthenticated scans** — authenticated scans log in to check internals; unauthenticated scans mimic an external attacker's view
- **CVE/CVSS scoring** — standardizes how findings are identified and prioritized by severity
- **Common tools:** Nessus, OpenVAS, Qualys

**Defensive Notes:**
- Scanning is a snapshot, not continuous protection — schedule regular scan cycles
- False positives are common; findings should be validated before remediation effort is spent
- Prioritize by exploitability and asset criticality, not CVSS score alone

---

## 5. Snort Challenge – The Basics

**Purpose:** Hands-on application of IDS concepts using Snort, an open-source NIDS/IPS engine, to write and test detection rules.

**Key Concepts:**
- **Snort modes:** sniffer, packet logger, NIDS/IPS mode
- **Rule anatomy:**
  ```
  action protocol src_ip src_port -> dst_ip dst_port (rule options)
  ```
  Example:
  ```
  alert tcp any any -> any 80 (msg:"HTTP traffic detected"; sid:1000001; rev:1;)
  ```
- **Rule options** — `msg` (alert text), `sid` (unique rule ID), `content` (payload match), `flags` (TCP flags)

**Defensive Notes:**
- Well-written rules balance specificity (avoid false positives) with coverage (avoid missing variants)
- Test rules against known traffic captures (pcaps) before deploying to production
- Custom rules should be documented with intent — why the rule exists, what it detects, and its expected false-positive rate
