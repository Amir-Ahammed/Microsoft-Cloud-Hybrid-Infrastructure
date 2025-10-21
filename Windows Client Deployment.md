# 🖥️ Windows Client Deployment using Microsoft Intune

## 📘 Overview
Microsoft Intune is a cloud-based endpoint management solution that helps organizations manage and secure Windows devices. It enables automated deployment, configuration, and compliance enforcement for Windows clients across hybrid or fully cloud environments.

---
<details> <summary>🧩 Autopilot registration process</summary>

Your company uses Microsoft Intune to manage Windows devices. After purchasing laptops, you can register them in Intune using **Windows Autopilot**. There are two trusted paths: **Vendor Upload** and **IT Admin Upload**.

---

## ✅ 1. Vendor Upload (OEM or Reseller)

**Best for:** Bulk purchases from authorized vendors like Dell, HP, Lenovo, etc.

### What You Provide
- **Tenant ID** (GUID format)
- **Tenant Domain** (e.g., `company.onmicrosoft.com`)

### What Vendor Does
- Extract hardware hashes from each device.
- Upload them to your Autopilot tenant via Microsoft’s OEM portal or Partner Center API.
- Optionally assign:
  - `Group Tag` (for profile targeting)
  - `Order ID`
  - `Assigned User`

> 📦 Devices arrive pre-registered and ready for Autopilot provisioning.

---

## 🛠 2. IT Admin Upload (Manual or Scripted)

**Best for:** Internal setup, lab devices, or when vendor upload isn’t available.

### Steps to Collect and Upload Hardware Hash

1. Boot the device into Windows.
2. Run PowerShell to extract the hardware hash:
   ```powershell
   md c:\HWID
   Set-Location c:\HWID
   Install-Script -Name Get-WindowsAutopilotInfo
   Get-WindowsAutopilotInfo -OutputFile AutopilotHWID.csv
</details>

<details>
  <summary>🧭 Intune Enrollment Methods: How a device actually connects to Intune and becomes manageable.</summary>

  So once the device is registered (via vendor or admin), it still needs to enroll into Intune. Microsoft Intune supports multiple ways to register and manage Windows devices, depending on your environment and goals.

  ### 🔐 Enrollment Options

  | Method                        | Best For                          | Trigger Type         |
  |------------------------------|-----------------------------------|----------------------|
  | **Windows Autopilot**        | New corporate devices             | Hardware hash upload |
  | **Azure AD Join + MDM Auto-enrollment** | Cloud-first environments         | Automatic            |
  | **Hybrid Azure AD Join + GPO** | On-prem AD + Intune              | Automatic via GPO    |
  | **Company Portal App**       | BYOD or manual enrollment         | User-initiated       |
  | **Provisioning Package (PPKG)** | Offline or bulk setup             | Admin-initiated      |
  | **Co-management (SCCM + Intune)** | Existing SCCM-managed devices     | Admin-initiated      |

  ### 🧩 Key Scenarios

  - **BYOD (Bring Your Own Device):**  
    Users install the **Company Portal app**, sign in with their work account, and manually enroll.

  - **Hybrid Azure AD Join + GPO:**  
    Devices joined to on-prem AD can be auto-enrolled into Intune using **Group Policy** and **Azure AD Connect**.

  - **Offline Provisioning:**  
    Use **PPKG files** created with Windows Configuration Designer to enroll devices without internet during setup.

  - **Co-management:**  
    Organizations using **SCCM** can enable co-management to gradually shift workloads to Intune.

</details>


## 🚀 Deployment Scenarios
### 1. **Azure AD Join (Cloud-Only)**
- Devices are joined directly to Azure Active Directory.
- Ideal for cloud-first environments.
- Managed entirely via Intune.

<details>
  <summary>💡 Example Scenario: Azure AD Join</summary>

  Imagine you're deploying **20 new laptops** using **Microsoft Intune Autopilot**. Here's how the process works:

  1. Upload hardware hashes into Intune Autopilot.
  2. When users log in for the first time, devices automatically:
     - ✅ Join Azure AD  
     - ✅ Enroll in Intune  
     - ✅ Install company policies and apps  
     - 🎉 Done!

  🔒 No need for on-premises Active Directory (AD) or a Virtual Private Network (VPN).

</details>

### 2. **Hybrid Azure AD Join**
- Devices are joined to on-premises AD and registered in Azure AD.
- Suitable for organizations using both on-prem and cloud resources.
- Requires Azure AD Connect.
<details>
  <summary>🌐 Hybrid Azure AD Join: Full Scenario Overview</summary>

  ### 💡 Real-World Setup

  Your company still has an on-premises Active Directory (AD) with servers, file shares, and Group Policies (GPOs).  
  You're also using Microsoft 365 and Intune for cloud management.

  **Environment Details:**
  - Domain Controller (on-prem): `corp.local`
  - Azure tenant: `company.onmicrosoft.com`
  - Azure AD Connect installed on-premises (syncs users and devices between AD ↔ Azure AD)
  - Devices are joined to the on-prem domain (classic domain join)

  ---

  ### ⚙️ Deployment Workflow

  1. **IT sets up the laptop manually or via imaging**
     - Joins it to the on-prem AD domain (`corp.local`)
     - Example:  
       ```
       Right-click This PC > Properties > Change settings > Domain: corp.local
       ```

  2. **Azure AD Connect syncs the device identity to Azure AD**
     - Azure AD now recognizes the device, though it's still managed by on-prem AD.

  3. **Auto-enrollment into Intune**
     - If GPO or MDM auto-enrollment is configured, the device registers with Intune.
     - Intune begins applying compliance and configuration policies.

  4. **User signs in**
     - Uses on-prem AD credentials (synced to Azure AD)
     - Device becomes a **Hybrid Azure AD Joined** device

  ---

  ### ✅ Final Result

  - ✅ Device is domain-joined (on-prem)
  - ✅ Registered in Azure AD (cloud)
  - ✅ Managed by Intune and possibly SCCM / GPO
  - ✅ User can access:
    - File servers and printers (on-prem)
    - Microsoft 365 and cloud apps (Azure AD)
    - Intune policies and apps (cloud)
</details>

### 3. **Autopilot Deployment**
- Simplifies the Windows device setup process.
- Prepares devices with custom configurations, apps, and policies.
- User-driven or pre-provisioned setups available.

---

## ⚙️ Prerequisites
- Microsoft Intune subscription (part of Microsoft 365 E3/E5 or EMS).
- Azure AD Premium license.
- Windows 10/11 Enterprise or Pro edition.
- Internet connectivity for devices.
- Hardware ID registration for Autopilot (via OEM or script).

---

## 🧩 Deployment Methods
### 1. **Windows Autopilot**
- Zero-touch provisioning for new devices.
- Profiles can include:
  - Device naming convention.
  - Out-of-Box Experience (OOBE) customization.
  - User or device-based enrollment settings.

### 2. **Manual Enrollment**
- Users manually enroll via **Settings → Accounts → Access work or school**.
- Used for BYOD or small-scale deployments.

### 3. **Group Policy Enrollment**
- For domain-joined devices using GPO to auto-enroll into Intune.

---

## 🛠️ Configuration Steps

### Step 1: Prepare Intune
- Create device groups in **Microsoft Endpoint Manager (MEM) Admin Center**.
- Configure **Enrollment restrictions** and **Device categories**.

### Step 2: Create Autopilot Profile
- Navigate to:
  **Devices → Windows → Windows enrollment → Deployment Profiles**
- Define settings like:
  - User account type (Standard/Admin)
  - Skip privacy setup
  - Automatically join Azure AD

### Step 3: Assign Profile to Devices
- Import hardware hash or assign by device group.

### Step 4: Configure Device Policies
- Examples:
  - **Compliance policies** → Require BitLocker, PIN, or password.
  - **Configuration profiles** → Deploy Wi-Fi, VPN, or Endpoint Protection.
  - **Windows Update rings** → Manage OS updates and restart behavior.

### Step 5: Deploy Applications
- Add and assign apps (Win32, MSI, Microsoft Store, or LOB apps).
- Assign to user or device groups.

### Step 6: Test & Monitor
- Use **Endpoint analytics** and **Device compliance reports**.
- Verify deployment success, policy status, and app installation.

---

## 🔐 Security & Compliance
- Enforce **Conditional Access** based on device compliance.
- Apply **BitLocker encryption** and **Defender for Endpoint** integration.
- Monitor device health and security posture in Intune dashboard.

---

## 🧾 Best Practices
- Test deployment profiles with pilot users.
- Use dynamic groups for automated targeting.
- Document Autopilot profiles and version control.
- Monitor using Intune’s **Endpoint Security** and **Reports**.

---

## 📊 Monitoring & Reporting
- **Devices → Monitor** for enrollment status and compliance.
- **Endpoint Analytics** for performance insights.
- Export reports for audit and compliance tracking.

---

## 🧠 Summary
| Area | Key Function |
|------|---------------|
| Enrollment | Azure AD Join / Autopilot / GPO |
| Configuration | Policies & Profiles |
| Application Deployment | Win32, Store, LOB apps |
| Security | BitLocker, Defender, Conditional Access |
| Monitoring | Compliance reports & Endpoint Analytics |

---

## 📚 References
- [Microsoft Intune Documentation](https://learn.microsoft.com/mem/intune/)
- [Windows Autopilot Overview](https://learn.microsoft.com/mem/autopilot/)
- [Azure AD Join Guide](https://learn.microsoft.com/azure/active-directory/devices/concept-azure-ad-join)

