# 🌐 Networking & Traffic Analysis

A comprehensive reference guide covering fundamental computer networking concepts, protocol security, packet capturing, and network discovery tools from a defensive and SOC analyst perspective.

---

## 1. Network Concepts & Architecture Baselines

Understanding network models and addressing is critical for identifying unauthorized traffic, analyzing firewall rules, and tracking host communications.

### Key Conceptual Layers (OSI vs. TCP/IP)
* **Application / Presentation / Session (Layers 5–7):** High-level protocols (`HTTP`, `DNS`, `SSH`, `FTP`) where payload inspection occurs.
* **Transport (Layer 4):** Manages end-to-end communication via **TCP** (connection-oriented, reliable) or **UDP** (connectionless, fast).
* **Network (Layer 3):** Handles logical IP routing (`IPv4`, `IPv6`, `ICMP`).
* **Data Link / Physical (Layers 1–2):** Handles physical hardware communication (`MAC addresses`, `Ethernet`, `ARP`).

---

## 2. Core vs. Secure Protocols

Defenders evaluate protocol safety to prevent cleartext credential harvesting, eavesdropping, and man-in-the-middle (MitM) attacks.

| Function | Insecure / Cleartext Protocol | Secure / Encrypted Equivalent | Defensive Focus |
| :--- | :--- | :--- | :--- |
| **Web Browsing** | HTTP (Port 80) | HTTPS (TLS/SSL - Port 443) | Ensure legacy HTTP redirects to HTTPS; inspect TLS certificates for anomalies. |
| **Remote Shell** | Telnet (Port 23) | SSH (Port 22) | Flag any active Telnet traffic as high-risk cleartext administrative access. |
| **File Transfer** | FTP (Ports 20/21) | SFTP / FTPS (Port 22 / 990) | Monitor file transfers for unauthorized data exfiltration. |
| **Name Resolution** | DNS (UDP Port 53) | DoH / DoT (Port 443 / 853) | Monitor DNS query logs for tunneling, C2 beacons, or suspicious domain lookups. |

---

## 3. Packet Capture & Traffic Analysis

Capturing and analyzing network traffic allows blue teams to perform deep packet inspection (DPI) during incident investigation.

### Wireshark (Graphical Packet Analyzer)
* **Purpose:** Interactive analysis of live PCAP files.
* **Essential Display Filters:**
  * `ip.addr == 192.168.1.50` – Filter all traffic involving a specific IP address.
  * `http.request.method == "POST"` – Identify outbound form submissions or data uploads.
  * `tcp.flags.syn == 1 and tcp.flags.ack == 0` – Detect potential SYN scan activity.

### Tcpdump (Command-Line Packet Analyzer)
* **Purpose:** Lightweight packet capture utility ideal for remote headless Linux servers.
* **Key Commands:**
  * `tcpdump -i eth0 -n` – Capture live interface traffic without resolving hostnames.
  * `tcpdump -r capture.pcap 'port 80 or port 443'` – Read a PCAP file filtered for web traffic.
  * `tcpdump -i eth0 -w output.pcap src host 10.10.10.5` – Capture and save traffic originating from a single target host.

---

## 4. Network Discovery & Auditing (Nmap)

Network mappers are used defensively to locate active hosts, identify unauthorized running services, and audit exposed open ports.

### Basic Reconnaissance Commands

```bash
# Ping sweep to discover live hosts on a subnet without port scanning
nmap -sn 192.168.1.0/24

# Default service version detection scan against a target host
nmap -sV 192.168.1.100

# Fast scan of the top 100 most common network ports
nmap -F 192.168.1.100
