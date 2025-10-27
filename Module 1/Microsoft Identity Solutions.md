# Microsoft Identity Solutions

## 🔐 Identity Fundamentals

- **Authentication**: Verifies identity (e.g., username + password).
- **Authorization**: Grants access to resources (e.g., open file, change settings).
- **Capabilities**: Management rights (e.g., admin privileges).

---

## 🧑‍💻 Local Identity Management

### 🏠 Windows Local Accounts
- Created during initial setup or manually.
- Stored in the local SAM (Security Account Manager) database.
- Used for access to **only that device**.

### 👥 Local Groups

Local groups are used to assign permissions to multiple users on a single device. They are managed via:

- **Computer Management → Local Users and Groups**
- **PowerShell (`Get-LocalGroup`, `Add-LocalGroupMember`)**

### 📋 Common Local Groups

| Group               | Purpose |
|---------------------|---------|
| **Administrators**      | Full control over the device. |
| **Backup Operators**    | Can back up and restore files. |
| **Event Log Readers**   | Can view system logs. |
| **Guests**              | Limited access to the system. |
| **Users**               | Standard access to use apps and files. |
| **Power Users**         | More privileges than Users, less than Administrators. |
| **Remote Desktop Users**| Can log in remotely using Remote Desktop. |
| **Network Configuration Operators** | Can manage network settings. |
| **Replicator**          | Supports file replication services. |
| **Distributed COM Users** | Can launch and use DCOM objects. |
| **Cryptographic Operators** | Can perform cryptographic operations. |
| **Device Owners**       | Identified as owners of the device. |
| **Access Control Assistance Operators** | Can remotely query access permissions. |

> ⚠️ Always assign users to the **least privileged group** necessary to perform their tasks. This helps maintain system security and stability.

### 🧙 Special Identity Groups

These are predefined identity groups in Windows and Active Directory environments. They are automatically created by the system and used to manage access control and permissions.

| Group | Description |
|-------|-------------|
| **Anonymous Logon** | Represents users who access resources without providing credentials. Common in public or unauthenticated scenarios. |
| **Authenticated Users** | Includes all users who have successfully signed in with valid credentials. More secure than "Everyone." |
| **Creator Owner** | Refers to the user who created a file or folder. Used to assign ownership-based permissions. |
| **Everyone** | Includes all users, including guests and anonymous users. Broadest group—use with caution. |
| **Interactive** | Represents users who are logged in locally to the computer (e.g., keyboard/mouse access). |
| **Network** | Represents users accessing the system remotely over the network (e.g., file shares, remote desktop). |
| **Service** | Represents services running under a service account. Used for background tasks and system services. |
| **System** | Represents the operating system itself. Has full access to all system resources and files. |

---

## 🏢 On-Premises AD Identity Management

Active Directory Domain Services (AD DS) is Microsoft’s on-premises identity and access management system. It provides centralized control over users, groups, devices, and resources.

### 🌳 Key Characteristics

- **Hierarchical and Granular**: AD is structured like a tree with forests, domains, and organizational units (OUs). This allows fine-grained control over resources and policies.
- **Security Based on Groups**: Permissions are assigned to groups rather than individual users. This simplifies access control and improves scalability.
- **Group Policy Administration**: AD uses Group Policy Objects (GPOs) to enforce settings across users and computers (e.g., password policies, desktop restrictions).
- **Kerberos Authentication**: AD uses the Kerberos protocol for secure, ticket-based authentication. It’s fast, secure, and supports single sign-on (SSO).

### 👥 Group Types

| Type | Description | Use Case |
|------|-------------|----------|
| **Security Group** | Used to assign permissions to resources. | File shares, printers, apps. |
| **Distribution Group** | Used for email distribution only. | Exchange mailing lists. |

### 📦 Group Scopes

| Scope | Description | Use Case |
|-------|-------------|----------|
| **Domain Local** | Used within a single domain. | Grant access to local resources. |
| **Global** | Members from same domain, usable across domains. | Department-level access. |
| **Universal** | Members from any domain in forest. | Enterprise-wide access across domains.

### 🔧 Group Management Tools

- **Active Directory Users and Computers (ADUC)**: GUI tool for managing users, groups, and OUs.
- **PowerShell**: Scripted management using cmdlets like `New-ADGroup`, `Add-ADGroupMember`, `Get-ADGroup`.

### 🧩 Group Membership Effects

- **Direct Membership**: User inherits permissions assigned to the group.
- **Nested Groups**: Groups can be added to other groups for scalable access control.
- **Transitive Membership**: Universal groups support nesting across domains.
- **Access Control**: Membership affects access to files, folders, apps, and GPOs.

### 🔐 Authentication Protocol

- **Kerberos**: Default protocol used in AD DS. Provides secure, ticket-based authentication.
- **NTLM**: Legacy protocol, still supported but less secure.

> ⚠️ Best Practice: Use **Global groups for users**, **Domain Local groups for resources**, and **Universal groups for cross-domain access**.

---

## ☁️ Cloud Identity Management (Detailed Overview)

Cloud identity solutions extend identity and access management beyond traditional on-premises environments. These services are designed for scalability, security, and integration with modern apps and infrastructure.

---

### 🔐 Microsoft Entra ID (formerly Azure AD)

Microsoft Entra ID is a **cloud-based identity provider** used to manage users, groups, devices, and access to SaaS applications.

#### ✅ Key Features
- Single Sign-On (SSO) for Microsoft 365, Zoom, Salesforce, etc.
- Supports OAuth2, OpenID Connect, SAML protocols.
- Role-Based Access Control (RBAC) for apps and resources.
- Conditional Access policies for secure authentication.
- Identity Governance: Access reviews, entitlement management, lifecycle workflows.

#### 👤 User Management
- Create users manually or sync from on-prem AD via Azure AD Connect.
- Assign licenses (e.g., Microsoft 365, Intune).
- Configure user attributes, roles, and MFA settings.
- Delegate access using **RBAC** and **PIM**.

#### 👥 Group Management
- **Security Groups**: Used to assign access to resources (e.g., SharePoint, Teams).
- **Microsoft 365 Groups**: Used for collaboration (email, Teams, Planner).
- **Dynamic Groups**: Automatically add/remove users based on attributes (e.g., department = Sales).
- Groups can be used for Conditional Access, app assignments, and policy targeting.

#### 🔧 Management Tools
- Microsoft Entra Admin Center
- PowerShell (`New-AzureADGroup`, `Add-AzureADGroupMember`)
- Microsoft Graph API

---

### 🏢 Microsoft Entra Domain Services

Entra Domain Services provides **managed domain services** like domain join, LDAP, Kerberos, and Group Policy—**without deploying domain controllers**.

#### ✅ Key Features
- Domain join for Azure VMs.
- LDAP and Kerberos authentication.
- Group Policy support in cloud environments.
- No need to patch or manage domain controllers.

#### 🧰 Use Cases
- Lift-and-shift legacy apps to Azure.
- Extend AD-like capabilities to cloud workloads.
- Support hybrid identity scenarios without full AD infrastructure.

---

### 🧱 Azure IaaS Windows Server VMs with AD DS

This approach involves deploying **Windows Server virtual machines** in Azure and manually installing **Active Directory Domain Services (AD DS)**.

#### ✅ Key Features
- Full control over domain controllers.
- Supports traditional AD features: forests, trusts, OUs, GPOs.
- Can be integrated with on-prem AD via VPN or ExpressRoute.

#### 🧰 Use Cases
- Custom AD configurations in the cloud.
- Hosting AD-dependent applications.
- Hybrid environments needing full AD control.

---

### 🔍 Comparison Summary

| Feature | Entra ID | Entra Domain Services | Azure IaaS AD DS |
|--------|----------|------------------------|------------------|
| Type | Cloud-native identity | Managed legacy AD | Self-managed AD |
| Domain Join | ❌ | ✅ | ✅ |
| LDAP/Kerberos | ❌ | ✅ | ✅ |
| Group Policy | ❌ | ✅ | ✅ |
| Admin Control | High-level (RBAC) | Limited (managed) | Full (manual) |
| Use Case | SaaS access, cloud apps | Legacy app support | Full AD in cloud |



---



## ☁️ Azure IaaS AD DS

- Deploy **Windows Server VMs** in Azure.
- Install and configure **Active Directory Domain Services**.
- Offers full control over domain controllers in the cloud.
- Ideal for lift-and-shift scenarios or hybrid environments.

---

## 🔄 Identity Synchronization

- Use **Azure AD Connect** to sync on-prem AD with Entra ID.
- Enables hybrid identity: users can access both on-prem and cloud resources.
- Supports password hash sync, pass-through authentication, and federation.

---

## 🛡️ Role-Based Access Control (RBAC)

- Assign roles based on job function.
- Roles define what actions users can perform.

| Role | Permissions |
|------|-------------|
| Reader | View-only access |
| Contributor | Create/edit resources |
| Owner/Admin | Full control |

Used in:
- Microsoft Entra ID
- Microsoft 365 Admin Center
- Azure Resource Manager

---

## 🆚 Admin Center Comparison

| Feature | Microsoft 365 Admin Center | Entra Admin Center | Intune Admin Center |
|---------|----------------------------|---------------------|----------------------|
| **Interface** | Simplified | Advanced | Policy-driven |
| **User Management** | Basic | Granular (RBAC, PIM) | Limited (focuses on devices) |
| **Group Types** | Microsoft 365 Groups, DLs | Security, Dynamic Groups | Uses Entra groups for targeting |
| **Role Assignment** | Limited | Full RBAC & PIM | Scoped roles for device admins |
| **Governance** | Minimal | Full lifecycle & access reviews | Compliance policies, conditional access |
| **Focus Area** | Collaboration & licensing | Identity, access, governance | Device, app, and endpoint security |
| **Key Capabilities** | License assignment, mailbox setup | Identity lifecycle, MFA, SSO | Device enrollment, app deployment, compliance enforcement |

---

## ☁️ Azure in Identity and Access Management

Azure is Microsoft’s cloud platform that hosts infrastructure, applications, and identity services. It integrates deeply with Entra ID and Intune to provide secure, scalable, and flexible identity solutions.

### 🔐 Key Identity Features in Azure

| Feature | Description |
|--------|-------------|
| **Azure Active Directory (Entra ID)** | Cloud-based identity provider for apps, users, and devices. |
| **Azure AD Connect** | Syncs on-prem AD with Entra ID for hybrid identity. |
| **Azure RBAC** | Role-based access control for managing Azure resources. |
| **Azure Policy** | Enforces governance rules across subscriptions. |
| **Privileged Identity Management (PIM)** | Just-in-time access for sensitive roles. |
| **Managed Identities** | Automatically managed credentials for Azure services. |
| **Conditional Access** | Enforces access policies based on user/device conditions. |

### 🧱 Azure IaaS (Infrastructure as a Service)

- Deploy **Windows Server VMs** and install **AD DS** for full control.
- Use **Azure Files**, **Azure Storage**, and **Azure Networking** with identity-based access.
- Integrate with **Entra Domain Services** for legacy support.

> Azure enables hybrid and cloud-native identity models, making it ideal for enterprises transitioning from on-prem to cloud.

---

## 🧠 Best Practices

- Use **RBAC** to avoid over-permissioning.
- Sync **on-prem AD** with **Entra ID** for hybrid identity.
- Use **Entra Domain Services** for legacy app support in cloud.
- Regularly audit **group memberships** and **access rights**.
- Avoid assigning permissions directly to users—use groups.
- Use **Privileged Identity Management (PIM)** for just-in-time admin access.

---
