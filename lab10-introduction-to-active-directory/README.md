# Lab 10 – Introduction to Active Directory

## Overview

This lab focuses on deploying and configuring a Windows Server 2025 environment with Active Directory Domain Services (AD DS).

The objective is to gain practical experience with Active Directory administration, Domain Controllers, DNS, Organizational Units, user and group management, Windows domain authentication, shared network resources, and group-based access permissions.

The environment is being built using Hyper-V on a Windows 11 Pro host.

This project is designed to develop practical skills relevant to Tier 1 IT Support / Help Desk and future Junior System Administrator roles.

## Lab Objectives

- Install and configure Windows Server 2025.
- Deploy Active Directory Domain Services (AD DS).
- Promote a Windows Server machine to a Domain Controller.
- Create and configure an Active Directory domain.
- Understand the relationship between Active Directory and DNS.
- Create and manage Organizational Units (OUs).
- Create and manage domain user accounts.
- Configure security groups and group membership.
- Join a Windows 11 client to an Active Directory domain.
- Practise domain authentication and basic account administration.
- Troubleshoot network connectivity and DNS resolution between domain machines.
- Configure shared network folders.
- Understand the difference between share permissions and NTFS permissions.
- Apply group-based access permissions to shared resources.
- Test access to network resources using different domain user accounts.

## Lab Environment

| Component | Configuration |
|---|---|
| Hyper-V Host | EDU-LAB-CL02 |
| Host OS | Windows 11 Pro |
| Processor | AMD Ryzen AI 7 PRO 450 |
| RAM | 32 GB |
| Hypervisor | Hyper-V |
| Domain Controller | EDU-DC01 |
| Server OS | Windows Server 2025 Standard Evaluation |
| Server VM Generation | Generation 2 |
| Server VM Memory | 4 GB |
| Server Virtual Processors | 4 |
| Server Virtual Disk | 80 GB Dynamic VHDX |
| Windows Client | EDU-CL03 |
| Client OS | Windows 11 Pro |
| Client VM Memory | 4 GB |
| Active Directory Domain | ad.lab |

## Network Architecture

A dedicated Hyper-V Internal Virtual Switch was created to provide an isolated environment for the Active Directory lab.

**Virtual Switch:** `EDU-LAB-INT`

**Network:** `192.168.10.0/24`

| Device | Hostname | IP Address | Subnet Mask | Status |
|---|---|---|---|---|
| Hyper-V Host | EDU-LAB-CL02 | 192.168.10.1 | 255.255.255.0 | Configured |
| Domain Controller | EDU-DC01 | 192.168.10.10 | 255.255.255.0 | Configured |
| Windows Client | EDU-CL03 | 192.168.10.20 | 255.255.255.0 | Configured |

**Domain Controller Network Configuration**

- Static IPv4 address: `192.168.10.10`
- Preferred DNS server: `192.168.10.10`
- Default gateway: Not configured
- DHCP: Disabled

The Domain Controller uses its own IP address for DNS services.

The Internal Virtual Switch allows communication between the Hyper-V host and connected virtual machines. Internet access has not been configured for this isolated network.

## Implementation and Completed Tasks

### 1. Hyper-V Configuration

- Created a dedicated Internal Virtual Switch named `EDU-LAB-INT`.
- Configured the host's virtual network adapter with a static IPv4 address.
- Established the network addressing plan for the lab.

### 2. Windows Server Installation

- Created a Generation 2 virtual machine named `EDU-DC01`.
- Installed Windows Server 2025 Standard Evaluation with Desktop Experience.
- Configured static IPv4 addressing.
- Renamed the server to `EDU-DC01`.

### 3. Active Directory Deployment

- Installed the Active Directory Domain Services role.
- Created a new Active Directory forest.
- Configured the domain name `ad.lab`.
- Enabled DNS Server and Global Catalog.
- Completed the Domain Controller promotion process.
- Successfully restarted and logged into the Domain Controller.
- Verified the Active Directory domain configuration.

### 4. Active Directory User and Group Management

**Organizational Units**

Created a structured Organizational Unit (OU) hierarchy under `Company`:

- `Users`
- `Computers`
- `Groups`
- `Departments`

**Domain User Accounts**

Created the following domain user accounts:

- John Smith (`jsmith`)
- Sarah Johnson (`sjohnson`)

Both accounts were created under `Company\Users`.

**Security Groups**

Created the following Global Security Groups under `Company\Groups`:

- `IT Support`
- `HR`

Configured group membership:

- John Smith → IT Support
- Sarah Johnson → HR

**Account Administration Practice**

Used Active Directory Users and Computers (ADUC) to inspect and manage user accounts.

Practised common account administration tasks:

- Resetting user passwords.
- Enabling and disabling user accounts.
- Reviewing account lockout settings.
- Checking account expiration and logon restrictions.
- Reviewing password-change requirements at next logon.
- Inspecting user group membership.

### 5. Windows 11 Client Deployment and Domain Joining

- Created a Windows 11 Pro client virtual machine named `EDU-CL03`.
- Connected the client to the `EDU-LAB-INT` Internal Virtual Switch.
- Configured static IPv4 addressing.
- Configured the preferred DNS server to point to the Domain Controller (`192.168.10.10`).
- Started both virtual machines simultaneously.
- Successfully tested network connectivity between EDU-CL03 and EDU-DC01 using ICMP ping.
- Successfully joined the Windows 11 client to the `ad.lab` domain.
- Confirmed the successful domain join.
- Verified domain membership using PowerShell:

```powershell
Get-CimInstance Win32_ComputerSystem | Select-Object Name, Domain, PartOfDomain
```

**Domain Authentication Testing**

Successfully tested domain authentication on EDU-CL03 using:

- `AD\jsmith`
- `AD\sjohnson`

Both domain accounts successfully logged into the Windows 11 client.

Practised changing the initial password when prompted during the first domain login.

### 6. Host Memory Investigation and Hyper-V Optimization

During testing, memory pressure was observed when running both virtual machines simultaneously.

An investigation revealed that Windows 11 was reporting approximately 8.3 GB of hardware-reserved memory despite the host having 32 GB of installed RAM.

Inspected the Lenovo ThinkPad BIOS and located the AMD UMA Framebuffer Size setting, which was configured to Auto.

Changed the setting to 2 GB and restarted Windows.

**Results:**

- Previously available to Windows: approximately 23.7 GB.
- Available to Windows after the BIOS change: approximately 29 GB.
- Memory recovered: approximately 5.3 GB.
- Both virtual machines now run with 4 GB RAM allocated each.
- Approximately 9 GB of memory remained available while both VMs were running.

This investigation provided practical experience with integrated graphics memory allocation, hardware-reserved RAM, BIOS configuration, and virtualization resource management.

### 7. DNS Troubleshooting – Forward and Reverse Lookup

During the initial DNS troubleshooting, `nslookup` repeatedly timed out when querying the Domain Controller, even though other DNS tests were successful.

**Initial Findings**

- `Resolve-DnsName` successfully resolved `ad.lab` to `192.168.10.10`.
- The Active Directory LDAP SRV record resolved successfully.
- `dcdiag /test:dns` passed.
- `nslookup` displayed the DNS server as `Unknown` and reported timeouts.
- `nslookup -vc` successfully completed the query over TCP.

**Investigation and Resolution**

Further investigation revealed that no reverse lookup zone had been configured.

A new IPv4 Reverse Lookup Zone was created in DNS Manager:

| Setting | Configuration |
|---|---|
| Network ID | `192.168.10` |
| Reverse lookup zone | `10.10.168.192.in-addr.arpa` |
| Dynamic updates | Secure only |

A Pointer (PTR) record was then created:

| Setting | Value |
|---|---|
| Host IP address | `192.168.10.10` |
| Host name | `EDU-DC01.ad.lab` |

**Verification**

After creating the reverse lookup zone and PTR record, the original `nslookup` command completed successfully:

```powershell
nslookup ad.lab 192.168.10.10
```

The DNS server was now identified by its hostname instead of appearing as `Unknown`, and the domain name resolved successfully.

Additional DNS verification was performed from the Windows 11 client, including the Active Directory LDAP service record:

```powershell
Resolve-DnsName _ldap._tcp.dc._msdcs.ad.lab
```

The record successfully returned `EDU-DC01.ad.lab` on port `389`.

**Key Concepts Learned**

- Forward DNS resolution: hostname to IP address (A record).
- Reverse DNS resolution: IP address to hostname (PTR record).
- DNS troubleshooting requires checking both forward and reverse resolution.
- `nslookup` is a useful command for testing DNS resolution and identifying DNS server response issues.
- Active Directory relies on DNS service records to locate Domain Controllers and related services.

### 8. Shared Network Folders and Group-Based Permissions

Created a shared folder structure on the Domain Controller to practise managing network resources and controlling access through Active Directory security groups.

**Folder Structure**

```text
C:\Shares
│
├── IT
│
└── HR
```

**Network Share Configuration**

- Created the root folder `C:\Shares`.
- Created the `IT` and `HR` departmental subfolders.
- Shared the root folder using the share name `Shares`.
- Configured share permissions for the `Domain Users` group with Read and Change permissions.
- Left Full Control unchecked.
- Used NTFS permissions to manage access to the individual departmental folders.

**IT Folder – NTFS Permissions**

Configured the IT folder's NTFS permissions:

- Disabled permission inheritance.
- Converted inherited permissions into explicit permissions.
- Added the `IT Support` Global Security Group.
- Granted the group Modify permissions.
- Configured permissions to apply to the folder, subfolders and files.
- Removed the broad `AD\Users` permission entries.
- Preserved SYSTEM and Administrators permissions.

**HR Folder – NTFS Permissions**

Configured the HR folder independently to restrict access to the HR department:

- Disabled permission inheritance.
- Converted inherited permissions into explicit permissions.
- Inspected the inherited `AD\Users` permission entries.
- Identified broad read permissions and permissions allowing users to create files and folders.
- Removed both broad `AD\Users` permission entries.
- Added the `HR` Global Security Group.
- Granted the group Modify permissions.
- Configured permissions to apply to the folder, subfolders and files.
- Preserved SYSTEM, Administrators and CREATOR OWNER entries.

**Practical Access Testing**

Tested departmental access from the Windows 11 client (`EDU-CL03`) using both domain accounts.

| User | Department | IT Folder | HR Folder |
|---|---|---|---|
| John Smith (`AD\jsmith`) | IT Support | Allowed | Denied |
| Sarah Johnson (`AD\sjohnson`) | HR | Denied | Allowed |

**IT Access Verification**

Logged into EDU-CL03 as John Smith (`AD\jsmith`).

Successfully accessed:

```text
\\EDU-DC01\Shares\IT
```

Created and accessed the test file:

```text
IT-Access-Test.txt
```

John successfully retained access to the IT folder after the HR permission changes.

**HR Access Verification**

Logged into EDU-CL03 as Sarah Johnson (`AD\sjohnson`).

Successfully accessed:

```text
\\EDU-DC01\Shares\HR
```

Created and opened the test file:

```text
HR-Sarah-Access-Test.txt
```

Sarah successfully retained access to the HR folder after the permission changes.

**Departmental Access Restriction Testing**

Additional tests confirmed that departmental access was correctly restricted:

- Sarah Johnson received a permission error when attempting to access the IT folder.
- John Smith received a permission error when attempting to access the HR folder.

All four departmental access tests produced the expected results.

**Key Concepts Learned**

- The difference between share permissions and NTFS permissions.
- How to configure shared network folders in Windows Server.
- How to apply NTFS permissions to Active Directory security groups.
- The purpose of Modify permissions when managing shared resources.
- How permission inheritance affects access to files and folders.
- How to disable inheritance and convert inherited permissions into explicit permissions.
- Why broad permissions such as `AD\Users` can create unintended access.
- How to use security groups to manage departmental resource access.
- How to apply the principle of least privilege.
- How to test network resource access using different domain user accounts.
- Why practical access testing is important when troubleshooting permissions.

**Result**

Successfully implemented and verified departmental access control using Active Directory security groups and NTFS permissions.

John Smith can access and modify IT resources but cannot access HR resources.

Sarah Johnson can access and modify HR resources but cannot access IT resources.

The departmental access configuration was successfully tested from the domain-joined Windows 11 client.

---

## Additional Key Concepts Learned

Add these bullets to the existing `Key Concepts Learned` section:

- How to configure departmental folder permissions independently of their parent folder.
- How to troubleshoot unintended access caused by inherited NTFS permissions.
- How to distinguish permission configuration from actual permission verification.
- How to use UNC paths to access shared network resources.
- The purpose of User Account Control (UAC) when performing administrative tasks.
- Why standard domain user accounts should not automatically have administrative privileges.

## Next Steps

The lab is ongoing. Planned activities include:

1. Practise additional Active Directory administration and troubleshooting tasks.
2. Create a dedicated administrative account for lab administration.
3. Understand the differences between standard domain users, local administrators and Domain Admins.
4. Practise UAC elevation using an authorized administrator account.
5. Explore additional user, group and computer administration tasks.
6. Document further findings and implementation milestones.

Additional screenshots and documentation will be added as these milestones are completed.

---

**Lab Status:** In Progress

**Completed Milestone:** Shared Network Folders and Departmental Group-Based Permissions.

**Focus:** Practical IT Support / Help Desk and Junior System Administrator skills.
