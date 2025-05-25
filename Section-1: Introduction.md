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
  - **Multi-Factor Authentication (MFA)**: Confirms it's really you with extra proof (beyond just a password). It uses things like phone codes or biometrics, often via Microsoft Entra ID.
  - **Group Policy Objects (GPOs)**: Lets admins set rules for security, software installation, and desktop settings across many computers automatically
  - **Security Groups**: Simplify permission management by bundling users and computers together. Access rights are then granted to the group, and they also help target Group Policy application.

* **Organization & Structure:**
  - **Domains**: The main way AD DS groups things. Think of `company.com` as a domain – it's a boundary for management and security.
  - **Organizational Units (OUs)**: Folders within a domain to further organize users, computers (e.g., "Sales" OU, "Marketing" OU). This helps delegate administrative tasks.
  - **Trees**: Groups domains that extend a common DNS name (like `uk.mycorp.com` from `mycorp.com`). These domains automatically trust each other.
  - **Forests**: Unites one or more domain trees (even with different DNS names like `mycorp.com` and `anotherbusiness.org`). They all share a common directory blueprint and trust each other by default.
  - **Schema**: Think of it as the official **rulebook and set of definitions** for Active Directory. It clearly lists:
    - Every **type of item** AD can keep track of (like 'users,' 'computers,' 'printers').
    - All the **specific details** (or properties) it can store for each type of item (like a user having a 'name,' 'email address,' and 'department').

* **Replication & Reliability**
  - **Domain Controllers (DCs)**: Use a **multi-master replication** model where information is copied and kept up-to-date across these multiple servers. If one DC has a problem, others can take over, so the network stays running (fault tolerance).
  - **FSMO Roles**: Assigns unique, critical AD DS jobs to a single designated Domain Controller (to prevent conflicts). These roles handle tasks like schema updates or distributing new security IDs.
  - **SYSVOL Replication**: Copies Group Policy files and login scripts between all Domain Controllers (to ensure consistent policy application). It uses DFSR (Distributed File System Replication) technology in modern setups.
  - **Replication Topology (Sites & KCC)**: Governs how and when AD data replicates between Domain Controllers (optimized for different physical locations called Sites). The KCC (Knowledge Consistency Checker) automatically creates these efficient replication routes.
  - **Conflict Resolution (AD Replication)**: Decides which change "wins" if the same data is updated differently on two Domain Controllers before they sync up (to prevent data inconsistencies). It typically uses methods like timestamps or version numbers to pick the correct update.

* **Directory Services:**
  - Provides a searchable directory (like a phonebook) so users and applications can find resources (e.g., finding a printer). It uses **LDAP** (Lightweight Directory Access Protocol) for queries.
* **Single Sign-On (SSO)**
  - Log in once with your username and password to access multiple network resources you're authorized for, without needing to log in repeatedly.

* **Integration & Compliance**
  - **Microsoft Entra ID**: Sync with cloud services for hybrid environments.
  - **Audit Logs**: Track security events (logins, permission changes) for compliance.

## 🔷 How It Works
* **The Directory (NTDS.dit)**: Imagine a big, organized address book or database stored on servers called **Domain Controllers (DCs)**. This database holds all the info about users, computers, permissions, etc
* **Domain Controllers (DCs)**: Think of these as powerful Windows Servers that act as the **guardians and managers** of the domain because they run the **Active Directory Domain Services (AD DS)**
  - When you log in to a computer joined to the domain, your computer talks to a DC.
  - The DC checks your username and password (**authentication**).
  - If you're verified, the DC tells your computer what you have access to based on your permissions and any Group Policies that apply to you (**authorization**).
 
* **DNS (Domain Name System)**: AD DS heavily relies on DNS (like the internet's phonebook) to help computers find DCs and other services within the domain.
* **Structure**:
  - Everything is organized in a hierarchy: **Forest** (top level) > **Trees** (if multiple related domain structures) > **Domains** (e.g., `sales.mycompany.com`) > **Organizational Units (OUs)** (e.g., `US_Sales_Team` OU). This allows for clear organization and targeted policy application.


---

## 🔷 What is a Domain in Active Directory?
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




# A foundation for Remote Access, DMZs, and Virtualization


