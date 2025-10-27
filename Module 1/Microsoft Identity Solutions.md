# Microsoft Identity Solutions
**Microsoft Identity Solutions provide a unified framework for managing user identities, access permissions, and authentication across local devices, on-premises networks, and cloud environments. These solutions enable secure, scalable, and flexible identity management for organizations of all sizes.**

---

## 🟦 Identity Fundamentals

- **Authentication**: Verifying user identity using credentials or tokens (e.g., username + password).
- **Authorization**: Granting access to resources based on roles and policies (e.g., open file, change settings).
- **Governance**: Controlling lifecycle, compliance, and privileged access.

## 🧱 Identity Models Covered

- **Windows Local Identity**: Device-specific accounts and groups.
- **Active Directory Domain Services (AD DS)**: Centralized on-premises identity infrastructure.
- **Microsoft Entra ID**: Cloud-native identity provider for SaaS and enterprise apps.
- **Microsoft Entra Domain Services**: Managed domain services in Azure for legacy app support.
- **Azure IaaS AD DS**: Full AD deployment on cloud-hosted Windows Server VMs.

> Microsoft’s identity ecosystem supports hybrid environments, enabling seamless integration between on-premises and cloud systems while enforcing modern security practices like Conditional Access, MFA, and Zero Trust.

---

## 🟦 Windows Local Accounts
Windows Local Accounts are user identities created and stored directly on a specific Windows device. They are not connected to any domain or cloud identity provider, making them ideal for standalone systems or simple environments.

### Key Characteristics

- Stored in the **Security Account Manager (SAM)** database on the local machine.
- Used for **authentication and access control** on that device only.
- Can be created during setup or manually via **Settings** or **Computer Management**.
- Supports **local password policies**, but lacks centralized management or synchronization.

### Group Types

| Group Type | Description | Use Case |
|------------|-------------|----------|
| Local Groups | Built-in groups for assigning permissions on a single device. | File system access, service control, remote desktop permissions |
| Special Identity Groups | System-defined identities used in access control lists (ACLs). Not manually created or managed. | OS-level access control, service permissions, anonymous or network logon contexts |

### Group Scopes

- ❌ No domain or forest structure.
- All groups are **local to the device**.

### Local Groups

Local groups are used to assign permissions to multiple users on a single device. They are managed via:

- **Computer Management → Local Users and Groups**
- **PowerShell (`Get-LocalGroup`, `Add-LocalGroupMember`)**

### Common Local Groups

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

### Special Identity Groups

These are predefined identity groups in Windows and Active Directory environments. They are automatically created by the system and used to manage access control and permissions.

| Group | Description |
|-------|-------------|
| **Anonymous Logon** | Represents users accessing resources without credentials. |
| **Authenticated Users** | All users who have signed in successfully. |
| **Creator Owner** | Refers to the user who created a file or folder. |
| **Everyone** | Includes all users, including guests and anonymous. |
| **Interactive** | Users logged in locally to the computer. |
| **Network** | Users accessing the system remotely. |
| **Service** | Represents services running under service accounts. |
| **System** | Represents the operating system itself. |

### Membership Effects

- **Direct Membership**: Users added to local groups inherit permissions on that device.
- **No Nesting Support**: Local groups do not support nested group structures.
- **Access Control**: Membership affects access to local files, folders, services, and system features.
- **Special Identity Groups**: Automatically assigned based on login context (e.g., Network, Interactive).

---

## 🟦 On-Premises AD Identity Management

Active Directory Domain Services (AD DS) is Microsoft’s on-premises identity and access management system. It provides centralized control over users, groups, devices, and resources.

### Key Characteristics

- **Hierarchical and Granular**: AD is structured like a tree with forests, domains, and organizational units (OUs). This allows fine-grained control over resources and policies.
- **Security Based on Groups**: Permissions are assigned to groups rather than individual users. This simplifies access control and improves scalability.
- **Group Policy Administration**: AD uses Group Policy Objects (GPOs) to enforce settings across users and computers (e.g., password policies, desktop restrictions).
- **Kerberos Authentication**: AD uses the Kerberos protocol for secure, ticket-based authentication. It’s fast, secure, and supports single sign-on (SSO).

### Group Types

| Type | Description | Use Case |
|------|-------------|----------|
| **Security Group** | Used to assign permissions to resources. | File shares, printers, apps. |
| **Distribution Group** | Used for email distribution only. | Exchange mailing lists. |

### Group Scopes

| Scope | Description | Use Case |
|-------|-------------|----------|
| **Domain Local** | Used within a single domain. | Grant access to local resources. |
| **Global** | Members from same domain, usable across domains. | Department-level access. |
| **Universal** | Members from any domain in forest. | Enterprise-wide access across domains.

### Group Management Tools

- **Active Directory Users and Computers (ADUC)**: GUI tool for managing users, groups, and OUs.
- **PowerShell**: Scripted management using cmdlets like `New-ADGroup`, `Add-ADGroupMember`, `Get-ADGroup`.
- **GPMC**: Group Policy Management Console

### Membership Effects

- **Direct Membership**: User inherits permissions assigned to the group.
- **Nested Groups**: Groups can be added to other groups for scalable access control.
- **Transitive Membership**: Universal groups support nesting across domains.
- **Access Control**: Membership affects access to files, folders, apps, and GPOs.

### Authentication Protocol

- **Kerberos**: Default protocol used in AD DS. Provides secure, ticket-based authentication.
- **NTLM**: Legacy protocol, still supported but less secure.

> ⚠️ Best Practice: Use **Global groups for users**, **Domain Local groups for resources**, and **Universal groups for cross-domain access**.

---

## 🟦 Cloud Identity Management

Cloud identity solutions extend identity and access management beyond traditional on-premises environments. These services are designed for scalability, security, and integration with modern apps and infrastructure.

### 🔐 Microsoft Entra ID (formerly Azure AD)

Microsoft Entra ID is a **cloud-based identity provider** used to manage users, groups, devices, and access to SaaS applications.

#### Key Features
- Single Sign-On (SSO) for Microsoft 365, Zoom, Salesforce, etc.
- Supports OAuth2, OpenID Connect, SAML protocols.
- Role-Based Access Control (RBAC) for apps and resources.
- Conditional Access policies for secure authentication.
- Identity Governance: Access reviews, entitlement management, lifecycle workflows.

#### User Management
- Create users manually or sync from on-prem AD via Azure AD Connect.
- Assign licenses (e.g., Microsoft 365, Intune).
- Configure user attributes, roles, and MFA settings.
- Delegate access using **RBAC** and **PIM**.

#### Group Types

| Group Type | Description | Use Case |
|------------|-------------|----------|
| **Security Group** | Assign access to apps and resources. | Conditional Access, app targeting (e.g., SharePoint, Teams) |
| **Microsoft 365 Group** | Collaboration group with mailbox, Teams, Planner. | Team-based collaboration |
| **Dynamic Group** | Auto-membership based on attributes. | Auto-grouping by department, location (e.g., department = Sales) |
| **Distribution Group** | Email distribution only. | Exchange Online mailing lists |

> ⚠️ Dynamic groups in Entra ID are available only for Security and Microsoft 365 Groups.

#### Group Scopes

- ❌ No Domain Local, Global, or Universal scopes.
- All groups are **tenant-wide** and operate across the entire Entra tenant.

#### Membership Effects

- **Direct Membership**: Users explicitly added to a group inherit permissions and access assigned to that group. 
- **Dynamic Membership**: Users are automatically added/removed based on attributes (e.g., department, job title). Managed via rules. 
- **Role Assignment**: Group membership can be used to assign roles (e.g., Entra roles, Intune roles, Azure RBAC roles). 
- **Access Control**: Membership affects access to apps, Conditional Access policies, license assignments, and resource targeting. 
- **App Assignment**: Groups can be used to assign access to enterprise applications (e.g., Salesforce, Zoom, SharePoint). 
- **Policy Targeting**: Groups are used to scope Conditional Access, compliance policies, and Intune configurations. 

> ⚠️ Entra ID does not support nested groups in the same way as AD DS. While some apps (like Exchange Online) may support nested group resolution, Entra ID itself treats group membership as flat.

#### Group Management Tools
- Microsoft Entra Admin Center
- PowerShell (`New-AzureADGroup`, `Add-AzureADGroupMember`)
- Microsoft Graph API

---

### 🔐 Microsoft Entra Domain Services

Entra Domain Services provides **managed domain services** like domain join, LDAP, Kerberos, and Group Policy—**without deploying domain controllers**.

#### Key Features
- Domain join for Azure VMs.
- LDAP and Kerberos authentication.
- Group Policy support in cloud environments.
- No need to patch or manage domain controllers.

#### User Management
- Users are synced from Entra ID (no direct creation in Entra DS).
- Passwords must be synced using **Azure AD password hash sync**.
- Users can log in to domain-joined VMs using their Entra credentials.

#### Group Types

| Group Type | Description | Use Case |
|------------|-------------|----------|
| Security Group (synced from Entra ID) | Used for access control and GPO targeting. | Domain-joined VM access, legacy app permissions |

#### Group Scopes

| Scope | Description |
|-------|-------------|
| Domain Local | Behaves like AD DS but scoped to the managed domain |

#### Group Membership Effects

- **Direct Membership**: Users inherit permissions from Entra-synced security groups.
- **Group Policy Targeting**: Membership affects GPO application within the managed domain.
- **Domain Join Access**: Group membership controls login rights to domain-joined Azure VMs.
- **No Dynamic or Nested Groups**: Entra DS does not support dynamic or nested group logic natively.

#### Group Management Tools
- Group Policy Management Console (GPMC) on domain-joined VM
- Entra Admin Center (for upstream group management)

---

### 🔐 Azure IaaS Windows Server VMs with AD DS

This approach involves deploying **Windows Server virtual machines** in Azure and manually installing **Active Directory Domain Services (AD DS)**.

#### Key Features
- Full control over domain controllers.
- Supports traditional AD features: forests, trusts, OUs, GPOs.
- Can be integrated with on-prem AD via VPN or ExpressRoute.

#### User Management
- Create users using **Active Directory Users and Computers (ADUC)**.
- Full control over user attributes, OU placement, and login policies.
- Can sync with Entra ID using **Azure AD Connect**.

#### Group Types

| Group Type | Description | Use Case |
|------------|-------------|----------|
| Security Group | Assign permissions to resources. | File shares, apps, GPOs |
| Distribution Group | Email distribution only. | Exchange mailing lists |

### Group Scopes

| Scope | Description | Use Case |
|-------|-------------|----------|
| Domain Local | Used within a single domain. | Local resource access |
| Global | Members from same domain, usable across domains. | Department-level access |
| Universal | Members from any domain in forest. | Enterprise-wide access across domains |

#### Group Membership Effects

- **Direct Membership**: Users inherit permissions from AD DS groups.
- **Nested Groups**: Supports scalable access control via group nesting.
- **Transitive Membership**: Universal groups allow cross-domain nesting and access.
- **Access Control**: Membership affects access to files, folders, apps, and GPOs.
- **Full AD Behavior**: Mirrors traditional on-prem AD group logic and effects.

#### Group Management Tools
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

## 🧩 Azure Identity Features

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
