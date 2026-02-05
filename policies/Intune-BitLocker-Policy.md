# Intune BitLocker Policy

## Purpose
Ensure all corporate Windows devices encrypt data at rest to protect against data exposure in the event of device loss or theft.

BitLocker encryption is a foundational endpoint security control and a prerequisite for device compliance.

---

## Scope
- Platform: Windows 10 and later
- Targeting: DG-Windows-Intune-Managed

---

## Configuration
- BitLocker enabled for OS drive
- Encryption method: XTS-AES (Windows default)
- TPM required
- Recovery keys escrowed to Microsoft Entra ID

---

## Notes
- BitLocker state contributes to Intune device compliance evaluation.
- Recovery keys stored in Entra ID allow secure recovery without local key storage.
- Encryption is enforced silently where hardware requirements are met.
