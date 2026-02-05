# Conditional Access Policy – Require MFA (All Users)

## Purpose
Establish a baseline identity protection control by requiring multi-factor authentication for all users accessing cloud applications.

This policy ensures that stolen or weak credentials alone are not sufficient to gain access.

---

## Scope
- Users: All users
- Exclusions: G-BreakGlass-Exclude
- Cloud apps: All cloud apps

---

## Grant Controls
- Require multi-factor authentication

---

## Notes
- This policy provides foundational identity security across the tenant.
- Break-glass accounts are excluded to prevent administrative lockout.
- MFA enforcement is separated from device trust to keep policy intent clear.
