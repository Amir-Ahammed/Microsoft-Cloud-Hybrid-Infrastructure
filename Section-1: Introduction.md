---
# Jump To
1. Introduction
2. Microsoft Active Directory (AD) Architecture & Core Components
3. 

---


## 1. Introduction to Microsoft 365 and Azure
This topic provides a foundational overview of Microsoft 365 and Microsoft Azure — two core components of the modern Microsoft cloud ecosystem. It explains how Microsoft 365 delivers cloud-based productivity and collaboration tools (like Teams, Intune, Exchange, and SharePoint), while Azure provides the underlying cloud infrastructure and identity services (like Entra ID, VMs, networking, and security). It also highlights how these platforms integrate to support secure identity management, device compliance, and enterprise-level IT operations.

### 🔷 What is Microsoft 365?
Microsoft 365 is a cloud-based productivity suite (PaaS) that includes:
  * **Office Apps**: Word, Excel, Outlook, PowerPoint (web and desktop versions)
  * **Exchange Online**: Cloud-based email and calendaring
  * **SharePoint Online**: Document management and intranet services
  * **OneDrive for Business**: Cloud storage
  * **Microsoft Teams**: Chat, video meetings, and collaboration
  * **Intune**: Device and application management (MDM/MAM)

### 🔷 What is Microsoft Azure?
Azure is Microsoft’s cloud platform offering infrastructure, platform, and software services (IaaS, PaaS, SaaS).
  * **Azure Virtual Machines**: Cloud-based Windows/Linux servers
  * **Azure Storage**: Blob, file, queue, and disk storage
  * **Azure Networking**: Virtual networks, firewalls, VPNs
  * **Azure Active Directory (Entra ID)**: Identity service used across Microsoft 365
  * **Azure Monitor & Defender**: Security, monitoring, and compliance tools
  * **Azure AD Connect**: Syncs on-premises AD with Entra ID (hybrid identity)

### 🔷 Admin Portals You Should Know

| Portal                        | Purpose                                             |
|------------------------------|-----------------------------------------------------|
| **Microsoft 365 Admin Center** | Manage users, licenses, apps, and services          |
| **Entra Admin Center (Azure AD)** | Identity and access management                    |
| **Intune Admin Center**       | Device compliance, policies, and deployment         |
| **Azure Portal**              | Manage all Azure resources (VMs, networks, storage) |
| **Security & Compliance Center** | Data loss prevention, audit logs, security reports |

---

## 2. Microsoft Active Directory Domain Services (AD DS)
**Microsoft Active Directory Domain Services (AD DS)** is a server role in Windows Server operating systems that allows administrators to manage and organize network resources. It provides a centralized and secure way to manage users, computers, and other network objects within an organization.

### 🔷 What is AD DS?
**AD DS** is a **centralized database** that stores information about network resources. This includes:
* **Users** (like employees, their usernames, passwords)
* **Computers** (which machines are part of the network)
* **Printers**, **shared folders**, and other devices/services.

### 📌 Key Features & Core Functions
* **Centralized User & Resource Management:**
  - Stores **user/computer accounts, passwords, and permissions** in a centralized directory.
  - Manages all users, computers, and resources from the directory.
  - Manages access to network resources (files, printers, apps).
  - No need to set up each computer individually for every user or resource.

* **Security & Authentication:**
  - **Identifies users and computers**: Makes sure you are who you say you are (like a bouncer checking IDs – this is called authentication).
  - **Controls access**: Decides what you are allowed to use or see (this is called **authorization**). It uses things like **Kerberos** (a secure ticket system) for this
  - **Multi-Factor Authentication (MFA)**: Supported via Azure AD hybrid integration.
  - **Group Policy Objects (GPOs)**: Lets admins set rules for security, software installation, and desktop settings across many computers automatically

* **Organization & Structure:**
  - **Domains**: The main way AD DS groups things. Think of `company.com` as a domain – it's a boundary for management and security.
  - **Organizational Units (OUs)**: Folders within a domain to further organize users, computers (e.g., "Sales" OU, "Marketing" OU). This helps delegate administrative tasks.
  - **Trees**: Multiple domains sharing a contiguous namespace (e.g., `us.company.local`, `eu.company.local`).
  - **Forests**: Collections of domain trees with shared **trust relationships** (transitive or non-transitive).

* **Replication & Reliability**
  - **Domain Controllers (DCs)**: Information is copied and kept up-to-date across multiple servers called **Domain Controllers (DCs)**. If one DC has a problem, others can take over, so the network stays running (fault tolerance).
  - **FSMO Roles**: Critical roles like Schema Master and PDC Emulator handle unique tasks.

* **Directory Services:**
  - Provides a searchable directory (like a phonebook) so users and applications can find resources (e.g., finding a printer). It uses **LDAP** (Lightweight Directory Access Protocol) for queries.
* **Single Sign-On (SSO)**
  - Log in once with your username and password to access multiple network resources you're authorized for, without needing to log in repeatedly.

* **Integration & Compliance**
  - **Azure AD Hybrid**: Sync with cloud services for hybrid environments.
  - **Audit Logs**: Track security events (logins, permission changes) for compliance.

## 🔷 How It Works
* **The Directory (NTDS.dit)**: Imagine a big, organized address book or database stored on servers called **Domain Controllers (DCs)**. This database holds all the info about users, computers, permissions, etc
* **Domain Controllers (DCs)**: These are the **guardians and managers** of the domain.
  - When you log in to a computer joined to the domain, your computer talks to a DC.
  - The DC checks your username and password (**authentication**).
  - If you're verified, the DC tells your computer what you have access to based on your permissions and any Group Policies that apply to you **(authorization)**.
 
* **DNS (Domain Name System)**: AD DS heavily relies on DNS (like the internet's phonebook) to help computers find DCs and other services within the domain.
* **Structure**:
  - Everything is organized in a hierarchy: **Forest** (top level) > **Trees** (if multiple related domain structures) > **Domains** (e.g., `sales.mycompany.com`) > **Organizational Units (OUs)** (e.g., `US_Sales_Team` OU). This allows for clear organization and targeted policy application.


---











* **Core Services**
  - **AD Domain Services (AD DS)** – Central authentication/authorization.
  - **Domain** – Logical security boundary (e.g., `corp.example.com`).
  - **Domain Controller (DC)** – Hosts AD DS; replicates changes.
  - **Global Catalog (GC)** – Enables cross-domain queries.
* **Key Roles**
  - **FSMO Roles** – Schema Master, PDC Emulator, etc.
  - **Group Policy (GPOs)** – Policy enforcement across domains/OUs.
  - **Organizational Units (OUs)** – Hierarchical object organization.
  - **AD Replication** – Multi-master replication with KCC-managed topology.
* **Physical & Logical Design**
  - **AD Sites & Subnets** – Optimize traffic for WAN/LAN.
  - **Trust Relationships** – Connect domains/forests (transitive/non-transitive).
  - **Read-Only DCs (RODCs)** – Secure deployment for remote sites.
* **Dependencies & Tools**
  - **DNS Integration** – Critical for AD functionality (SRV records).
  - **AD Database (NTDS.dit)** – Storage location for objects.
  - **AD Database (NTDS.dit)** – Storage location for objects.
* **Security & Authentication**
  - **Kerberos**: Primary authentication protocol (secure ticket-based).
  - **NTLM**: Legacy fallback protocol (less secure).
  - **Multi-Factor Authentication (MFA)**: Supported via Azure AD hybrid integration.
  - **Group Policy Objects (GPOs)**: Enforce security policies, deploy software, run scripts, and manage registry settings.








* **AD Domain Services (AD DS)**
  - The core service handling authentication and object management.
  - Schema: Defines object classes/attributes (e.g., user, group, printer).
  - Global Catalog (GC): A distributed index of all objects in the forest for fast searches.
  - Runs on Domain Controllers (DCs).
* **AD Domain**
  - A logical security boundary (e.g., `corp.example.com`).
  - Core unit for administration, security, and policy enforcement.
  - Each domain has a unique DNS name.
  - Contains its own objects (users, groups, computers) and domain controllers.
* **Domain Controller (DC)** – The server hosting AD DS.
  - A server hosting AD DS.
  - Authenticates users and enforces policies.
* **FSMO Roles** – Specialized DC roles (Schema Master, PDC Emulator, etc.).
  - Schema Master (modifies AD schema).
* **Group Policy (GPOs)** – Centralized policy enforcement.
* **Organizational Units (OUs)** – Containers for organizing AD objects.


### AD Domain Services (AD DS)



### What is a Domain in Active Directory?
An AD domain is a logical group of network objects (like users, computers, and devices) that share the same AD database and security policies.
 * A domain is identified by its DNS name (e.g., company.local).
 * It is managed by one or more Domain Controllers (DCs).
 * All objects in a domain are part of a single security boundary — meaning policies and permissions can be enforced centrally.

Core Components of an AD Domain

| Component                  | Description                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| **Domain Controller (DC)** | A server that stores the AD database and handles authentication and authorization |
| **Users**                  | Individual accounts that authenticate to access resources on the network    |
| **Computers**              | Devices that are joined to the domain for centralized control and policies  |
| **Groups**                 | Logical collections of users or computers for easier permission management |
| **Organizational Units (OUs)** | Containers for organizing AD objects and applying Group Policies        |



---



| Feature | Description |
|---------|-------------|
| ...     | ...         |



## 🔷 What is a Domain Controller (DC)?
A Domain Controller is a Windows Server that runs Active Directory Domain Services (AD DS). It is the central authority for handling authentication and authorization within an AD domain.
 * **Login Authentication**: Verifies credentials of users trying to access the network.
 * **Directory Lookups**: Responds to queries for object information (e.g., user details, group membership).
 * **Replication**: Syncs directory data with other DCs in the same domain or forest.
 * **Policy Enforcement**: Applies Group Policy settings across domain members.






# A foundation for Remote Access, DMZs, and Virtualization


