# 📂 Cryptography

A technical documentation module covering fundamental cryptographic principles, public-key infrastructure, hash functions, and password auditing methodology.

---

## 🗺️ Module Overview & Roadmap

| Topic / Room | Focus Area | Core Concepts & Tools |
| :--- | :--- | :--- |
| **Cryptography Basics** | Theoretical Fundamentals | Symmetric/Asymmetric Ciphers, Plaintext, Ciphertext, Entropy |
| **Public Key Cryptography Basics** | Key Infrastructure | RSA, Diffie-Hellman, Key Pairs (Public/Private), Digital Signatures |
| **Hashing Basics** | Data Integrity & Verification | MD5, SHA-256, One-Way Functions, Collisions, Salt Implementation |
| **John the Ripper: The Basics** | Password Hash Auditing | Hash Identification, Wordlists, Rule-Based Cracking, Hashcat vs. JtR |

---

## 📑 Detailed Topics & Documentation Index

### 1. Cryptography Basics
* **Overview:** Introduction to fundamental cryptographic terminology and operational modes.
* **Key Topics:**
  * Symmetric encryption algorithms (AES, DES, 3DES).
  * Stream ciphers vs. Block ciphers (modes of operation: ECB, CBC).
  * Confidentiality, Integrity, and Authenticity (CIA Triad alignment).

---

### 2. Public Key Cryptography Basics
* **Overview:** Mechanics of asymmetric encryption and key exchange protocols.
* **Key Topics:**
  * Public/Private key pair math and usage protocols.
  * Key Exchange Algorithms (Diffie-Hellman / ECDHE).
  * Digital Signatures and Certificate Authorities (PKI Architecture).

---

### 3. Hashing Basics
* **Overview:** Cryptographic hash functions and non-reversible message digest algorithms.
* **Key Topics:**
  * Properties of secure hash functions (Pre-image resistance, Collision resistance).
  * Common hashing standards (MD5, SHA-1, SHA-256, SHA-512).
  * Salt and Pepper mechanics to prevent precomputed Rainbow Table attacks.

---

### 4. John the Ripper: The Basics
* **Overview:** Practical application of John the Ripper for auditing user password strength and verifying system hash storage compliance.
* **Key Topics:**
  * Identifying hash formats (`hash-identifier`, `john` auto-detection).
  * Wordlist attacks using `rockyou.txt`.
  * Custom rule creation for mangling wordlists (`/etc/john/john.conf`).
  * Unshadowing Linux user files (`unshadow /etc/passwd /etc/shadow`).

---

## 🛡️ Defensive Engineering Standards

* **Hash Storage:** Enforce modern key derivation functions (Argon2id, bcrypt, PBKDF2) instead of raw single-iteration hashes (MD5, SHA-1) for credential storage.
* **Key Management:** Store private keys securely in Hardware Security Modules (HSM) or secret management systems; strictly enforce minimum key length standards (RSA $\ge 2048$-bit, ECC $\ge 256$-bit).
