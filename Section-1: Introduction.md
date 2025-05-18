# Jump To
1. Introduction

# 1. Introduction to Microsoft 365 and Azure
This topic provides a foundational overview of Microsoft 365 and Microsoft Azure — two core components of the modern Microsoft cloud ecosystem. It explains how Microsoft 365 delivers cloud-based productivity and collaboration tools (like Teams, Intune, Exchange, and SharePoint), while Azure provides the underlying cloud infrastructure and identity services (like Entra ID, VMs, networking, and security). It also highlights how these platforms integrate to support secure identity management, device compliance, and enterprise-level IT operations.

## 🔷 What is Microsoft 365?
Microsoft 365 is a cloud-based productivity suite (PaaS) that includes:
  * **Office Apps**: Word, Excel, Outlook, PowerPoint (web and desktop versions)
  * **Exchange Online**: Cloud-based email and calendaring
  * **SharePoint Online**: Document management and intranet services
  * **OneDrive for Business**: Cloud storage
  * **Microsoft Teams**: Chat, video meetings, and collaboration
  * **Intune**: Device and application management (MDM/MAM)

## 🔷 What is Microsoft Azure?
Azure is Microsoft’s cloud platform offering infrastructure, platform, and software services (IaaS, PaaS, SaaS).
  * **Azure Virtual Machines**: Cloud-based Windows/Linux servers
  * **Azure Storage**: Blob, file, queue, and disk storage
  * **Azure Networking**: Virtual networks, firewalls, VPNs
  * **Azure Active Directory (Entra ID)**: Identity service used across Microsoft 365
  * **Azure Monitor & Defender**: Security, monitoring, and compliance tools
  * **Azure AD Connect**: Syncs on-premises AD with Entra ID (hybrid identity)

## 🔷 How Microsoft 365 and Azure Work Together
Microsoft 365 = The tools you use | Azure = The engine that powers those tools
  * Single Sign-On (SSO) across Microsoft 365 and Azure
  * Multi-Factor Authentication (MFA) via Azure
  * Licensing & Role Management is unified using Microsoft Admin Centers
  * Device management and compliance enforced via Microsoft Intune integrated with Azure AD

## 🔷 Admin Portals You Should Know

| Portal                        | Purpose                                             |
|------------------------------|-----------------------------------------------------|
| **Microsoft 365 Admin Center** | Manage users, licenses, apps, and services          |
| **Entra Admin Center (Azure AD)** | Identity and access management                    |
| **Intune Admin Center**       | Device compliance, policies, and deployment         |
| **Azure Portal**              | Manage all Azure resources (VMs, networks, storage) |
| **Security & Compliance Center** | Data loss prevention, audit logs, security reports |




# 2. Microsoft Active Directory Domain Architecture 
Learn the basics of Active Directory Domains, including how they help manage users, computers, and resources in a Windows network. This guide covers key concepts and steps to get started with setting up and using Active Directory.

## 🔷 What is Microsoft Active Directory (AD)?
Active Directory is a directory service developed by Microsoft for Windows domain networks. It is used to manage and organize users, computers, and other resources within a network.

## 🔷 What is a Domain in Active Directory?
An AD domain is a logical group of network objects (like users, computers, and devices) that share the same AD database and security policies.
 * A domain is identified by its DNS name (e.g., company.local).
 * It is managed by one or more Domain Controllers (DCs).
 * All objects in a domain are part of a single security boundary — meaning policies and permissions can be enforced centrally.

## 🔷 Core Components of an AD Domain

| Component                  | Description                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| **Domain Controller (DC)** | A server that stores the AD database and handles authentication and authorization |
| **Users**                  | Individual accounts that authenticate to access resources on the network    |
| **Computers**              | Devices that are joined to the domain for centralized control and policies  |
| **Groups**                 | Logical collections of users or computers for easier permission management |
| **Organizational Units (OUs)** | Containers for organizing AD objects and applying Group Policies        |

  
## 🔷 What is a Domain Controller (DC)?
A Domain Controller is a Windows Server that runs Active Directory Domain Services (AD DS). It is the central authority for handling authentication and authorization within an AD domain.
 * **Login Authentication**: Verifies credentials of users trying to access the network.
 * **Directory Lookups**: Responds to queries for object information (e.g., user details, group membership).
 * **Replication**: Syncs directory data with other DCs in the same domain or forest.
 * **Policy Enforcement**: Applies Group Policy settings across domain members.






# A foundation for Remote Access, DMZs, and Virtualization

# A foundation of the Microsoft Cloud Services

