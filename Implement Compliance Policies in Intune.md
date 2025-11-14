# Implement Compliance Policies in Intune
## 1. Compliance Policies
Compliance Policies define the minimum security requirements a device must meet to be considered *compliant*.  
Intune evaluates devices based on these rules, and Entra ID can enforce access using Conditional Access.

**Purpose:**
- Protect corporate data  
- Ensure devices meet security standards  
- Block or limit access for risky devices  

---

## 2. What a Compliance Policy Consists Of
A compliance policy includes the following components:

### **Device Health**
- Jailbreak/root detection
- Secure Boot
- Code integrity
- OS health

### **Security Settings**
- Password/PIN requirements
- Encryption rules (BitLocker/FileVault)
- Antivirus/antispyware
- Threat-level evaluation (Defender for Endpoint)

### **System Settings**
- Minimum/maximum OS version
- Real-time protection
- TPM requirement

### **Custom Compliance**
- JSON + script-based custom rules

### **Actions for Non-Compliance**
- Immediate or delayed non-compliance
- Email notifications
- Push alerts
- Used with Conditional Access to block access

---

## 3. Supported Platforms
- Windows 10/11  
- macOS  
- iOS/iPadOS  
- Android (Fully Managed / Work Profile / Dedicated)  
- Linux (limited support)  
- Windows Holographic / Surface Hub (limited scenarios)

---

## 4. Default Compliance Policy
- Built-in policy that applies when no custom compliance policy is assigned.
- Cannot be modified (settings are fixed).
- Admins can only modify the **action** taken for non-compliance.
- Checks basic device health like jailbreak/root detection.

---

## 5. Available Compliance Settings

### **Device Health**
- Jailbroken/rooted detection  
- Secure boot enabled  
- Code integrity  
- OS vers
