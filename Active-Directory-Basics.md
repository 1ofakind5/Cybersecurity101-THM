# 🏢 Active Directory Basics: Enterprise Architecture & Defense

A fundamental reference guide detailing the architecture of Windows Active Directory (AD), core enterprise authentication mechanisms, and defensive enumeration strategies used to map domain environments.

---

## 1. Active Directory Architecture & Components

Active Directory Domain Services (AD DS) is the centralized management system used by enterprises to handle users, computers, and access permissions across a network.

### The Structural Hierarchy
Defenders must understand the layout of an organization's network to spot anomalies or unauthorized trust boundaries:
* **Domain Controller (DC):** The crown jewel server running AD DS. It manages all security authentication requests (logins) and holds the central data store.
* **Objects:** The fundamental building blocks of AD, representing physical entities like **Users**, **Computers**, **Groups**, and **Printers**.
* **Organizational Units (OUs):** Administrative containers used to group objects within a domain. Group Policies are linked to OUs to enforce security baselines.
* **Domains:** A logical grouping of network objects sharing a central AD database.
* **Trees & Forests:** A **Tree** is a collection of domains sharing a contiguous namespace (e.g., `corp.local` and `dev.corp.local`). A **Forest** is the top-level security boundary containing multiple trees that share a common schema and global catalog.

---

## 2. Core Protocols & Network Authentication

Understanding how authentication functions in a Windows network allows security teams to identify lateral movement and identity-based attacks.

### 1. LDAP (Lightweight Directory Access Protocol)
* **Purpose:** The language used to query and interact with the Active Directory database (reading/writing object attributes).
* **Defensive Focus:** Unencrypted LDAP traffic (Port 389) exposes credentials in cleartext. Blue teams push for **LDAPS (LDAP over SSL/TLS - Port 636)** to secure internal queries.

### 2. Kerberos Authentication (The Enterprise Standard)
Kerberos relies on symmetric cryptography and a trusted third party—the Domain Controller's **Key Distribution Center (KDC)**—to authenticate identities without sending passwords over the wire.

#### The 3-Step Kerberos Handshake:
1. **Authentication Service (AS) Request/Exchange:** The user sends a pre-authentication string encrypted with their password hash. The KDC verifies it and issues a **Ticket Granting Ticket (TGT)**.
2. **Ticket Granting Service (TGS) Request/Exchange:** The user presents their TGT to the KDC to request access to a specific network resource (like a file share). The KDC verifies the TGT and issues a **Service Ticket (TGS)**.
3. **Application Request:** The user presents the Service Ticket directly to the target server to gain authorized access.

> 🛡️ **Defensive Insight:** Kerberos is heavily targeted by attackers via techniques like **Kerberoasting** (cracking service tickets offline) or **Golden Ticket** attacks (forging a TGT after stealing the KDC's master key, `krbtgt`). Monitoring anomalous ticket lifetimes and ticket requests is a key Blue Team priority.

---

## 3. Defensive AD Enumeration & Auditing

Before you can defend an Active Directory environment, you must audit what exists within it. Blue Teamers use both built-in Windows utilities and specialized tools to find weak configurations.

### Essential Built-in CLI Tools
Run from standard command environments to query basic domain architecture:

| Command | Purpose / Defensive Focus |
| :--- | :--- |
| `net user /domain` | Lists all user accounts registered in the Active Directory domain. |
| `net group "Domain Admins" /domain` | Audits the highest-privileged group to ensure no unauthorized accounts have been added. |
| `nltest /dclist:domain_name` | Locates and maps out all active Domain Controllers within the target domain. |

### Specialized Security Auditing Tools
* **BloodHound:** A graphical tool that uses graph theory to map out hidden, complex relationships and permission paths within an AD environment. Blue Teams use it to visualize and close unintended attack paths to Domain Admins.
* **SharpHound:** The command-line ingestor used to collect AD relationship data (users, groups, sessions, permissions) to feed into BloodHound.
