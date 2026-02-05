# Conditional Access Policy – Admin Access (Strict)

## Purpose
Apply stricter access controls to privileged accounts to reduce the risk associated with administrative access.

Administrative users are required to meet stronger security conditions than standard users.

---

## Scope
- Users: G-Admins
- Exclusions: G-BreakGlass-Exclude
- Cloud apps: All cloud apps

---

## Grant Controls
- Require multi-factor authentication
- Require device to be marked as compliant

---

## Notes
- This policy enforces defense-in-depth for privileged access.
- Admins must authenticate with MFA from a compliant, Intune-managed device.
- Designed to align with least-privilege and privileged access best practices.
