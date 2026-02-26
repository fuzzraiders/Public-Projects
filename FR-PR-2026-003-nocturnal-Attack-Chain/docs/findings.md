# Findings (sanitized)

Severity reflects practical impact in this lab environment and how the issues chained together.

---

## FR-F-01 - Broken access control (IDOR)
**Severity:** High  
**Category:** OWASP Broken Access Control

### What happened (summary)
A file retrieval feature allowed access to objects that were not owned by the requesting user. By manipulating identifiers, an attacker could access other users' documents.

### Impact
- Unauthorized access to sensitive files
- Information disclosure that enabled follow-on compromise

### Root cause
- Missing server-side authorization checks
- Object ownership not enforced consistently

### Recommendation
- Enforce authorization server-side for every object request
- Bind file/object access to the authenticated session and ownership
- Centralize authorization checks (middleware / policy layer)
- Add monitoring for abnormal access patterns (enumeration)

---

## FR-F-02 - Command injection in administrative backup feature
**Severity:** Critical  
**Category:** Injection / Remote Code Execution

### What happened (summary)
An administrative function accepted user-controlled input that was executed in an OS command context. This allowed command execution and data exposure.

### Impact
- Remote code execution within the application context
- Database exposure and credential compromise risk
- Enabled escalation from web access to system access

### Root cause
- Unsanitized input reaching command execution
- Unsafe design pattern: concatenating inputs into OS commands

### Recommendation
- Remove shell execution from user-controlled flows
- Use safe APIs (no shell) and strict allow-listing for arguments
- Implement defense-in-depth: input validation + output encoding + least privilege
- Add application security tests for admin features (often overlooked)

---

## FR-F-03 - Weak credential management enabling offline cracking and SSH access
**Severity:** High  
**Category:** Authentication / Credential Management

### What happened (summary)
Credential material exposed during the attack chain enabled offline password cracking. Recovered credentials allowed authenticated remote access as a valid user.

### Impact
- Authenticated system access and lateral movement
- Access to additional internal-only services

### Root cause
- Weak passwords and/or weak password policy enforcement
- Insufficient protection of password material (hashes) and related secrets

### Recommendation
- Enforce strong password policy + MFA where possible
- Use modern password hashing (Argon2id/bcrypt) and proper parameters
- Reduce exposure of sensitive credential data; segment and protect backups
- Monitor for suspicious remote logins and credential stuffing signals

---

## FR-F-04 - Unpatched internal management service enabling privilege escalation
**Severity:** Critical  
**Category:** Security Misconfiguration / Vulnerable Components

### What happened (summary)
An internal-only management service accessible after initial compromise was running a known-vulnerable version. This enabled privilege escalation to root in the lab environment.

### Impact
- Full root compromise when chained with earlier findings
- Complete loss of confidentiality, integrity, and availability

### Root cause
- Patch management failure (vulnerable component remained unpatched)
- Internal service exposure without additional hardening/controls

### Recommendation
- Apply timely patches and maintain an inventory of internal services
- Restrict internal admin services with network controls and host-based firewall rules
- Implement least privilege and segmentation so internal services do not imply root compromise
- Add continuous vulnerability scanning and change-control gates for admin panels
