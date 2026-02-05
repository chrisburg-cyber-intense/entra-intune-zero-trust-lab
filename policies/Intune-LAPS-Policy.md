# Windows LAPS Policy

## Purpose
Prevent lateral movement and credential reuse by automatically rotating local administrator passwords on managed Windows devices.

This eliminates shared local admin credentials across endpoints.

---

## Scope
- Platform: Windows 10 and later
- Targeting: DG-Windows-Intune-Managed

---

## Configuration
- Windows LAPS enabled
- Password rotation managed automatically
- Local Administrator account protected
- Passwords escrowed securely in Microsoft Entra ID

---

## Notes
- LAPS removes the need for static or shared local admin passwords.
- Password access is restricted to authorized administrators.
- Supports least-privilege and Zero Trust endpoint access principles.
