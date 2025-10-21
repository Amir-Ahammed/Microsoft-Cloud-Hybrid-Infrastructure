# 🖥️ Windows Client Deployment using Microsoft Intune

## 📘 Overview
Microsoft Intune is a cloud-based endpoint management solution that helps organizations manage and secure Windows devices. It enables automated deployment, configuration, and compliance enforcement for Windows clients across hybrid or fully cloud environments.

---
<details> 
  <summary>🚀 Intune Device Lifecycle Workflow</summary>
This guide outlines the complete process for onboarding and managing Windows devices using Microsoft Intune and Windows Autopilot.

---

### 🧾 Step 1: Register

> Add devices to your Autopilot tenant so they can be identified and provisioned.

- Vendor or IT admin uploads **hardware hashes**.
- Devices appear in **Intune > Windows Autopilot devices**.
- Assign Autopilot profiles and group tags.

---

### ⚙️ Step 2: Deploy

> Prepare the device with OS, apps, and policies before the user starts using it.

- User unboxes the laptop and connects to Wi-Fi.
- Autopilot applies:
  - Azure AD Join or Hybrid Join
  - Apps, policies, and branding
- Device is provisioned automatically.

---

### 🔐 Step 3: Enroll

> Connect the device to Intune so it can be managed.

- Device **auto-enrolls** into Intune during deployment.
- Appears in **Intune > Devices**.
- Starts receiving compliance policies, config profiles, and apps.

---

### 🛡️ Step 4: Manage

> Use Intune to monitor, secure, and control the device throughout its lifecycle.

- Push updates, apps, and security settings.
- Monitor compliance, health, and inventory.
- Perform remote actions:
  - Wipe
  - Retire
  - Restart
  - Remote assist

---

</details>

<details> <summary>🧩 Autopilot registration process</summary>

Your company uses Microsoft Intune to manage Windows devices. After purchasing laptops, you can register them in Intune using **Windows Autopilot**. There are two trusted paths: **Vendor Upload** and **IT Admin Upload**.

---

### ✅ 1. Vendor Upload (OEM or Reseller)

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

### 🛠 2. IT Admin Upload (Manual or Scripted)

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

# 📘 Deployment & Enrollment in Intune

Understanding the difference between **deployment** and **enrollment** is essential for managing Windows devices with Microsoft Intune and Autopilot.

---

## 🚀 What Is Deployment?

> Deployment is the process of preparing and configuring a device before the user starts using it.

### 🔧 Deployment Includes:
- Applying **Autopilot profiles**
- Installing **apps and policies**
- Configuring **settings** (language, region, privacy)
- Branding the device (e.g., company logo, name)
- Joining the device to **Azure AD** or **Hybrid AD**

### 🧭 When It Happens:
- During **Out-of-Box Experience (OOBE)**
- Or via **Pre-provisioning** (White Glove)
<details>
  <summary>Out-of-Box Experience (OOBE)</summary>

- The device boots into a guided setup screen.
- The user selects:
  - Language
  - Region
  - Keyboard layout
- The device connects to Wi-Fi or Ethernet.
- If the device is registered with Windows Autopilot, it contacts the Autopilot service and:
  - Downloads its assigned deployment profile
  - Applies company branding and settings
  - Prompts the user to sign in with their Azure AD credentials
  - Automatically joins Azure AD and enrolls into Intune

</details>

<details>
  <summary>Or via Pre-provisioning (White Glove)</summary>

  Pre-provisioning allows IT to fully configure a Windows Autopilot device **before handing it to the end user**. It speeds up deployment by applying apps, policies, and settings in advance.

- IT boots the device into Out-of-Box Experience (OOBE)
- Connects to Wi-Fi or Ethernet
- Presses **Windows key + 5** to start pre-provisioning
- The device contacts the Autopilot service and:
  - Downloads its assigned deployment profile
  - Applies company branding and configuration
  - Installs required apps and policies
  - Validates setup and shows a green screen if successful
- IT shuts down the device
- End user powers on the device and sees:
  - Company branding
  - Sign-in screen
  - Fully configured environment

</details>

### 🧩 Deployment Methods:
- **User-driven Autopilot**
- **Self-deploying Autopilot**
- **Pre-provisioned Autopilot**
- **Provisioning Package (PPKG)**

---

## 🔐 What Is Enrollment?

> Enrollment is the process of connecting the device to Intune so it can be managed.

### 🔧 Enrollment Enables:
- Remote management via Intune
- Delivery of compliance policies and updates
- Monitoring and reporting
- Remote actions (wipe, retire, restart)

### 🧭 When It Happens:
- Automatically during Autopilot deployment
- Manually via Company Portal or PPKG
- Via GPO for Hybrid AD Join
- Through SCCM for co-managed devices

### 🧩 Enrollment Methods:
- **Autopilot Enrollment**
- **Azure AD Join + Auto-enroll**
- **Hybrid Azure AD Join + GPO**
- **Company Portal App**
- **Provisioning Package (PPKG)**
- **Co-management (SCCM + Intune)**

---

## 🔁 Summary Flow

```text
1. Register → Upload device to Autopilot
2. Deploy   → Configure device setup (OOBE, apps, policies)
3. Enroll   → Connect device to Intune for management
4. Manage   → Monitor, secure, and support the device






















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

# 🧩 Intune Device Lifecycle: Full Scenario-Based Breakdown

This guide maps out the full lifecycle of Windows device management using Microsoft Intune and Autopilot — from registration to deployment, enrollment, and beyond.

---

## 🔁 Phase 1: Register Devices

> Devices must be registered in Autopilot before any deployment or enrollment can begin.

### ✅ Methods:
- **Vendor Upload**: OEM/reseller uploads hardware hashes to your tenant.
- **IT Admin Upload**: Use PowerShell to extract and upload hardware hashes manually.

### 🔍 Result:
- Devices appear in **Intune > Windows Autopilot devices**
- Ready for profile assignment and deployment

---

## 🚀 Phase 2: Deploy Devices (Setup Process)

> Deployment = How the device is prepared and configured before use.

### ✅ Deployment Methods (Choose Based on Scenario):

| Method               | Scenario                                | Description |
|----------------------|------------------------------------------|-------------|
| **User-Driven Autopilot** | Employee laptops                      | User signs in during OOBE; apps/policies applied |
| **Self-Deploying Autopilot** | Kiosks, shared devices             | No user input; setup completes automatically |
| **Pre-Provisioned Autopilot** | IT preps device before handoff     | Faster setup; apps/policies preloaded |
| **Provisioning Package (PPKG)** | Offline or bulk setup             | Admin uses USB/config file to deploy settings |
| **Manual Setup + Company Portal** | BYOD or unmanaged devices         | User installs Company Portal and enrolls manually |

---

## 🔐 Phase 3: Enroll Devices (Connect to Intune)

> Enrollment = How the device becomes manageable by Intune.

### ✅ Enrollment Methods (Triggered During or After Deployment):

| Method                                | Scenario                                | Trigger Type           |
|--------------------------------------|------------------------------------------|------------------------|
| **Autopilot Enrollment**             | New corporate devices                    | Automatic during OOBE |
| **Azure AD Join + Auto-enroll**      | Cloud-first orgs                         | Automatic at sign-in  |
| **Hybrid Azure AD Join + GPO**       | On-prem AD with Intune                   | Automatic via GPO     |
| **Company Portal App**               | BYOD or manual enrollment                | User-initiated        |
| **Provisioning Package (PPKG)**      | Offline bulk setup                       | Admin-initiated       |
| **Co-management (SCCM + Intune)**    | Existing SCCM-managed devices            | Admin-initiated       |

---

## 🛡️ Phase 4: Post-Enrollment Configuration

> Once enrolled, devices receive policies, apps, and compliance settings.

### ✅ Actions:
- Assign **Configuration Profiles** (Wi-Fi, VPN, BitLocker, Defender)
- Deploy **Applications** (Win32, Store, LOB)
- Apply **Compliance Policies** (password, encryption, antivirus)
- Monitor via **Intune > Devices**
- Use **Endpoint Analytics** for performance insights
- Perform remote actions: Wipe, Retire, Restart, Rename

---

## 🧠 Real-World Example Matrix

| Device Type         | Deployment Method         | Enrollment Method             |
|---------------------|---------------------------|--------------------------------|
| Employee laptop     | User-Driven Autopilot     | Autopilot + Azure AD Join     |
| Retail kiosk        | Self-Deploying Autopilot  | Autopilot + Azure AD Join     |
| School lab PC       | Provisioning Package      | PPKG + Azure AD Join          |
| BYOD tablet         | Manual + Company Portal   | Company Portal App            |
| Legacy desktop      | Manual setup              | Hybrid Join + GPO             |
| SCCM-managed laptop | SCCM                      | Co-management                 |

---

> ✅ You can mix and match deployment + enrollment methods based on your environment, device type, and user needs. Intune supports hybrid, cloud-native, and manual workflows — all in parallel.
# 📘 Intune Device Lifecycle: Full Configuration by Method

This guide outlines every supported **deployment** and **enrollment** method in Microsoft Intune, with step-by-step configuration instructions for each.

---

## 🔁 Phase 1: Register Devices

> Devices must be registered in Autopilot before deployment or enrollment.

### ✅ Configuration Steps:
- Upload hardware hashes via:
  - Vendor (OEM/reseller)
  - IT Admin (PowerShell script)
- Go to **Intune > Devices > Windows Autopilot devices**
- Confirm:
  - Serial number
  - Hardware hash
  - Profile assignment status

---

## 🚀 Phase 2: Deployment Methods

> Deployment = How the device is set up before use.

### 1️⃣ User-Driven Autopilot

| Use Case | Employee laptops |
|----------|------------------|

#### Configuration:
- Go to **Windows enrollment > Deployment profiles**
- Create profile:
  - Join type: Azure AD Join or Hybrid
  - Deployment mode: User-driven
  - Skip privacy settings ✅
  - Set user account type (Standard/Admin)
- Assign profile to device or dynamic group
- Ensure MDM auto-enrollment is enabled in Azure AD

---

### 2️⃣ Self-Deploying Autopilot

| Use Case | Kiosks, shared devices |
|----------|------------------------|

#### Configuration:
- Same steps as User-Driven, but:
  - Deployment mode: Self-deploying
  - No user sign-in required
  - Requires TPM 2.0 and Ethernet
- Assign profile and confirm device readiness

---

### 3️⃣ Pre-Provisioned Autopilot

| Use Case | IT preps device before handoff |
|----------|-------------------------------|

#### Configuration:
- Create profile with:
  - Deployment mode: Pre-provisioned
  - Join type: Azure AD Join
- Boot device into OOBE
- Press **Windows key + 5** to start pre-provisioning
- IT completes setup; user signs in later

---

### 4️⃣ Provisioning Package (PPKG)

| Use Case | Offline or bulk setup |
|----------|-----------------------|

#### Configuration:
- Install **Windows Configuration Designer**
- Create provisioning package:
  - Join Azure AD
  - Enroll in Intune
  - Add Wi-Fi, apps, policies
- Save to USB
- Boot device and apply PPKG during OOBE

---

### 5️⃣ Manual Setup + Company Portal

| Use Case | BYOD or unmanaged devices |
|----------|---------------------------|

#### Configuration:
- User installs **Company Portal App**
- Signs in with work account
- Follows prompts to enroll
- Device appears in Intune for management

---

## 🔐 Phase 3: Enrollment Methods

> Enrollment = How the device connects to Intune.

### 1️⃣ Autopilot Enrollment

| Trigger | During OOBE |
|---------|-------------|

#### Configuration:
- Ensure Autopilot profile is assigned
- MDM auto-enrollment enabled in Azure AD
- User signs in → device auto-enrolls

---

### 2️⃣ Azure AD Join + Auto-enroll

| Trigger | User sign-in |
|---------|--------------|

#### Configuration:
- Go to **Azure AD > Mobility (MDM and MAM)**
- Set MDM scope to **All**
- Device joins Azure AD → auto-enrolls into Intune

---

### 3️⃣ Hybrid Azure AD Join + GPO

| Trigger | GPO policy |
|---------|------------|

#### Configuration:
- Set up **Azure AD Connect**
- Configure GPO:
  - Enable MDM auto-enrollment
  - Target domain-joined devices
- Devices join on-prem AD → register in Azure AD → enroll in Intune

---

### 4️⃣ Company Portal App

| Trigger | User-initiated |
|---------|----------------|

#### Configuration:
- User installs **Company Portal**
- Signs in with Azure AD account
- Follows enrollment steps
- Device appears in Intune

---

### 5️⃣ Provisioning Package (PPKG)

| Trigger | Admin-initiated |
|---------|-----------------|

#### Configuration:
- PPKG includes enrollment settings
- Applied during OOBE or post-setup
- Device joins Azure AD and enrolls in Intune

---

### 6️⃣ Co-management (SCCM + Intune)

| Trigger | Admin-initiated |
|---------|-----------------|

#### Configuration:
- Enable co-management in **SCCM console**
- Configure workloads to shift to Intune
- Devices remain in SCCM but also enroll in Intune

---

## 🛡️ Phase 4: Post-Enrollment Configuration

> Once enrolled, devices receive policies, apps, and compliance settings.

### ✅ Actions:
- Create and assign:
  - **Configuration Profiles** (Wi-Fi, VPN, BitLocker, Defender)
  - **Compliance Policies** (password, encryption, antivirus)
  - **Applications** (Win32, Store, LOB)
- Monitor via **Intune > Devices**
- Use **Endpoint Analytics** for performance insights
- Perform remote actions: Wipe, Retire, Restart, Rename

---

> ✅ You can mix and match deployment and enrollment methods based on your environment, device type, and user needs. Intune supports hybrid, cloud-native, and manual workflows — all in parallel.
