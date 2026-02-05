# Intune Compliance Policy

## Purpose
Define what “trusted device” means and produce a compliance signal for Conditional Access.

## Requirements
- BitLocker: Required
- Secure Boot: Required
- TPM: Required
- Minimum OS Version: 10.0.22000.0

## Targeting
- Group: DG-Windows-Intune-Managed

## Notes
- Compliance state feeds Entra Conditional Access decisions.
- Intune compliance may take 5–30 minutes to update in Entra.
