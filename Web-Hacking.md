# 📂 Web Hacking & Application Security

A technical reference guide detailing HTTP protocol architecture, client-side execution mechanics, database query security, and proxy-based web application auditing methodologies.

---

## 🗺️ Module Overview & Roadmap

| Topic / Room | Focus Area | Core Concepts & Defensive Indicators |
| :--- | :--- | :--- |
| **Web Application Basics** | Protocol Architecture | HTTP/HTTPS requests/responses, status codes, session headers, stateless mechanics |
| **JavaScript Essentials** | Client-Side Security | DOM execution contexts, client-side validation limitations, source-code endpoint discovery |
| **SQL Fundamentals** | Database Mechanics | Relational queries, SQL Injection (SQLi) mechanisms, parameterized defenses |
| **Burp Suite: The Basics** | Interception & Auditing | HTTP proxying, request manipulation, parameter tampering, response inspection |

---

## 📑 Detailed Topics & Technical Reference

### 1. Web Application Architecture & Protocol Mechanics
* **HTTP/HTTPS Request & Response Mechanics:**
  * **Verbs:** `GET` (data retrieval), `POST` (state modification), `PUT` (resource creation/replacement), `DELETE` (resource removal).
  * **Critical Headers:** `Host`, `User-Agent`, `Cookie`, `Set-Cookie` (`HttpOnly`, `Secure`, `SameSite`), `X-Forwarded-For` (IP preservation through reverse proxies).
  * **Status Codes:** `2xx` (Success), `3xx` (Redirection), `4xx` (Client-side errors: `401 Unauthorized`, `403 Forbidden`, `404 Not Found`), `5xx` (Server-side errors: `500 Internal Server Error`).
* **Defensive Monitoring & Logging:**
  * Web Server Access Logs (Apache/Nginx): Tracking HTTP verb anomalies, high-frequency status code distributions (`404`/`403` spikes during directory brute-forcing).

---

### 2. JavaScript Security & Client-Side Execution
* **Client-Side vs. Server-Side Execution:**
  * JavaScript runs within the user's browser context (DOM). Client-side security controls (e.g., input validation, disabled HTML form buttons) can be trivially bypassed by intercepting or modifying traffic.
* **Security Auditing Practices:**
  * Analyzing static JS bundles and scripts for exposed API keys, hidden endpoints, or hardcoded sensitive credentials.
  * Contextual risk of Cross-Site Scripting (XSS) where unformatted DOM manipulation (`innerHTML`, `eval()`) allows arbitrary script execution.

---

### 3. SQL Fundamentals & Query Security
* **Relational Database Structure:**
  * SQL execution flow using `SELECT`, `FROM`, `WHERE`, `JOIN`, and `UNION` clauses across database tables.
* **SQL Injection (SQLi) Mechanics:**
  * Occurs when untrusted user input is directly concatenated into dynamic database queries, altering logical execution (e.g., `' OR '1'='1`).
  * **In-Band SQLi:** Exploiting visible query output via `UNION SELECT` or triggering verbose database error messages.
  * **Blind SQLi:** Inferring data bit-by-bit using Boolean logic true/false responses or forced database delay sleep functions (`WAITFOR DELAY`, `pg_sleep()`).

---

### 4. Burp Suite Proxy Auditing Workflow
* **Core Tooling:**
  * **Intercept Proxy:** Captures HTTP/S traffic in real-time between browser and application server.
  * **Repeater:** Manually alters parameters, headers, and payloads to analyze backend validation behaviors.
  * **Intruder:** Automates targeted parameter fuzzing for vulnerability identification and endpoint enumeration.
* **Auditing Methodology:**
  * Parameter tampering, authority enforcement checks (IDOR testing), and token manipulation.

---

## 🛡️ Defensive Engineering & Remediation Baseline

* **Server-Side Validation:** Never rely on client-side controls (JavaScript/HTML) for security. Perform all authorization, input validation, and business logic enforcement strictly on the server.
* **Parameterized Queries (Prepared Statements):** Completely eradicate SQL Injection by separating executable code from user-supplied data at the database driver level.
* **Secure Cookie Management:** Enforce `HttpOnly` flags to prevent client-side script token access, `Secure` flags to enforce TLS transmission, and strict `SameSite` attributes to mitigate Cross-Site Request Forgery (CSRF).
