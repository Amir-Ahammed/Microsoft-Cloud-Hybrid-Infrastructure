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

### Microsoft Attestation Service Evaluation Settings. 
Use these settings to confirm that a device has protective measures enabled at boot time

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

> Enabling these ensures only secure and trusted devices are allowed access to company resources.

---

</details>
<details><summary><b>System Requirements</b>: OS updates, disk encryption, configuration levels</summary>

### Device Properties
Controls compliance based on the device's operating system version.  
Useful for enforcing minimum OS baselines, blocking outdated or vulnerable versions, or ensuring devices stay within supported OS ranges.

<b>Operating System Version</b>

**1. Minimum OS version** <br>
Specifies the lowest OS version allowed for compliance.  
If a device runs an older version, it becomes **non-compliant**.  
> **Example:** Require at least Windows 11 22H2 for security baselines.

**2. Maximum OS version** <br>
Specifies the highest OS version allowed.  
Useful when an organization has not yet approved a new OS release.  
> **Example:** Block Windows 11 24H2 until app testing is completed.

**3. Minimum OS version for mobile devices** <br>
Same as above but applies **only to mobile platforms** (iOS/iPadOS/Android).  
> **Example:** Require iOS 16.0 or later for device compatibility and security.

**4. Maximum OS version for mobile devices** <br>
Sets the highest approved version for mobile devices.  
> **Example:** Block iOS beta releases (e.g., iOS 18 Public Beta).

**5. Valid Operating System Builds** <br>
Allows you to define **specific OS build numbers** that are considered compliant.  
This is more granular than version number (major.minor), allowing control down to build and patch level.

You can enter up to **three** valid builds. & Exports the current OS build list for audit or documentation.

> **Example Use Cases:**
> - Allow only Windows 11 build **22621**, **22631**, and **26100**  
> - Useful for strict environments where patch validation is required  
> - Helps the organization run only tested and stable builds

---

</details>
<details><summary><b>Configuration Manager Compliance</b>: Compliance data from <b>Configuration Manager</b> (co-managed devices)</summary>

### Configuration Manager Compliance
This setting is used when your organization uses **Microsoft Configuration Manager (SCCM)** together with **Intune** in a co-management or tenant-attach scenario.  
It allows you to require compliance results coming directly from SCCM rather than relying only on Intune’s compliance engine.

**Require device compliance from Configuration Manager**
- **Require** → The device must also be compliant in **SCCM (Configuration Manager)**. If SCCM says the device is not compliant, Intune will also mark it as not compliant.
- **Not Configured** → Intune ignores SCCM.  

>> #### **What this means in practice**
> - Devices report hardware/software inventory to SCCM.
> - SCCM evaluates compliance based on its own baselines (e.g., antivirus status, patch level, configuration baselines).
> - Intune will *only* trust SCCM’s compliance result.
> - If SCCM reports **Non-compliant**, Intune will mark the device **Non-compliant**, even if Intune settings are fully met.

---

</details>
<details><summary><b>Security Requirements</b>: Password/PIN, encryption, firewall, antivirus, threat level, etc.</summary>

### Password Settings

- **Require a password to unlock mobile devices**
  - **Require** → Device must use a password to unlock.
  - **Not Configured** → No password requirement enforced.

- **Simple passwords**
  - **Block** → User cannot use easy passwords (1234, 1111).
  - **Not Configured** → Simple passwords are allowed.

- **Password type**
  - **Device default** → Uses the OS’s default password rules.

- **Minimum password length**
  - Sets the minimum number of characters (e.g., 4, 6, 8).

- **Maximum minutes of inactivity before password is required**
  - Forces password entry after the device is idle for X minutes.

- **Password expiration (days)**
  - Forces users to change their password every X days.

- **Number of previous passwords to prevent reuse**
  - Prevents reusing old passwords (e.g., last 5).

- **Require password when device returns from idle state**
  - **Require** → Password needed after screen turns off.
  - **Not Configured** → Device follows its own default behavior.

### Encryption

- **Require encryption of data storage on device**
  - **Require** → Device must use encryption (BitLocker for Windows).  
  - **Not Configured** → Intune will not check encryption.

### Device Security

- **Firewall**
  - **Require** → The device’s firewall must be ON.
  - **Not Configured** → Intune does not check firewall status.

- **Trusted Platform Module (TPM)**
  - **Require** → Device must have TPM for security features like BitLocker.
  - **Not Configured** → TPM is not checked.

- **Antivirus**
  - **Require** → An antivirus solution must be active on the device.
  - **Not Configured** → Antivirus status is not checked.

- **Antispyware**
  - **Require** → Antispyware must be enabled.
  - **Not Configured** → Intune ignores antispyware state.

### Defender

- **Microsoft Defender Antimalware**
  - **Require** → Defender AV must be enabled and running.
  - **Not Configured** → Defender AV state not checked.

- **Microsoft Defender Antimalware minimum version**
  - Ensures the device runs at least the required Defender version.

- **Microsoft Defender Antimalware security intelligence up-to-date**
  - **Require** → Virus definitions must be updated.
  - **Not Configured** → Does not check signature updates.

- **Real-time protection**
  - **Require** → Real-time scanning must stay ON.
  - **Not Configured** → No enforcement on real-time protection.

---

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
