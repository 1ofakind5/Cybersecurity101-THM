# Open Source Intelligence (OSINT) & Search Skills

A reference guide for utilizing specialized search engines, threat intelligence databases, and vulnerability repositories to gather security information.

---

## 1. Shodan (IoT & Device Discovery)
Shodan is a search engine for internet-connected devices (servers, routers, webcams, industrial control systems). Unlike Google, which indexes web pages, Shodan indexes device banners.

### Core Use Cases:
* **Asset Discovery:** Finding exposed assets belonging to an organization.
* **Vulnerability Tracking:** Identifying devices running outdated, vulnerable firmware versions.

### Essential Search Filters:
* `port:` - Filters results by specific open ports (e.g., `port:21` for FTP, `port:3389` for RDP).
* `os:` - Filters by operating system (e.g., `os:"Windows 10"`).
* `country:` - Restricts results to a specific two-letter country code (e.g., `country:PH`).
* `product:` - Filters by software name (e.g., `product:"Apache httpd"`).

---

## 2. VirusTotal (Threat Intelligence)
VirusTotal is a service that analyzes files and URLs to detect malware and malicious content using over 70 antivirus scanners and URL/domain blacklisting services.

### Key Capabilities:
* **Hash Searching:** Checking MD5, SHA-1, or SHA-256 hashes of files to see if they have been previously flagged as malicious without uploading the actual file.
* **Domain/URL Analysis:** Investigating suspicious links, checking historical passive DNS records, and identifying associated IP addresses.
* **Community Comments:** Utilizing the community tab to read write-ups from other security analysts regarding specific malware behavior.

---

## 3. Vulnerability Databases (CVE & NVD)
When a security flaw is discovered, it is assigned a unique identifier to standardize tracking across the industry.

### Key Concepts:
* **CVE (Common Vulnerabilities and Exposures):** The standard dictionary identifier for publicly known cybersecurity vulnerabilities (Format: `CVE-YYYY-XXXXX`).
* **NVD (National Vulnerability Database):** A US government repository that synchronizes with CVE feeds and provides additional analysis, fine-grained details, and severity scoring.
* **CVSS (Common Vulnerability Scoring System):** A standardized framework (ranging from 0.0 to 10.0) used to assess the principal characteristics of a vulnerability and determine its severity impact.