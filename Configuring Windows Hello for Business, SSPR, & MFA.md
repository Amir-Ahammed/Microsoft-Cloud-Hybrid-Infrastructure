# Configuring Windows Hello for Business, SSPR, & MFA

## What is Windows Hello for Business?
Windows Hello for Business is a passwordless authentication method that replaces traditional passwords with:
- Biometrics (Fingerprint, Face ID)
- PIN tied to the device’s TPM (Trusted Platform Module)

**It provides strong two-factor authentication using:**
1. Something you have → your device
2. Something you are or know → biometric or PIN

## What Windows Hello Enables
- Passwordless sign-in to Windows 10/11 devices
- Secure login to Microsoft 365, Azure AD, and on-prem AD (Hybrid)
- Fast login experience
- Protection against phishing (no passwords transmitted)
- TPM-based credential protection

## Configure Windows Hello in Intune

1. **Navigate to**: `Intune → Endpoint security → Account protection → Create Policy`
2. **Choose:**
   * **Platform:** Windows 10/11
   * **Profile:** Identity Protection
3. **Configure settings:**
   * **Device Guard**
     * Credential Guard
       * Not configured
       * Disabled (Default)
       * Enabled with UEFI Lock ✔️
       * Enabled without UEFI Lock
   * **Windows Hello For Business**
     * Facial Features Use Enhanced Anti-Spoofing (true ✔️ / false / Not configured)
   * **Device-scoped settings**
     * Enable PIN Recovery
     * Expiration
     * PIN History
     * Lowercase Letters
     * Maximum PIN Length
     * Minimum PIN Length
     * Special Characters
     * Uppercase Letters
     * Require Security Device
     * Use Certificate for On-Prem Auth
     * Use Windows Hello for Business (Device)
   * **User-scoped settings**
     * Enable PIN Recovery (User)
     * Expiration (User)
     * PIN History (User)
     * Lowercase Letters (User)
     * Maximum PIN Length (User)
     * Minimum PIN Length (User)
     * Special Characters (User)
     * Uppercase Letters (User)
     * Require Security Device (User)
     * Use Windows Hello For Business (User)
4. **Assign the profile to users or devices**
* Specific **users** or **device groups**
* Entire organization, depending on deployment scope

> ### Note:
> - **Credential Guard**: Prevents attackers from using tools like Mimikatz to steal passwords from memory even if they get admin access to the device.
> - **Facial Features Use Enhanced Anti-Spoofing**: Stops someone from unlocking a device by holding up a photo or video of the user's face during Windows Hello sign-in.

---
