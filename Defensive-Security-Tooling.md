# Defensive Security Tooling

A structured reference covering core tooling used in defensive security workflows — malware capability triage, data transformation, and dedicated analysis environments.

---

## CAPA: The Basics

**Category:** Malware Capability Analysis

**Overview**
CAPA is a static analysis tool that identifies the capabilities of a binary (e.g. persistence mechanisms, network communication, anti-analysis techniques) by matching code patterns against a rule set — without requiring execution.

**Key Concepts**
- Rule-based capability detection (MITRE ATT&CK–mapped where applicable)
- Static analysis — no sandbox execution required
- Useful for rapid triage before deeper reverse engineering

**Defensive Application**
Speeds up initial malware triage by surfacing likely behaviors, helping analysts prioritize which samples warrant deeper investigation.

---

## CyberChef: The Basics

**Category:** Data Decoding & Transformation

**Overview**
CyberChef is a browser-based "Swiss Army knife" for encoding, decoding, encryption, and data manipulation, built around a drag-and-drop recipe system.

**Key Concepts**
- Chainable operations (recipes) for multi-step decoding
- Common use: deobfuscating payloads, decoding encoded C2 traffic, parsing logs
- No installation required — runs client-side in the browser

**Defensive Application**
Frequently used to decode obfuscated strings, malicious scripts, or encoded network artifacts recovered during incident response.

---

## REMnux: Getting Started

**Category:** Malware Analysis Distribution

**Overview**
REMnux is a Linux toolkit purpose-built for reverse-engineering and analyzing malicious software, bundling utilities for static and dynamic analysis, network forensics, and memory analysis.

**Key Concepts**
- Curated collection of open-source malware analysis tools
- Supports both static and behavioral (dynamic) analysis workflows
- Complements Windows-based analysis environments

**Defensive Application**
Provides an isolated, tool-rich environment for safely dissecting malware samples and observing behavior without risking the host system.

---

## FlareVM: Arsenal of Tools

**Category:** Windows Malware Analysis Environment

**Overview**
FlareVM is a Windows-based malware analysis distribution developed by Mandiant (FireEye), installing a curated suite of reverse engineering, forensics, and analysis tools onto a Windows VM.

**Key Concepts**
- Automated installation of dozens of analysis tools (disassemblers, debuggers, network monitors)
- Designed specifically for Windows malware, which often behaves differently outside its native OS
- Pairs well with REMnux for cross-platform analysis coverage

**Defensive Application**
Enables safe, isolated analysis of Windows-targeted malware in its native environment, supporting both static inspection and controlled detonation.

---

## Summary

| Tool | Type | Primary Use |
|---|---|---|
| CAPA | Static Analysis | Rapid capability triage |
| CyberChef | Data Transformation | Decoding/deobfuscation |
| REMnux | Analysis Distro (Linux) | Malware reverse engineering |
| FlareVM | Analysis Distro (Windows) | Windows malware analysis |
