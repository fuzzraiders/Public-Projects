# Methodology

This assessment followed a phase-based penetration testing workflow designed to simulate a realistic attack scenario in a controlled lab environment.

## Approach

- Black-box: no prior credentials or internal knowledge provided
- Goal: validate exploitability, demonstrate impact, and produce remediation-ready findings
- Evidence: collected privately (screenshots/logs) with a public, sanitized report produced from the validated results

## Phases

1. **Reconnaissance & enumeration**
   - Identify exposed services and application surface area
   - Baseline versions, routes, and user-facing functionality

2. **Vulnerability discovery**
   - Access control testing (authorization and object ownership)
   - Input handling and server-side execution paths
   - Authentication and credential management review

3. **Controlled exploitation (validated impact)**
   - Exploitability was confirmed only to the extent required to prove risk
   - No denial-of-service testing was performed

4. **Post-exploitation and privilege escalation**
   - Confirmed the extent of compromise and critical paths
   - Assessed internal-only services reachable after initial access

5. **Reporting**
   - Findings written with root cause, risk, and remediation steps
   - Public version intentionally removes step-by-step exploit instructions

## Tooling (representative)

- Network enumeration, web proxying, directory/content discovery
- Secure remote access tooling for validated post-exploitation steps
- Offline password auditing for exposed hashes (where applicable)
