# Lab 10 – Introduction to Active Directory

## Overview

This lab focuses on deploying and configuring a Windows Server 2025 environment with Active Directory Domain Services (AD DS).

The objective is to gain practical experience with Active Directory administration, Domain Controllers, DNS, Organizational Units, user and group management, Windows domain authentication, shared network resources, permissions, and basic account troubleshooting.

The environment was built using Hyper-V on a Windows 11 Pro host.

This project develops practical skills relevant to Tier 1 IT Support / Help Desk and future Junior System Administrator roles.

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
- Practise domain authentication and account administration.
- Troubleshoot network connectivity and DNS resolution.
- Configure shared network folders.
- Understand the difference between share permissions and NTFS permissions.
- Apply group-based access permissions to shared resources.
- Test access using different domain user accounts.
- Create and use a dedicated domain administrator account.
- Understand User Account Control (UAC) and administrative elevation.
- Practise basic domain account troubleshooting.

## Lab Environment

| Component | Configuration |
|---|---|
| Hyper-V Host | EDU-LAB-CL02 |
| Host OS | Windows 11 Pro |
| Processor | AMD Ryzen AI 7 PRO 450 |
| Installed RAM | 32 GB |
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

| Device | Hostname | IP Address | Subnet Mask |
|---|---|---|---|
| Hyper-V Host | EDU-LAB-CL02 | 192.168.10.1 | 255.255.255.0 |
| Domain Controller | EDU-DC01 | 192.168.10.10 | 255.255.255.0 |
| Windows Client | EDU-CL03 | 192.168.10.20 | 255.255.255.0 |

**Domain Controller Network Configuration**

- Static IPv4 address: `192.168.10.10`
- Preferred DNS server: `192.168.10.10`
- Default gateway: Not configured
- DHCP: Disabled

The Domain Controller uses its own IP address for DNS services.

The Internal Virtual Switch allows communication between the host and connected virtual machines. Internet access has not been configured for this isolated network.

---

# Implementation and Completed Tasks

## 1. Hyper-V Configuration

- Created a dedicated Internal Virtual Switch named `EDU-LAB-INT`.
- Configured the host's virtual network adapter with a static IPv4 address.
- Established the network addressing plan for the lab.
- Connected both virtual machines to the internal network.

## 2. Windows Server Installation

- Created a Generation 2 virtual machine named `EDU-DC01`.
- Installed Windows Server 2025 Standard Evaluation with Desktop Experience.
- Configured static IPv4 addressing.
- Configured the server hostname as `EDU-DC01`.

## 3. Active Directory Deployment

- Installed the Active Directory Domain Services role.
- Created a new Active Directory forest.
- Configured the domain name `ad.lab`.
- Enabled DNS Server and Global Catalog.
- Promoted the server to a Domain Controller.
- Successfully restarted and logged into the Domain Controller.
- Verified the Active Directory domain configuration.

## 4. Active Directory User and Group Management

### Organizational Units

Created a structured Organizational Unit (OU) hierarchy under `Company`:

- `Users`
- `Computers`
- `Groups`
- `Departments`

### Domain User Accounts

Created the following domain user accounts under `Company\Users`:

- John Smith (`jsmith`)
- Sarah Johnson (`sjohnson`)

### Security Groups

Created the following Global Security Groups under `Company\Groups`:

- `IT Support`
- `HR`

Configured group membership:

- John Smith → IT Support
- Sarah Johnson → HR

### Account Administration Practice

Used Active Directory Users and Computers (ADUC) to inspect and manage domain accounts.

Practised:

- Resetting a user's password.
- Enabling and disabling user accounts.
- Configuring the requirement to change a password at the next logon.
- Inspecting user group membership.
- Verifying account status in Active Directory.
- Understanding the difference between disabling an account and unlocking an account.

## 5. Windows 11 Client Deployment and Domain Joining

- Created a Windows 11 Pro client virtual machine named `EDU-CL03`.
- Connected the client to the `EDU-LAB-INT` Internal Virtual Switch.
- Configured static IPv4 addressing.
- Configured the preferred DNS server to point to the Domain Controller (`192.168.10.10`).
- Started both virtual machines simultaneously.
- Successfully tested network connectivity between EDU-CL03 and EDU-DC01 using ICMP ping.
- Successfully joined the Windows 11 client to the `ad.lab` domain.
- Confirmed successful domain membership.

Verified domain membership using PowerShell:

```powershell
Get-CimInstance Win32_ComputerSystem | Select-Object Name, Domain, PartOfDomain
```

### Domain Authentication Testing

Successfully tested domain authentication on EDU-CL03 using:

- `AD\jsmith`
- `AD\sjohnson`

Both domain accounts successfully logged into the Windows 11 client.

Practised changing the initial password when prompted during the first domain login.

## 6. Host Memory Investigation and Hyper-V Optimization

During testing, memory pressure was observed when running both virtual machines simultaneously.

An investigation revealed that Windows 11 was reporting approximately 8.3 GB of hardware-reserved memory despite the host having 32 GB of installed RAM.

Inspected the Lenovo ThinkPad BIOS and located the AMD UMA Framebuffer Size setting, which was configured to Auto.

Changed the setting to 2 GB and restarted Windows.

**Results:**

- Previously available to Windows: approximately 23.7 GB.
- Available to Windows after the BIOS change: approximately 29 GB.
- Memory recovered: approximately 5.3 GB.
- Both virtual machines run with 4 GB RAM allocated each.
- Approximately 9 GB of memory remained available while both VMs were running.

This investigation provided practical experience with integrated graphics memory allocation, hardware-reserved RAM, BIOS configuration, and virtualization resource management.

## 7. DNS Troubleshooting – Forward and Reverse Lookup

During initial DNS troubleshooting, `nslookup` repeatedly timed out when querying the Domain Controller, even though other DNS tests were successful.

### Initial Findings

- `Resolve-DnsName` successfully resolved `ad.lab` to `192.168.10.10`.
- The Active Directory LDAP SRV record resolved successfully.
- `dcdiag /test:dns` passed.
- `nslookup` displayed the DNS server as `Unknown` and reported timeouts.
- `nslookup -vc` successfully completed the query over TCP.

### Investigation and Resolution

Further investigation revealed that no reverse lookup zone had been configured.

Created a new IPv4 Reverse Lookup Zone in DNS Manager:

| Setting | Configuration |
|---|---|
| Network ID | `192.168.10` |
| Reverse lookup zone | `10.10.168.192.in-addr.arpa` |
| Dynamic updates | Secure only |

Created a Pointer (PTR) record:

| Setting | Value |
|---|---|
| Host IP address | `192.168.10.10` |
| Host name | `EDU-DC01.ad.lab` |

### Verification

After creating the reverse lookup zone and PTR record, the original `nslookup` command completed successfully:

```powershell
nslookup ad.lab 192.168.10.10
```

The DNS server was identified by its hostname instead of appearing as `Unknown`, and the domain name resolved successfully.

Additional DNS verification was performed from the Windows 11 client, including the Active Directory LDAP service record:

```powershell
Resolve-DnsName _ldap._tcp.dc._msdcs.ad.lab
```

The record successfully returned `EDU-DC01.ad.lab` on port `389`.

### Key Concepts Learned

- Forward DNS resolution: hostname to IP address (A record).
- Reverse DNS resolution: IP address to hostname (PTR record).
- DNS troubleshooting requires checking both forward and reverse resolution.
- `nslookup` is useful for testing DNS resolution and identifying DNS server response issues.
- Active Directory relies on DNS service records to locate Domain Controllers and related services.

## 8. Shared Network Folders and Group-Based Permissions

Created a shared folder structure on the Domain Controller to practise managing network resources and controlling access through Active Directory security groups.

### Folder Structure

```text
C:\Shares
│
├── IT
│
└── HR
```

### Network Share Configuration

- Created the root folder `C:\Shares`.
- Created the `IT` and `HR` departmental subfolders.
- Shared the root folder using the share name `Shares`.
- Configured share permissions for the `Domain Users` group with Read and Change permissions.
- Left Full Control unchecked.
- Used NTFS permissions to manage access to the individual departmental folders.

### IT Folder – NTFS Permissions

- Disabled permission inheritance.
- Converted inherited permissions into explicit permissions.
- Added the `IT Support` Global Security Group.
- Granted the group Modify permissions.
- Configured permissions to apply to the folder, subfolders and files.
- Removed the broad `AD\Users` permission entries.
- Preserved SYSTEM and Administrators permissions.

### HR Folder – NTFS Permissions

- Disabled permission inheritance.
- Converted inherited permissions into explicit permissions.
- Inspected the inherited `AD\Users` permission entries.
- Identified broad read permissions and permissions allowing users to create files and folders.
- Removed both broad `AD\Users` permission entries.
- Added the `HR` Global Security Group.
- Granted the group Modify permissions.
- Configured permissions to apply to the folder, subfolders and files.
- Preserved SYSTEM, Administrators and CREATOR OWNER entries.

### Practical Access Testing

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

### Departmental Access Restriction Testing

Additional tests confirmed that departmental access was correctly restricted:

- Sarah Johnson received a permission error when attempting to access the IT folder.
- John Smith received a permission error when attempting to access the HR folder.

All four departmental access tests produced the expected results.

### Key Concepts Learned

- The difference between share permissions and NTFS permissions.
- How to configure shared network folders in Windows Server.
- How to apply NTFS permissions to Active Directory security groups.
- The purpose of Modify permissions when managing shared resources.
- How permission inheritance affects access to files and folders.
- How to disable inheritance and convert inherited permissions into explicit permissions.
- Why broad permissions such as `AD\Users` can create unintended access.
- How to use security groups to manage departmental resource access.
- How to apply the principle of least privilege.
- How to use UNC paths to access shared network resources.
- How to test network resource access using different domain user accounts.

## 9. Dedicated Administrator Account and User Account Control (UAC)

Created a dedicated domain administrator account to practise administrative tasks without using a standard domain user account.

### Administrator Account Configuration

Created the following account under `Company\Users`:

- Display name: Lab Administrator
- Username: `lab.admin`

Added the account to the built-in `Domain Admins` group.

Verified group membership using the account's Member Of tab in Active Directory Users and Computers.

The account retained membership in `Domain Users` as well as `Domain Admins`.

### Administrator Authentication Testing

Signed into the Windows 11 client (`EDU-CL03`) using:

```text
AD\lab.admin
```

Verified the authenticated account using:

```powershell
whoami
```

The command returned:

```text
ad\lab.admin
```

### UAC Elevation Testing

Practised User Account Control (UAC) using both a domain administrator account and a standard domain user account.

**Administrator account test**

- Signed in as `AD\lab.admin`.
- Opened Windows Terminal with administrative privileges.
- Accepted the UAC confirmation prompt.
- Verified the account identity using `whoami`.
- Inspected group and integrity information using `whoami /groups`.

**Standard user test**

- Signed into EDU-CL03 as Sarah Johnson (`AD\sjohnson`).
- Attempted to open Windows Terminal with administrative privileges.
- Windows displayed a credential prompt requesting administrator credentials.
- Supplied the authorized `AD\lab.admin` credentials.
- Successfully opened the elevated PowerShell session.
- Verified that the elevated process was running as `ad\lab.admin`.

### Key Concepts Learned

- The purpose of a dedicated administrator account.
- The difference between standard domain users and Domain Admins.
- How domain-qualified account names work, such as `AD\lab.admin`.
- The purpose of User Account Control (UAC).
- How standard users can request administrative elevation using authorized administrator credentials.
- Why administrative privileges should not be granted unnecessarily.
- The importance of using least privilege for everyday user accounts.

## 10. Domain Account Administration and Basic Troubleshooting

Practised common account administration tasks using Active Directory Users and Computers and the Windows 11 domain client.

### Password Reset

- Reset Sarah Johnson's domain password through ADUC.
- Selected the option requiring the user to change the password at the next logon.
- Signed into EDU-CL03 using the temporary password.
- Successfully completed the password change.
- Verified that Sarah could authenticate using the new password.

### Account Disable and Re-enable

- Disabled Sarah Johnson's domain account through ADUC.
- Verified the disabled status in Active Directory.
- Re-enabled the account.
- Successfully signed into EDU-CL03 again using `AD\sjohnson`.

### Account Verification

Used the following command to verify the signed-in account:

```powershell
whoami
```

The command returned:

```text
ad\sjohnson
```

Also inspected the account's configured group membership through the Member Of tab in ADUC.

### Key Concepts Learned

- How to reset a domain user's password.
- How to require a password change at the next logon.
- How to disable and re-enable a domain account.
- How to verify whether an account is enabled.
- The difference between disabling an account and unlocking an account.
- How to verify the currently authenticated user with `whoami`.
- How to inspect configured group membership in Active Directory Users and Computers.

---

# Additional Key Concepts Learned

Throughout this lab, the following concepts were explored and practised:

- Active Directory forests, domains and Domain Controllers.
- The relationship between Active Directory and DNS.
- Organizational Units and directory organization.
- Domain user accounts and Global Security Groups.
- Domain authentication from a Windows client.
- Static IPv4 addressing and DNS configuration.
- Forward and reverse DNS resolution.
- Active Directory service records (SRV).
- Hyper-V Internal Virtual Switches and isolated lab networks.
- Hardware-reserved memory and virtualization resource management.
- Share permissions versus NTFS permissions.
- Permission inheritance and explicit permissions.
- Group-based access control and least privilege.
- UNC paths and network share troubleshooting.
- Dedicated administrator accounts and administrative elevation.
- User Account Control (UAC).
- Domain password resets and account enablement.
- Basic account troubleshooting and verification.

# Screenshots

The following screenshots provide visual evidence of the lab configuration, implementation and testing.

1. **Hyper-V Virtual Switch and Network Configuration** – The `EDU-LAB-INT` Internal Virtual Switch and its network configuration.
2. **Windows Server 2025 Virtual Machine** – The `EDU-DC01` virtual machine configuration in Hyper-V.
3. **Active Directory Domain Deployment** – The `ad.lab` domain and Domain Controller configuration.
4. **Organizational Units, Users and Security Groups** – The `Company` OU structure, domain users and departmental security groups.
5. **Windows 11 Domain Joining** – Evidence that `EDU-CL03` successfully joined the `ad.lab` domain.
6. **DNS Configuration and Verification** – Reverse lookup zone, PTR record and successful DNS verification.
7. **Shared Folders and NTFS Permissions** – The IT and HR folder structure and their group-based permissions.
8. **Departmental Access Testing** – Successful access to each user's own departmental folder and denied access to the other department's folder.
9. **Dedicated Administrator Account** – The `lab.admin` account showing membership in `Domain Users` and `Domain Admins`.
10. **UAC Administrative Elevation** – The administrator credential prompt for a standard user and successful elevation verified with `whoami`.

# Future Learning

The foundational objectives of this lab have been completed.

Potential future topics will be explored in separate labs or projects:

1. Group Policy Management (GPO).
2. Additional Active Directory user, group and computer administration.
3. PowerShell automation for Active Directory.
4. Advanced permissions and troubleshooting scenarios.
5. Additional domain administration and security practices.

These topics are intentionally outside the scope of this introductory lab.

---

**Lab Status:** Completed

**Completed Milestones:**
- Hyper-V environment and isolated network configuration.
- Windows Server 2025 installation and AD DS deployment.
- Active Directory domain and DNS configuration.
- Domain user, OU and security group management.
- Windows 11 domain joining and authentication.
- DNS troubleshooting and verification.
- Shared network folders and departmental access control.
- Dedicated administrator account and UAC testing.
- Password reset and basic domain account troubleshooting.

**Focus:** Practical Tier 1 IT Support / Help Desk and foundational Junior System Administrator skills.
