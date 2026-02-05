# Intune Compliance Policy

## Purpose
Defines the minimum security baseline a Windows device must meet to be considered trusted.  
Compliance status is used by Microsoft Entra Conditional Access to control access to cloud applications.

---

## Scope
- Platform: Windows 10 and later
- Targeting: DG-Windows-Intune-Managed

---

## Compliance Requirements

| Control | Requirement |
|------|-----------|
| BitLocker | Required |
| Secure Boot | Required |
| TPM | Required |
| Firewall | Enabled |
| Minimum OS Version | 10.0.22000.0 |
| Defender Antimalware | Enabled (if available) |

---

## Enforcement
- Devices are marked **Noncompliant** immediately if requirements are not met.
- Noncompliant devices are blocked by Conditional Access policies requiring device compliance.

---

## Notes
- Compliance state updates typically within 5–30 minutes after device check-in.
- Firewall configuration is enforced via Intune security policies; compliance reporting confirms firewall state.
