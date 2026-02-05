# Defender Attack Surface Reduction (ASR) Policy

## Purpose
Disrupt common enterprise attack chains (phishing/macro abuse/payload execution/credential theft).

## Rules (Recommended for Lab)
BLOCK:
- Block credential stealing from LSASS
- Block Office apps from creating child processes
- Block Office apps from creating executable content
- Block Win32 API calls from Office macros
- Block executable files unless trusted by prevalence/age/trusted list

AUDIT (optional):
- PSExec/WMI process creation
- Unsigned/untrusted processes from USB

## Targeting
- Group: DG-Windows-Intune-Managed
