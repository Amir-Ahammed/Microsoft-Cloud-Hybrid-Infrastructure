# Implement Compliance Policies in Intune

## 1. Compliance Policies
Compliance Policies define the minimum security requirements a device must meet to be considered *compliant*.
**Intune evaluates devices based on these rules, and Entra ID can enforce access using Conditional Access.**

**Purpose:**
- Secure corporate resources  
- Ensure devices meet security standards  
- Block or limit access for risky/non-compliant devices

---

## 2. Supported Platforms
- Windows 10 and later
- Windows 8 and later
- macOS  
- iOS/iPadOS  
- Android Device Administrator
- Android (AOSP)
- Android Enterprise
- Linux (limited support)  
- Windows Holographic / Surface Hub (limited scenarios)

---

## 3. Compliance Policy Consists Of
A compliance policy includes the following components:

### Compliance Settings

<details><summary><b>Device Health Requirements:</b> OS version, jailbreak/rooting detection, TPM, secure boot, etc.</summary>

Microsoft Attestation Service evaluation settings. Use these settings to confirm that a device has protective measures enabled at boot time

<b>Windows 10 and 11</b>

**1. BitLocker**
- **Require** → The device must have BitLocker disk encryption enabled.  
  This protects data at rest and ensures the disk cannot be accessed if stolen.
- **Not Configured** → Intune will not check whether BitLocker is enabled.

**2. Secure Boot**
- **Require** → The device must have Secure Boot turned on.  
  This ensures the device boots only trusted operating system files and prevents boot-level malware.
- **Not Configured** → Intune won’t evaluate Secure Boot status.

**3. Code Integrity**
- **Require** → The device must validate code integrity at boot.  
  This prevents untrusted or modified drivers from loading during startup.
- **Not Configured** → No evaluation will be done for this setting.

### **Why These Settings Matter**
These attestation checks increase device security by verifying protections *before* the OS fully loads.  
They are typically used in:
- Zero trust environments  
- High-security organizations  
- Compliance-driven industries  

> Enabling these ensures only secure and trusted devices are allowed access to company resources.

---

</details>
<details><summary><b>Security Requirements</b>: Password/PIN, encryption, firewall, antivirus, threat level, etc.</summary>

>

</details>
<details><summary><b>System Requirements</b>: OS updates, disk encryption, configuration levels</summary>

>

</details>
<details><summary><b>Custom Compliance</b>: JSON + PowerShell script-based custom rules</summary>

#### Example (PowerShell Script)
This script checks whether Chrome exists on the device and outputs the result in Intune-required format.
```
# Check if Google Chrome is installed
$chromePath = "C:\Program Files\Google\Chrome\Application\chrome.exe"

if (Test-Path $chromePath) {
    # Chrome found — Compliant
    Write-Output "{""ChromeInstalled"": true}"
}
else {
    # Chrome not found — Noncompliant
    Write-Output "{""ChromeInstalled"": false}"
}
```

Example (JSON)
```
{
  "Rules": [
    {
      "SettingName": "ChromeInstalled",
      "Operator": "IsEquals",
      "DataType": "Boolean",
      "Operand": true,
      "DefaultValue": false,
      "Description": "Checks if Google Chrome is installed on the device."
    }
  ]
}

```
</details>

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

### Notification Settings
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

## 4. Built-in Device Compliance Policy
Define how Intune treats devices when no compliance policy is assigned OR when compliance data becomes outdated. <br>
This acts as the **global fallback compliance behavior**.

### Mark Devices with No Compliance Policy Assigned As
- You can choose:
  - **Compliant** (recommended for BYOD or during early rollout)
  - **Not Compliant** (recommended for strict environments)
- This setting ensures unmanaged devices are handled consistently.

> **Example**: If a user never receives a compliance policy, this toggle decides whether their device is allowed access by Conditional Access.

### Compliance Status Validity Period (Days)
- Defines how long a device’s compliance state remains valid without checking in.
- Default: 30 days
- After this period:
  - The device is marked “Not Compliant” until it checks in again.
  - Helps catch devices that have gone missing, offline, or outdated.
 
>> **Why this matters**:
> - Ensures stale devices don’t remain permanently compliant.
> - Forces regular device check-in for accurate compliance reporting.


---
