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
- **GPMC**: Group Policy Management Console

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

## ☁️ Cloud Identity Management

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

#### 🛡️ Role-Based Access Control (RBAC)

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

### 🏢 Microsoft Entra Domain Services

Entra Domain Services provides **managed domain services** like domain join, LDAP, Kerberos, and Group Policy—**without deploying domain controllers**.

#### ✅ Key Features
- Domain join for Azure VMs.
- LDAP and Kerberos authentication.
- Group Policy support in cloud environments.
- No need to patch or manage domain controllers.

#### 👤 User Management
- Users are synced from Entra ID (no direct creation in Entra DS).
- Passwords must be synced using **Azure AD password hash sync**.
- Users can log in to domain-joined VMs using their Entra credentials.

#### 👥 Group Management
- Security groups from Entra ID are available in Entra DS.
- Group membership affects access to domain-joined resources.
- Group Policy can be applied to users and groups within the managed domain.

#### 🔧 Tools
- Group Policy Management Console (GPMC) on domain-joined VM
- Entra Admin Center (for upstream group management)

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

#### 👤 User Management
- Create users using **Active Directory Users and Computers (ADUC)**.
- Full control over user attributes, OU placement, and login policies.
- Can sync with Entra ID using **Azure AD Connect**.

#### 👥 Group Management
- Create and manage **Security** and **Distribution** groups.
- Use **Group Scopes**: Domain Local, Global, Universal.
- Apply **Group Policy Objects (GPOs)** to groups for centralized management.

#### 🔧 Tools
- ADUC (GUI)
- PowerShell (`New-ADUser`, `Add-ADGroupMember`)
- GPMC (Group Policy Management Console)

---
## 🆚 Identity Management Models Comparison

| Feature | Local Identity Management | On-Premises AD Identity | Microsoft Entra ID | Entra Domain Services | Azure IaaS AD DS |
|--------|----------------------------|--------------------------|---------------------|------------------------|------------------|
| **Location** | Device-local | On-premises servers | Cloud-native | Cloud-managed | Cloud-hosted VMs |
| **User Storage** | Local SAM database | AD DS database | Entra ID tenant | Synced from Entra ID | AD DS on Azure VMs |
| **Group Types** | Local groups | Security & Distribution | Security, M365, Dynamic | Security (from Entra ID) | Security & Distribution |
| **Group Scopes** | Device-only | Domain Local, Global, Universal | Tenant-wide | Domain Local | Domain Local, Global, Universal |
| **Authentication** | NTLM (local only) | Kerberos, NTLM | OAuth2, OpenID Connect, SAML | Kerberos, LDAP | Kerberos, NTLM |
| **Policy Control** | Manual per device | Group Policy (GPO) | Conditional Access, PIM | Group Policy (GPO) | Group Policy (GPO) |
| **User Creation** | Manual on device | Manual or synced | Manual, synced, API | Synced only | Manual via ADUC |
| **Management Tools** | Computer Management, PowerShell | ADUC, GPMC, PowerShell | Entra Admin Center, Graph API | GPMC on domain-joined VM | ADUC, GPMC, PowerShell |
| **Directory Sync** | None | Optional (to Entra ID) | Native | From Entra ID | Optional (to Entra ID) |
| **Use Case** | Standalone PCs, unmanaged devices | Traditional enterprise networks | SaaS apps, cloud-first orgs | Legacy app support in cloud | Full AD control in cloud |
| **Admin Control** | Local only | Full domain control | Role-based (RBAC, PIM) | Limited (managed service) | Full domain controller access |

---

## ☁️ Azure Identity Features

| Feature | Description |
|--------|-------------|
| Entra ID | Cloud identity for apps, users, devices |
| Azure AD Connect | Sync on-prem AD with Entra ID |
| Azure RBAC | Role-based access for resources |
| Azure Policy | Governance enforcement |
| PIM | Just-in-time privileged access |
| Managed Identities | Credential-free service access |
| Conditional Access | Policy-based authentication control |

---

## 🧠 Best Practices

- Use **RBAC** to avoid over-permissioning.
- Sync **on-prem AD** with **Entra ID** for hybrid identity.
- Use **Entra Domain Services** for legacy app support in cloud.
- Regularly audit **group memberships** and **access rights**.
- Avoid assigning permissions directly to users—use groups.
- Use **Privileged Identity Management (PIM)** for just-in-time admin access.

---
