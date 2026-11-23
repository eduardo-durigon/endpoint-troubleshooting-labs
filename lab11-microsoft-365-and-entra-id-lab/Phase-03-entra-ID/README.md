# Phase 03 - Lab 11 – Microsoft Entra ID Administration

## Overview

This lab focuses on Microsoft Entra ID administration and cloud-based identity management using a Microsoft 365 Business Premium environment.

The objective is to develop practical skills relevant to Tier 1 Help Desk, IT Support, and Junior Systems Administrator roles.

The lab explores cloud identities, user and group administration, password resets, administrative roles, authentication methods, Multi-Factor Authentication (MFA), Conditional Access, and the differences between Microsoft Entra ID and traditional on-premises Active Directory.

The environment uses a dedicated Microsoft 365 Business Premium tenant, separate from my personal Microsoft 365 account and my on-premises Active Directory lab.

## Environment

- **Cloud platform:** Microsoft 365
- **Subscription:** Microsoft 365 Business Premium
- **Identity platform:** Microsoft Entra ID
- **Tenant:** EduIT Training Lab
- **Initial domain:** `eduittraininglab.onmicrosoft.com`
- **Administrative account:** Dedicated business administrator account
- **Authentication:** Microsoft Authenticator
- **On-premises comparison:** Windows Server 2025 with Active Directory Domain Services (Lab 10)

## Objectives

- Understand Microsoft 365 tenant administration.
- Explore the Microsoft Entra admin center.
- Understand tenants and cloud-only identities.
- Create and manage cloud user accounts.
- Perform password resets and modify user properties.
- Create and manage security groups.
- Understand administrative roles and least privilege.
- Explore authentication methods and MFA configuration.
- Understand Conditional Access policies and their purpose.
- Compare Microsoft Entra ID with on-premises Active Directory.
- Understand cloud-only, on-premises, and hybrid identity models.
- Develop practical cloud identity administration skills.

## Structure

### Phase 01 – Personal Microsoft 365 Environment

Set up and explored Microsoft 365 using a personal Microsoft account.

Key activities:
- Installed and configured Microsoft 365 desktop applications.
- Signed into Office applications.
- Configured Outlook and tested email functionality.
- Added personal email accounts to Outlook.
- Configured and tested OneDrive synchronization.
- Verified synchronization between local files and cloud storage.

### Phase 02 – Business Microsoft 365 Environment

Created and configured a dedicated Microsoft 365 Business Premium environment.

Key activities:
- Registered a Microsoft 365 Business Premium tenant.
- Created the initial organizational domain.
- Configured the business administrator account.
- Configured Microsoft Authenticator.
- Explored the Microsoft 365 Admin Center.
- Inspected subscription and licence information.
- Explored the relationship between Microsoft 365 administration and Microsoft Entra ID.

### Phase 03 – Microsoft Entra ID Administration

Explored cloud identity management and security administration.

**Exercise 01 – Exploring the Microsoft Entra Admin Center**

- Explored the Microsoft Entra administration interface.
- Located the main administration sections:
  - Users
  - Groups
  - Roles and administrators
  - Authentication methods
  - Conditional Access
- Practised navigating the administration portal.
- Completed a practical knowledge check matching common Help Desk tasks to the appropriate administration sections.

**Exercise 02 – Understanding Tenants and Cloud Identities**

- Explored the Microsoft Entra tenant.
- Understood the purpose of the organizational tenant and its initial domain.
- Distinguished cloud-only identities from on-premises Active Directory accounts.
- Confirmed that the cloud identities created in this lab are independent of the accounts in Lab 10.
- Learned the difference between Member and Guest accounts.
- Explored the concept of hybrid identity and directory synchronization.
- Completed a practical knowledge check covering tenant isolation, cloud-only identities, and hybrid identity.

**Exercise 03 – Creating and Managing Cloud Users**

Created two cloud-only test accounts:

| User | Job Title | Department | Employee ID |
|---|---|---|---|
| John Smith | IT Support Technician | IT | EMP001 |
| Sarah Johnson | HR Assistant | HR | EMP002 |

Configured user properties including:
- Display name and username.
- Job title and department.
- Employee ID and employee type.
- Company name.
- Usage location.

Verified that both accounts appeared in the Entra user directory.

**Exercise 04 – Password Reset and User Properties**

- Practised resetting a cloud user's password.
- Simulated a Tier 1 Help Desk password-reset request.
- Generated a temporary password for the test user.
- Explored user profile and account properties.

**Exercise 05 – Creating and Managing Security Groups**

Created two security groups:

| Group | Membership Type | Members |
|---|---|---|
| IT Support Group | Assigned | John Smith |
| HR Group | Assigned | Sarah Johnson |

- Configured group names and descriptions.
- Added users to their corresponding groups.
- Verified group membership.
- Explored the difference between user department attributes and security group membership.

**Exercise 06 – Administrative Roles and Permissions**

Explored Microsoft Entra administrative roles and their permission boundaries.

Inspected:
- Global Administrator
- User Administrator
- Password Administrator
- Groups Administrator
- Authentication Administrator

Key concepts:
- Role-based access control (RBAC).
- Administrative privilege boundaries.
- Least privilege.
- Separating routine user administration from highly privileged administrative tasks.

Confirmed that the business administrator account had the Global Administrator role. No additional role assignments were made.

**Exercise 07 – Multi-Factor Authentication (MFA)**

Explored Microsoft Entra authentication methods and MFA-related configuration.

Inspected the Authentication Methods policy:

| Authentication Method / Setting | Configuration |
|---|---|
| Microsoft Authenticator | Enabled for all users |
| SMS | Disabled |
| Temporary Access Pass (TAP) | Enabled for all users |
| Authenticator OTP | Enabled |
| Application name in notifications | Enabled for all users |
| Geographic location in notifications | Enabled for all users |
| Authenticator on companion applications | Enabled for all users |

Inspected the legacy Per-user MFA status:

- John Smith: Disabled.
- Sarah Johnson: Disabled.

Inspected Security Defaults and confirmed that it was enabled.

Key concepts:
- Authentication method availability versus user registration.
- MFA enforcement.
- Microsoft Authenticator.
- Temporary Access Pass.
- Security Defaults.
- Differences between legacy Per-user MFA and modern authentication policies.

No authentication or security settings were modified during this exercise.

**Exercise 08 – Conditional Access**

Explored the Conditional Access administration area and identified two existing policy descriptions in the policy list.

Identified the following policy descriptions:

1. **MFA outside the company network**
   - Described as applying to all users.
   - Requires MFA when users are outside the company's trusted network locations.

2. **Device requirements for Managers**
   - Described as applying to users in the Managers group.
   - Requires an Intune-compliant or domain-joined device.

Key concepts:
- Conditional Access policies.
- User and group assignments.
- Network locations.
- MFA requirements.
- Device compliance.
- Access controls.

Also confirmed that Security Defaults was enabled.

The policy details could not be opened, so their individual assignments, grant controls, and enabled status were not independently verified.

No Conditional Access policies were created, edited, or enabled during this exercise.

**Exercise 09 – Microsoft Entra ID vs. On-Premises Active Directory**

Compared the cloud identity platform with the Windows Server Active Directory environment built in Lab 10.

Topics explored:
- Domain Controllers and cloud identity infrastructure.
- Domain accounts and cloud accounts.
- Kerberos and modern cloud authentication protocols.
- Organizational Units and security groups.
- Group Policy and Intune.
- Password resets and account administration.
- Domain-joined and Entra-joined devices.
- Cloud-only, on-premises, and hybrid identity models.
- The impact of internet connectivity on cloud and on-premises authentication.

Completed a practical knowledge check covering common Help Desk scenarios.

## Practical Skills Developed

- Microsoft 365 tenant navigation.
- Microsoft Entra user administration.
- Cloud account creation and management.
- User profile configuration.
- Cloud password resets.
- Security group creation and membership management.
- Understanding administrative roles and permissions.
- Authentication methods and MFA configuration inspection.
- Security Defaults inspection.
- Conditional Access policy inspection.
- Understanding tenant isolation and cloud-only identities.
- Understanding Member and Guest accounts.
- Understanding cloud-only versus hybrid identities.
- Comparing Microsoft Entra ID with on-premises Active Directory.

## Progress

- [x] Phase 01 – Personal Microsoft 365 Environment
- [x] Phase 02 – Business Microsoft 365 Environment
- [x] Exercise 01 – Exploring the Microsoft Entra Admin Center
- [x] Exercise 02 – Understanding Tenants and Cloud Identities
- [x] Exercise 03 – Creating and Managing Cloud Users
- [x] Exercise 04 – Password Reset and User Properties
- [x] Exercise 05 – Creating and Managing Security Groups
- [x] Exercise 06 – Administrative Roles and Permissions
- [x] Exercise 07 – Multi-Factor Authentication (MFA)
- [x] Exercise 08 – Conditional Access
- [x] Exercise 09 – Comparing Entra ID with On-Premises Active Directory

## Final Goal

Build practical experience administering Microsoft 365 and Microsoft Entra ID in a business environment.

This lab complements Lab 10 (Windows Server 2025 and Active Directory), providing a foundation for understanding both traditional on-premises identity management and modern cloud-based identity administration.

Future improvements may include:
- Creating and testing Conditional Access policies in a safe test environment.
- Exploring Microsoft Intune device enrollment and compliance.
- Exploring hybrid identity and directory synchronization.
- Practising Microsoft 365 administration through realistic Help Desk scenarios.
