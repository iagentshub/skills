---
id: HOST_SECURITY
name: Host Security Audit
description: "Audit and harden host security. Use when security audits, firewall/SSH/update hardening, network exposure review, or server health checks are requested (laptop, workstation, VPS, Raspberry Pi)."
icon: 🔒
category: security
created_at: "2026-04-22"
updated_at: "2026-04-22"
---

# Host Security Audit

## Summary

Evaluate and harden the host, aligning to the user's risk tolerance without breaking access. Treat OS hardening as a separate, explicit set of steps.

## Key rules

- Require explicit approval before any state-changing action.
- Do not modify remote access configuration without confirming how the user connects.
- Prefer reversible, incremental changes with a rollback plan.
- If role/identity is unknown, provide recommendations only.
- Format: number each set of options so the user can respond with a single digit.
- System backups are recommended; try to verify their status.

## Workflow (follow in order)

### 0) Model check (non-blocking)

Before starting, check the current model. If below state-of-the-art, recommend switching. Don't block execution.

### 1) Establish context (read-only)

Try to infer points 1-5 from the environment before asking. Prefer simple, non-technical questions if you need confirmation.

Determine (in order):

1. OS and version (Linux/macOS/Windows), container vs host.
2. Privilege level (root/admin vs regular user).
3. Access method (local console, SSH, RDP, tailnet).
4. Network exposure (public IP, reverse proxy, tunnel).
5. Firewall status and open ports.
6. Backup system and status (Time Machine, system images).
7. Deployment context (local app, remote gateway, container/CI).
8. Disk encryption status (FileVault/LUKS/BitLocker).
9. Automatic OS security update status.
10. Usage mode (personal workstation vs headless server vs other).

Ask permission once to run read-only checks. If granted, run them by default and only ask about items you can't infer.

### 2) Run security audits (read-only)

Run checks per OS:

```bash
# OS status
uname -a         # Linux
sw_vers          # macOS

# Listening ports
ss -ltnp         # Linux
lsof -nP -iTCP -sTCP:LISTEN  # macOS

# Firewall status
ufw status       # Linux (Ubuntu/Debian)
/usr/libexec/ApplicationFirewall/socketfilterfw --getglobalstate  # macOS
```

### 3) Determine risk tolerance

After knowing the system context, ask the user for their risk profile (numbered options):

1. **Balanced/Workstation** (most common): active firewall with sensible defaults, remote access restricted to LAN or tailnet.
2. **Hardened VPS**: default-deny firewall, minimal ports, SSH key-only, no root login, automatic updates.
3. **Developer comfort**: more local services allowed, explicit exposure warnings, still audited.
4. **Custom**: user-defined restrictions.

### 4) Produce a remediation plan

Provide a plan including:

- Target profile
- Current posture summary
- Gaps vs target
- Step-by-step remediation with exact commands
- Strategy for preserving access and rollback
