# 🕸️ Web Security Monitoring

A learning module covering how Security Operations Center (SOC) analysts monitor the web ecosystem, detect common web-based threats, and investigate attacks targeting web servers and applications. It covers web fundamentals, log and traffic analysis for attack detection, identifying web shells, and mitigating Denial-of-Service (DDoS) attacks.

## 🎯 Learning Objectives

By completing this module, I developed an understanding of how SOC teams:

- Explain how the web works, including HTTP/HTTPS protocols, status codes, and web architecture.
- Identify common website security risks and the protections used to secure web traffic.
- Detect web attacks by analysing web server logs, network traffic, and application behaviour.
- Recognise the signs of web shell deployment and execution through log, file system, and network analysis.
- Understand Denial-of-Service (DDoS) attacks, their detection techniques, and strategies for protection.
- Apply SOC-oriented investigation techniques to real-world web security scenarios.

## 🗺️ Module Roadmap

| Topic | Main Focus | Practical SOC Value |
| :--- | :--- | :--- |
| **Web Security Essentials** | How the web works, common risks, and protections | Understand the attack surface and normal web behaviour |
| **Detecting Web Attacks** | Log and network traffic analysis for web attacks | Identify exploitation attempts and malicious requests |
| **Detecting Web Shells** | Log, file system, and network analysis for web shells | Detect persistence mechanisms on compromised web servers |
| **Detecting Web DDoS** | DoS/DDoS attack detection and protection strategies | Recognise traffic floods and mitigate service disruption |

---

## 📑 Detailed Topics

### 1. Web Security Essentials

**Web Security Essentials** establishes the foundation: how the web functions, the components that deliver a web request, and the common security risks that organisations face. It covers the client-server model, HTTP methods and status codes, and the difference between vulnerabilities on the server side versus the client side.

#### Core Concepts

| Concept | Description |
| :--- | :--- |
| **HTTP/HTTPS** | The protocols that govern web communication; HTTPS adds TLS encryption. |
| **Request/Response** | The client sends a request (GET, POST, etc.), and the server returns a response (200, 404, 500, etc.). |
| **Headers** | Metadata such as User-Agent, Host, Cookie, and Content-Type that provide context to requests and responses. |
| **Web Architecture** | Load balancers, reverse proxies, web servers (Apache, Nginx, IIS), and application servers. |

#### Common Web Risks

| Risk | Description |
| :--- | :--- |
| **Injection** | SQL injection, command injection, and Cross-Site Scripting (XSS) where untrusted data is executed. |
| **Broken Authentication** | Weak credentials, session hijacking, or flawed login mechanisms. |
| **Sensitive Data Exposure** | Cleartext transmission of passwords or credit card data. |
| **Security Misconfiguration** | Default credentials, verbose error messages, or unnecessary services exposed. |

#### Defensive Application

- Understanding the normal flow of web traffic allows analysts to spot anomalies such as unusual request methods or unexpected status codes.
- Knowing common risks helps prioritise alerts—e.g., a SQL injection attempt is higher priority than a 404 error.
- Recognising legitimate web behaviour (like a browser requesting assets) helps filter out noise when investigating logs.

---

### 2. Detecting Web Attacks

**Detecting Web Attacks** focuses on the investigative techniques used to identify attacks against web applications. This involves analysing web server access logs, application logs, and network traffic captures (PCAPs) to find evidence of exploitation attempts, such as directory traversal, SQL injection, and cross-site scripting.

#### Investigation Areas

| Data Source | What to Look For |
| :--- | :--- |
| **Web Server Logs** | Unusual HTTP methods (PUT, DELETE), suspicious User-Agents, long or encoded URLs, and repeated 403/500 errors. |
| **Application Logs** | Stack traces, database errors, and unexpected input validation failures. |
| **Network Traffic** | Cleartext HTTP requests containing attack payloads, such as `' OR 1=1--` or `<script>alert(1)</script>`. |
| **WAF Logs** | Blocked requests that indicate a probing attempt. |

#### Common Attack Patterns

| Attack | Log/Traffic Indicator |
| :--- | :--- |
| **SQL Injection** | `SELECT`, `UNION`, `OR 1=1` in URL parameters or POST bodies. |
| **Directory Traversal** | `../` sequences in request paths. |
| **XSS** | `<script>` tags or JavaScript event handlers in input fields. |
| **Brute Force** | Multiple POST requests to `/login` from the same IP with different credentials. |

#### Defensive Application

- Correlating web logs with network traffic confirms whether an attack was successful or blocked.
- Identifying the attack vector (e.g., a vulnerable plugin) helps guide remediation.
- Understanding log formats (Common Log Format, Combined Log Format) is essential for efficient parsing.

---

### 3. Detecting Web Shells

**Detecting Web Shells** dives into one of the most common persistence mechanisms used by attackers after compromising a web server. A web shell is a malicious script (PHP, ASP, JSP) uploaded to a server that provides remote access. This room covers how to detect them through log analysis, file system inspection, and network traffic monitoring.

#### Detection Methods

| Method | Indicators |
| :--- | :--- |
| **Log Analysis** | Requests to unusual file names (e.g., `shell.php`, `cmd.aspx`), POST requests with base64-encoded commands, or requests from unusual User-Agents. |
| **File System** | Newly created files in web-accessible directories (e.g., `/uploads/`, `/images/`), files with suspicious permissions, or obfuscated code. |
| **Network Traffic** | Outbound connections from the web server to unknown IPs, or HTTP responses containing command output. |
| **Process Monitoring** | Web server processes (e.g., `w3wp.exe`, `apache2`) spawning child processes like `cmd.exe` or `/bin/bash`. |

#### Defensive Application

- Web shells often blend in with legitimate traffic, so behavioural baselines are crucial.
- Monitoring file integrity on web servers helps detect unauthorised file creation.
- Analysing command-and-control (C2) traffic from a web shell can reveal the attacker's infrastructure.

---

### 4. Detecting Web DDoS

**Detecting Web DDoS** covers Denial-of-Service (DoS) and Distributed Denial-of-Service (DDoS) attacks targeting web services. It explains how these attacks work, how to detect them from traffic patterns and logs, and the strategies used to protect web infrastructure.

#### Attack Types

| Type | Description |
| :--- | :--- |
| **Volumetric** | Flooding the network with massive amounts of traffic (e.g., UDP floods, ICMP floods). |
| **Protocol** | Exhausting server resources by exploiting protocol behaviour (e.g., SYN floods). |
| **Application Layer** | Targeting specific web endpoints with seemingly legitimate requests (e.g., HTTP floods, Slowloris). |

#### Detection Techniques

| Indicator | Description |
| :--- | :--- |
| **Traffic Spikes** | Sudden, massive increases in requests from many IPs. |
| **Resource Exhaustion** | High CPU, memory, or bandwidth usage on web servers. |
| **Log Patterns** | A single endpoint receiving thousands of requests per second. |
| **Geographic Anomalies** | Traffic originating from regions where the organisation has no user base. |

#### Defensive Application

- Rate limiting and caching can mitigate application-layer DDoS.
- Content Delivery Networks (CDNs) and DDoS protection services absorb volumetric attacks.
- Distinguishing between a DDoS attack and a legitimate traffic surge (e.g., a viral marketing campaign) is a key analytical skill.
