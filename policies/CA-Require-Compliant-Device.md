# Conditional Access Policy: Require Compliant Device

## Purpose
Prevent access to sensitive apps unless device is Intune compliant.

## Scope
Users:
- Include: G-Users-Standard, G-Admins
- Exclude: G-BreakGlass-Exclude

Cloud Apps:
- Office 365

Grant Controls
- Grant access
- Require device to be marked as compliant

## Evidence
Attach screenshot of sign-in log showing CA failure when noncompliant, then success when compliant.
