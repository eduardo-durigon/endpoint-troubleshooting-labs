# Lab 10 – Introduction to Active Directory

## Overview

This lab focuses on deploying and configuring a Windows Server 2025 environment with Active Directory Domain Services (AD DS).

The objective is to gain practical experience with Active Directory administration, Domain Controllers, DNS, Organizational Units, user and group management, and Windows domain authentication.

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

### 5. Windows 11 Client Deployment

- Created a Windows 11 Pro client virtual machine named `EDU-CL03`.
- Connected the client to the `EDU-LAB-INT` Internal Virtual Switch.
- Configured static IPv4 addressing.
- Configured the preferred DNS server to point to the Domain Controller (`192.168.10.10`).
- Started both virtual machines simultaneously.
- Successfully tested network connectivity between EDU-CL03 and EDU-DC01 using ICMP ping.

The client is communicating with the Domain Controller over the isolated virtual network. Domain joining is still pending.

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

**Key Concepts Learned**

- Forward DNS resolution: hostname to IP address (A record).
- Reverse DNS resolution: IP address to hostname (PTR record).
- DNS troubleshooting requires checking both forward and reverse resolution.
- `nslookup` is a useful command for testing DNS resolution and identifying DNS server response issues.

## Screenshots

The following screenshots document the main implementation milestones completed so far.

### 01 – Hyper-V Virtual Machine Configuration

### 02 – Windows Server 2025 Installation

### 03 – Initial Server Configuration

### 04 – Active Directory Domain Controller

### 05 – Active Directory Users and Security Groups

### 06 – Windows 11 Client Virtual Machine

### 07 – Host Memory Investigation and BIOS Configuration

### 08 – Network Connectivity and DNS Troubleshooting

## Key Concepts Learned

- The difference between installing the AD DS role and promoting a server to a Domain Controller.
- The purpose of a Domain Controller within an Active Directory environment.
- The relationship between Active Directory and DNS.
- The purpose of a forest and domain in Active Directory.
- The role of the Global Catalog.
- The importance of static IP addressing for a Domain Controller.
- The purpose of an isolated Hyper-V Internal Virtual Switch.
- The role of Organizational Units in organizing Active Directory objects.
- The difference between user accounts, Organizational Units, and security groups.
- How Global Security Groups organize users for future resource access management.
- How administrators manage user account settings and password resets.
- How integrated graphics can reserve system RAM.
- The purpose of UMA Framebuffer Size in BIOS.
- How to investigate memory utilization when running multiple virtual machines.
- The difference between ICMP connectivity, TCP connectivity, and DNS resolution.
- The importance of UDP port 53 for standard DNS queries.
- How forward DNS resolution maps hostnames to IP addresses using A records.
- How reverse DNS resolution maps IP addresses to hostnames using PTR records.
- The purpose of reverse lookup zones in DNS.
- How to use `nslookup`, `Resolve-DnsName`, and `dcdiag` to investigate DNS issues.
- How to troubleshoot DNS services and network communication between domain machines.

## Next Steps

The lab is ongoing. Planned activities include:

1. Verify DNS resolution from the Windows 11 client.
2. Join the Windows 11 client to the `ad.lab` domain.
3. Test domain authentication using the John Smith and Sarah Johnson accounts.
4. Practise basic domain login troubleshooting.
5. Configure a shared resource and explore group-based permissions.
6. Practise additional Active Directory administration tasks.

Additional screenshots and documentation will be added as these milestones are completed.

---

**Lab Status:** In Progress

**Focus:** Practical IT Support / Help Desk and Junior System Administrator skills.
