# Implement Compliance Policies in Intune

## 1. Compliance Policies
Compliance Policies define the minimum security requirements a device must meet to be considered *compliant*.  <br>
**Intune evaluates devices based on these rules, and Entra ID can enforce access using Conditional Access.**

**Purpose:**
- Secure corporate resources  
- Ensure devices meet security standards  
- Block or limit access for risky/non-compliant devices

---

## 3. Supported Platforms
- Windows 10/11  
- macOS  
- iOS/iPadOS  
- Android (Fully Managed / Work Profile / Dedicated)  
- Linux (limited support)  
- Windows Holographic / Surface Hub (limited scenarios)

## 2. What a Compliance Policy Consists Of
A compliance policy includes the following components:

---

### Compliance Settings

**1. Device Health Requirements**
- OS version, jailbreak/rooting detection, TPM, secure boot, etc.
**2. Security Requirements**
- Password/PIN, encryption, firewall, antivirus, threat level, etc.
**3. System Requirements**
- OS updates, disk encryption, configuration levels
**4. Custom Compliance**
- JSON + PowerShell script-based custom rules

### Actions for Non-Compliance
These define what Intune should do when a device does not meet compliance rules.

**Common actions:**
- Mark device as Non-Compliant immediately
- Mark as Non-Compliant after a grace period (e.g., 3 days)
- Send email notification to user
- Send push notification (Can send alerts to Company Portal app on devices)
- Block access via **Conditional Access**

> These actions help push users to fix issues before access is blocked.

### Assignment
This determines who the policy applies to.

**Targets**
- Azure AD groups (users or devices)
- Dynamic groups
**Optional Assignment Filters**
- Include/exclude based on OS, device ownership, etc.
**Scope Tags**
- Used for delegated administration
- Controls which IT team can manage the policy

> Assignment ensures the policy applies only to the correct devices/users.

---

## Notification Settings
Notification Settings control how Intune alerts users when their device becomes non-compliant. These help ensure users take action before access is blocked.

**Name**
- Each notification configuration has a unique Name.
- Helps identify different templates (e.g., Windows Users Notification, BYOD Notification).

**Header and Footer Settings**
- Customize the header of the email:
  - Organization logo
  - Organization name
  - Organization contact details
- Customize the footer:
  - Support contact
  - IT department signature
  - Legal/compliance message
- Ensures all notifications follow company branding.

**Notification Message Templates**
Define the **email body** that users will receive.

- Includes:
  - Explanation of why the device is non-compliant
  - Steps the user must take to fix compliance issues
  - Optional links to Company Portal
  - IT support contact information
- Can be reused across multiple compliance policies.
- Supports multiple templates for different user groups or device types.

---

## 4. Default Compliance Policy
Intune includes a built-in default compliance policy:
- Applies when a device has no compliance policy assigned
- Evaluates basic things such as:
  - Device is not jailbroken/rooted
  - Intune can retrieve device health data
- You can change the default action but not modify the built-in settings.

> Admins usually create their own policies instead of relying on this one.


---
