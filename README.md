# [Intune Compliance + Conditional Access Zero Trust Lab](https://github.com/chrisburg-cyber-intense/entra-intune-zero-trust-lab) Project

In this project, we simulate building a scalable, enterprise-style Zero Trust access model using Microsoft Intune and Microsoft Entra ID (Azure AD).

_**Inception State:**_ the organization has no device compliance baseline, no enforced encryption, and no Conditional Access guardrails.

_**Completion State:**_ a formal device security baseline is enforced (BitLocker + ASR + LAPS), Intune compliance is measured, and Conditional Access blocks access to sensitive apps unless the device is compliant (with break-glass protections in place).

---

<img width="1000" alt="image" src="https://github.com/chrisburg-cyber-intense/entra-intune-zero-trust-lab/blob/d0bf78ac6fa2171ff8500371d31f5cebf211c665/diagrams/architecture.png">

# Technology Utilized
- Microsoft Intune (device management + compliance)
- Microsoft Entra ID / Azure AD (Conditional Access + identity control plane)
- Microsoft Defender (endpoint security signals, ASR rules)
- Windows 11 VM (repeatable test device)

---

# Table of Contents

- [Step 1: Lab Tenant and Identity Setup](#step-1-lab-tenant-and-identity-setup)
- [Step 2: Groups and Access Model Design](#step-2-groups-and-access-model-design)
- [Step 3: Intune Enrollment Configuration](#step-3-intune-enrollment-configuration)
- [Step 4: Device Enrollment and Validation](#step-4-device-enrollment-and-validation)
- [Step 5: Intune Compliance Policy (The Gate)](#step-5-intune-compliance-policy-the-gate)
- [Step 6: Security Baseline Configuration (Hardening)](#step-6-security-baseline-configuration-hardening)
- [Step 7: Conditional Access Policies (The Payoff)](#step-7-conditional-access-policies-the-payoff)
- [Step 8: Before/After Demo Scenario (Blocked vs Allowed)](#step-8-beforeafter-demo-scenario-blocked-vs-allowed)
- [Evidence: Sign-In Logs and Compliance Reporting](#evidence-sign-in-logs-and-compliance-reporting)
- [Final Summary: What This Lab Demonstrates](#final-summary-what-this-lab-demonstrates)

---

## Step 1: Lab Tenant and Identity Setup

This phase establishes a dedicated lab tenant to safely enforce strict security policies without impacting a production environment. A standard user, privileged admin user, and break-glass account are created to model enterprise access separation.

**Deliverables:**
- Tenant created with Intune + Entra licensing enabled
- `lab.user1` (standard user)
- `lab.admin1` (privileged admin user)
- `lab.breakglass` (emergency access account)

<img width="1000" alt="image" src="https://github.com/chrisburg-cyber-intense/entra-intune-zero-trust-lab/blob/efc223cb3f71df9c5d47f4a4120176a29bf66241/screenshots/Step%201.1.png">

---

## Step 2: Groups and Access Model Design

This phase creates security groups to support scalable policy targeting and privileged access management. The model separates standard users, admins, and emergency accounts to prevent lockout and enforce least privilege.

**Groups Created:**
- `G-Users-Standard` (Security group, Assigned)
- `G-Admins` (Security group, Assigned, **role-assignable**)
- `G-BreakGlass-Exclude` (Security group, Assigned)
- `DG-Windows-Intune-Managed` (Security group, **Dynamic device**)

<img width="1000" alt="image" src="https://github.com/chrisburg-cyber-intense/entra-intune-zero-trust-lab/blob/e29fa689e4fc1fa303b6133090e3467f4088e2f9/screenshots/Step%201.2.png">

<img width="1000" alt="image" src="https://github.com/chrisburg-cyber-intense/entra-intune-zero-trust-lab/blob/e29fa689e4fc1fa303b6133090e3467f4088e2f9/screenshots/Step%201.3.png">

Ensure this is selected for the G-Admin group ("Azure AD roles can be assigned to the group"). Use role-assignable groups for admin access so roles are granted through group membership rather than directly to users. It simplifies auditing, supports least privilege, and integrates cleanly with PIM.

<img width="1000" alt="image" src="https://github.com/chrisburg-cyber-intense/entra-intune-zero-trust-lab/blob/e29fa689e4fc1fa303b6133090e3467f4088e2f9/screenshots/Step%201.4.png">

<img width="1000" alt="image" src="https://github.com/chrisburg-cyber-intense/entra-intune-zero-trust-lab/blob/e29fa689e4fc1fa303b6133090e3467f4088e2f9/screenshots/Step%201.5.png">

This should be created in Entra.

---

## Step 3: Intune Enrollment Configuration

This phase configures Intune so users in scope automatically enroll devices. The goal is “hands-off” enrollment aligned with enterprise operations.

**Configuration:**
- Intune confirmed as MDM authority
- Automatic enrollment enabled for `G-Users-Standard` and `G-Admins`

**Deliverables:**
- Screenshot of automatic enrollment configuration

---

## Step 4: Device Enrollment and Validation

A Windows 11 VM is enrolled to simulate a repeatable enterprise endpoint. Enrollment is validated in both Intune and Entra ID.

**Validation Checks:**
- Device appears in Intune Devices
- Device appears in Entra Devices
- Device is a member of `DG-Windows-Intune-Managed`

**Deliverables:**
- Screenshot: device record in Intune
- Screenshot: device record in Entra ID
- Screenshot: group membership confirmation

---

## Step 5: Intune Compliance Policy (The Gate)

A compliance policy is implemented to establish a measurable “trusted device” standard. This compliance status later becomes a Conditional Access requirement.

**Compliance Requirements:**
- BitLocker required
- Secure Boot required
- TPM required
- Minimum OS version: `10.0.22000.0`

**Deliverables:**
- Screenshot: compliance policy settings
- Screenshot: device compliance report (Compliant/Noncompliant)

[Compliance Policy Notes](policies/Intune-Compliance-Policy.md)

---

## Step 6: Security Baseline Configuration (Hardening)

This phase enforces endpoint hardening controls that reduce attack surface and improve resilience.

### Step 6A: BitLocker Policy
- Enforces encryption on OS drive
- Stores recovery keys in Entra ID

[BitLocker Policy Notes](policies/Intune-BitLocker-Policy.md)

### Step 6B: Defender Attack Surface Reduction (ASR)
A small, high-value set of ASR rules is enabled to disrupt common enterprise attack chains (phishing → macro abuse → payload execution → credential theft).

[ASR Policy Notes](policies/Intune-ASR-Policy.md)

### Step 6C: Local Admin Password Management (Windows LAPS)
Windows LAPS is configured to automatically manage the local Administrator account and escrow credentials securely in Entra ID, reducing lateral movement risk from shared local admin passwords.

[LAPS Policy Notes](policies/Intune-LAPS-Policy.md)

**Deliverables:**
- Screenshots of each policy
- Screenshot of BitLocker recovery key / LAPS password visible in Entra device record

---

## Step 7: Conditional Access Policies (The Payoff)

Conditional Access is implemented to enforce MFA and require compliant devices for access to sensitive applications. Break-glass exclusion is used to prevent administrative lockout.

### Policy 1: Require MFA (All Users)
- Applies to all users
- Excludes break-glass group
- Requires MFA

[CA Policy 1 Notes](policies/CA-Require-MFA.md)

### Policy 2: Require Compliant Device (Office 365)
- Applies to standard + admin users
- Excludes break-glass group
- Requires device to be marked compliant

[CA Policy 2 Notes](policies/CA-Require-Compliant-Device.md)

### Policy 3: Admin Strict Policy
- Applies to admin group
- Requires MFA + compliant device (and stronger auth if available)

[CA Policy 3 Notes](policies/CA-Admins-Strict.md)

**Deliverables:**
- Screenshots of each CA policy
- Sign-in logs showing policies evaluated and enforced

---

## Step 8: Before/After Demo Scenario (Blocked vs Allowed)

This phase demonstrates the business value of the architecture:

**Demo A: Noncompliant Device Blocked**
- Device is forced Noncompliant (BitLocker disabled or compliance requirement changed)
- Attempt access to `portal.office.com`
- Access is blocked due to CA policy requiring compliance

**Demo B: Compliant Device Allowed**
- Device remediated and becomes Compliant
- Same sign-in attempt succeeds
- Entra sign-in logs show CA success

**Deliverables:**
- Screenshot: blocked sign-in screen
- Screenshot: sign-in logs showing CA failure reason
- Screenshot: successful sign-in
- Screenshot: sign-in logs showing CA success

---

## Evidence: Sign-In Logs and Compliance Reporting

This section consolidates proof of enforcement and auditability:
- Conditional Access evaluation results
- Compliance state reporting
- Device encryption and LAPS escrow evidence

[Sign-In Log Evidence](evidence/signin-logs.md)  
[Device Compliance Evidence](evidence/device-compliance.md)

---

## Final Summary: What This Lab Demonstrates

This lab demonstrates a senior-level approach to enterprise security engineering:
- Identity-driven Zero Trust access control (Entra ID + Conditional Access)
- Device trust enforcement (Intune compliance → CA decisions)
- Endpoint hardening (BitLocker + ASR + LAPS)
- Operational auditability (sign-in logs, compliance reporting)
- Lockout-safe design using break-glass exclusions
