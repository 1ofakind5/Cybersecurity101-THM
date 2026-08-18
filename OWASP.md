# OWASP Top 10 (2025)

A structured reference covering key application security risk categories from the OWASP Top 10 (2025) — focused on identifying root causes and applying defensive remediation rather than exploit walkthroughs.

---

## OWASP Top 10 2025: IAAA Failures

**Category:** Identification, Authentication, Authorization & Accounting

**Overview**
IAAA failures occur when an application fails to properly verify who a user is (Identification/Authentication), what they're allowed to do (Authorization), or fails to reliably track their actions (Accounting). This spans broken authentication flows, weak session management, and improper access control enforcement.

**Key Concepts**
- Authentication ≠ Authorization — verifying identity is separate from verifying permission
- Common failure points: weak password policies, predictable session tokens, missing MFA, broken object-level access control (BOLA)
- Accounting/logging gaps make it difficult to detect or investigate abuse after the fact

**Defensive Application**
Enforce strong authentication (MFA, secure session handling), apply least-privilege access controls checked server-side on every request, and ensure all authentication/authorization events are logged for audit and detection.

---

## OWASP Top 10 2025: Application Design Flaws

**Category:** Insecure Design

**Overview**
Design flaws are security weaknesses baked into an application's architecture before a single line of vulnerable code is written — missing threat modeling, insecure business logic, or a lack of security requirements during the design phase.

**Key Concepts**
- Distinct from implementation bugs — a "correctly coded" feature can still be insecurely *designed*
- Root causes: absence of threat modeling, insecure default configurations, missing rate limiting/abuse-case handling
- Business logic flaws (e.g. trusting client-side price/quantity values) fall into this category

**Defensive Application**
Incorporate threat modeling and secure design reviews early in the SDLC, apply secure-by-default configurations, and explicitly design for abuse cases (rate limiting, workflow validation) rather than only "happy path" functionality.

---

## OWASP Top 10 2025: Insecure Data Handling

**Category:** Sensitive Data Exposure / Cryptographic Failures

**Overview**
Insecure data handling covers failures in how sensitive data is stored, transmitted, or processed — including weak or missing encryption, improper data classification, and unnecessary retention of sensitive information.

**Key Concepts**
- Data should be protected both at rest (storage) and in transit (transmission)
- Common issues: weak/outdated cryptographic algorithms, hardcoded secrets, storing more sensitive data than necessary
- Data classification is a prerequisite — you can't protect what you haven't identified as sensitive

**Defensive Application**
Encrypt sensitive data at rest and in transit using current standards, minimize data retention to what's operationally necessary, and eliminate hardcoded credentials/keys in favor of secure secret management.

---

## Summary

| Topic | Root Issue | Primary Defense |
|---|---|---|
| IAAA Failures | Broken auth/access control | MFA, server-side least-privilege checks, audit logging |
| Application Design Flaws | Missing security in design phase | Threat modeling, secure defaults, abuse-case handling |
| Insecure Data Handling | Weak protection of sensitive data | Encryption, data minimization, secure secret management |
