# Detection & hardening notes (blue-team value)

This section describes defensive ideas at a high level (no exploit strings).

## Telemetry to collect

- Web server access logs (status codes, routes, query patterns)
- Application logs (auth events, admin actions, file access)
- Host process creation and command-line auditing (where feasible)
- SSH authentication logs (success/failure, source, user)
- Package/service inventory and patch status for internal services

## Detection ideas

### 1) Access-control abuse / object enumeration (IDOR-style)

- Alert on repeated access to many objects/files across multiple user identifiers in a short time window
- Detect unusual 200 responses for resources outside the normal user flow
- Add rate limiting and anomaly thresholds on sensitive endpoints

### 2) Suspicious server-side command execution signals

- Alert on web-app worker spawning shells or unusual binaries
- Monitor for abnormal child processes from the web service account
- Correlate admin actions with unusual process/network activity

### 3) Credential compromise and remote access

- Alert on new SSH logins from unusual sources or at unusual times
- Detect sudden privilege changes, new keys, or persistence mechanisms
- Enforce MFA where possible; disable password auth for SSH in hardened environments

### 4) Internal service exploitation / patch gaps

- Maintain a service inventory (what runs internally, versions, owners)
- Alert on logins to internal admin panels outside approved admin hosts
- Patch SLAs + scanning: treat internal panels as high-risk even if not internet-facing

## Hardening priorities (recommended order)

1. Fix broken access control (authorization) and add monitoring
2. Remove command execution paths and lock down admin features
3. Strengthen credential storage/policies and reduce secret exposure
4. Patch and restrict internal admin services; segment privileges
