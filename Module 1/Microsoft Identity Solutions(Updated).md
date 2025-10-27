# 🧩 Microsoft Identity Solutions

## 🔐 Identity Fundamentals

- **Authentication**: Verifies identity (e.g., username + password).
- **Authorization**: Grants access to resources (e.g., open file, change settings).
- **Capabilities**: Management rights (e.g., admin privileges).

---

## 🧑‍💻 Local Identity Management

### 🏠 Windows Local Accounts
- Created during setup or manually.
- Stored in the local SAM database.
- Used for access to that device only.

### 👥 Local Groups

Managed via:
- **Computer Management → Local Users and Groups**
- **PowerShell**: `Get-LocalGroup`, `Add-LocalGroupMember`

| Group | Purpose |
|-------|---------|
| Administrators | Full control over the device. |
| Backup Operators | Can back up and restore files. |
| Event Log Readers | Can view system logs. |
| Guests | Limited access. |
| Users | Standard access. |
| Power Users | Elevated privileges, less than Admins. |
| Remote Desktop Users | Remote login access. |
| Network Config Operators | Manage network settings. |
| Replicator | Supports file replication. |
| Distributed COM Users | Launch/use DCOM objects. |
| Cryptographic Operators | Perform crypto operations. |
| Device Owners | Identified device owners. |
| Access Control Assist Operators | Query access permissions remotely. |

> ⚠️ Assign users to the **least privileged group** necessary.

---

## 🧙 Special Identity Groups

| Group | Description |
|-------|-------------|
| Anonymous Logon | Unauthenticated access. |
| Authenticated Users | Signed-in users. |
| Creator Owner | File/folder creator. |
| Everyone | All users, including guests. |
| Interactive | Local login users. |
| Network | Remote access users. |
| Service | Background service accounts. |
| System | Operating system identity. |

---

## 🏢 On-Premises AD Identity Management

### 🌳 Key Concepts
- **Hierarchical**: Forests, domains, OUs.
- **Group-Based Security**: Permissions via groups.
- **Group Policy**: Centralized configuration via GPOs.
- **Kerberos Authentication**: Secure, ticket-based SSO.

### 👥 Group Types & Scopes

| Type | Description | Use Case |
|------|-------------|----------|
| Security | Assign permissions. | File shares, apps. |
| Distribution | Email only. | Exchange lists. |

| Scope | Description | Use Case |
|-------|-------------|----------|
| Domain Local | Access within one domain. | Local resource access. |
| Global | Members from same domain. | Cross-domain access. |
| Universal | Members from any domain. | Forest-wide access.

### 🔧 Management Tools
- **ADUC** (GUI)
- **PowerShell**: `New-ADGroup`, `Add-ADGroupMember`
- **GPMC**: Group Policy Management Console

### 🧩 Membership Effects
- **Direct**: Immediate access.
- **Nested**: Scalable access via group nesting.
- **Transitive**: Universal groups support cross-domain nesting.
- **Access Control**: Affects files, folders, apps, GPOs.

> ⚠️ Use Global for users, Domain Local for resources, Universal for cross-domain.

---

## ☁️ Cloud Identity Management

### 🔐 Microsoft Entra ID

Cloud-native identity provider for users, apps, and devices.

#### Key Features
- SSO for SaaS apps
- OAuth2, OpenID Connect, SAML
- RBAC, Conditional Access
- Identity Governance (PIM, access reviews)

#### User & Group Management
- Manual or synced users (via Azure AD Connect)
- Security, Microsoft 365, and Dynamic Groups
- Role assignment via RBAC and PIM

#### Tools
- Entra Admin Center
- PowerShell (`New-AzureADGroup`)
- Microsoft Graph API

---

### 🏢 Microsoft Entra Domain Services

Managed AD-like service—no domain controllers required.

#### Key Features
- Domain join, LDAP, Kerberos
- Group Policy support
- Password hash sync from Entra ID

#### User & Group Management
- Users synced from Entra ID
- Security groups available
- GPOs applied via GPMC on domain-joined VM

---

### 🧱 Azure IaaS Windows Server VMs with AD DS

Self-managed AD in Azure via Windows Server VMs.

#### Key Features
- Full control over AD DS
- Forests, trusts, OUs, GPOs
- VPN/ExpressRoute integration

#### User & Group Management
- ADUC for user creation
- Security & Distribution groups
- Domain Local, Global, Universal scopes
- GPOs for policy enforcement

---

## 🆚 Admin Center Comparison

| Feature | Microsoft 365 Admin Center | Entra Admin Center | Intune Admin Center | Azure Portal |
|---------|----------------------------|---------------------|----------------------|---------------|
| Interface | Simplified | Advanced | Policy-driven | Technical & modular |
| User Management | Basic | Granular (RBAC, PIM) | Device-focused | Identity + resource access |
| Group Types | M365 Groups, DLs | Security, Dynamic | Entra-based targeting | Entra-based access control |
| Role Assignment | Limited | Full RBAC & PIM | Scoped roles | Azure RBAC |
| Governance | Minimal | Lifecycle & access reviews | Compliance policies | Policies, blueprints, audit logs |
| Focus Area | Collaboration | Identity & access | Endpoint security | Infrastructure & cloud services |

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

- Use **RBAC** to minimize over-permissioning.
- Sync **on-prem AD** with **Entra ID** for hybrid identity.
- Use **Entra Domain Services** for legacy app support.
- Audit **group memberships** and **access rights** regularly.
- Assign permissions via **groups**, not individual users.
- Use **PIM** for sensitive role access.

