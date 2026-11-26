# Lab 11 – Microsoft 365 & Entra ID Administration

## Overview

This project focuses on developing practical Microsoft 365 and Microsoft Entra ID administration skills for entry-level IT Support / Help Desk roles.

The objective is to build and configure a small-business cloud environment, practise everyday administrative tasks, and understand how modern organizations manage cloud identities, authentication, security and access to resources.

The project builds upon the traditional Active Directory experience developed in Lab 10 and introduces cloud-based identity management and Microsoft 365 administration.

## Environment

### Personal Environment

- Computer: EDU-LAB-CL02
- Operating System: Windows 11 Pro
- Subscription: Microsoft 365 Family
- Microsoft Account: Personal Microsoft account
- Applications: Microsoft Outlook and Microsoft OneDrive
- OneDrive Storage: 1 TB

### Business Environment

- Subscription: Microsoft 365 Business Premium
- Tenant Name: EduIT Training Lab
- Initial Domain: `eduittraininglab.onmicrosoft.com`
- Identity Platform: Microsoft Entra ID
- Administrative Access: Dedicated business administrator account
- Authentication: Microsoft Authenticator
- Test Users: John Smith, Sarah Johnson and Michael Torres
- Security Groups: IT Support Group, HR Group and Sales Group

The business environment is separate from the personal Microsoft 365 environment and the on-premises Active Directory lab developed in Lab 10.

## Objectives

- Understand Microsoft 365 personal and business environments.
- Understand Microsoft Entra ID and cloud identity management.
- Understand the differences between traditional Active Directory and cloud identity.
- Create and manage cloud users.
- Configure user properties and account settings.
- Create and manage security groups.
- Understand authentication methods and multifactor authentication (MFA).
- Understand Temporary Access Pass (TAP).
- Explore Conditional Access and Security Defaults.
- Understand administrative roles and least-privilege principles.
- Navigate Microsoft 365 Admin Center and Microsoft Entra admin center.
- Practise common Microsoft Entra ID administration tasks.
- Apply these concepts through realistic administrative assessment tickets.

## Project Structure

### Phase 1 – Microsoft 365 Personal Environment

**Status: Completed**

- [x] Install Microsoft 365 desktop applications.
- [x] Configure Outlook.
- [x] Verify Microsoft account authentication.
- [x] Send and receive test emails.
- [x] Add multiple email accounts.
- [x] Verify access to multiple inboxes.
- [x] Inspect OneDrive account configuration.
- [x] Create a dedicated OneDrive test folder.
- [x] Test local-to-cloud synchronization.
- [x] Verify files through OneDrive web.
- [x] Test cloud-to-local synchronization.

### Phase 2 – Microsoft 365 Business Environment

**Status: Completed**

- [x] Investigate Microsoft 365 Business subscriptions.
- [x] Compare Microsoft Entra ID licensing.
- [x] Investigate Microsoft 365 trials.
- [x] Check Microsoft 365 Developer Program eligibility.
- [x] Compare available sandbox options.
- [x] Select a suitable business test environment.
- [x] Create the EduIT Training Lab business tenant.
- [x] Configure the initial administrative account.
- [x] Configure Microsoft Authenticator.
- [x] Access Microsoft 365 Admin Center.
- [x] Inspect the subscription and available licences.
- [x] Document the business tenant architecture.

### Phase 3 – Microsoft Entra ID Administration

**Status: Completed**

**Core Administration**

- [x] Navigate the Microsoft Entra admin center.
- [x] Understand tenants and cloud identities.
- [x] Understand cloud-only, on-premises and hybrid identities.
- [x] Understand Member and Guest accounts.
- [x] Create and manage cloud users.
- [x] Configure user profile properties.
- [x] Reset user passwords.
- [x] Create security groups.
- [x] Manage group membership.
- [x] Understand administrative roles and permissions.
- [x] Inspect authentication methods and MFA settings.
- [x] Understand Temporary Access Pass (TAP).
- [x] Explore Conditional Access policies and their purpose.
- [x] Compare Entra ID with traditional Active Directory.

**Security and Authentication**

- [x] Inspect Security Defaults.
- [x] Explore authentication methods policies.
- [x] Understand per-user MFA status.
- [x] Understand the purpose of Conditional Access.
- [x] Understand the relationship between cloud authentication and traditional Active Directory authentication.

### Phase 4 – Final Practical Assessment

**Status: Completed**

Five realistic administrative tickets were completed to apply the concepts covered throughout Lab 11.

**Ticket 01 – New Employee Onboarding**

- [x] Create Michael Torres as a cloud user.
- [x] Configure his job title, department and employee ID.
- [x] Verify the account.

**Ticket 02 – Security Group Management**

- [x] Create the Sales Group.
- [x] Configure the group as an Assigned security group.
- [x] Add Michael Torres to the group.
- [x] Verify group membership.

**Ticket 03 – Password Reset**

- [x] Investigate Sarah Johnson's password issue.
- [x] Reset her password.
- [x] Require a password change at next sign-in.
- [x] Understand identity verification and secure communication of temporary credentials.

**Ticket 04 – MFA Troubleshooting**

- [x] Investigate John's Authenticator issue after changing phones.
- [x] Inspect his registered authentication methods.
- [x] Identify the absence of usable authentication methods.
- [x] Generate a one-time Temporary Access Pass with a one-hour lifetime.
- [x] Simulate secure delivery and successful Authenticator re-registration.
- [x] Verify the simulated successful sign-in.

**Ticket 05 – Administrative Roles and Permissions**

- [x] Investigate Sarah's request to manage the HR group.
- [x] Identify Groups Administrator as the relevant broad administrative role.
- [x] Understand the scope and privileges of the role.
- [x] Consider group ownership as a more limited alternative.
- [x] Apply the principle of least privilege.

No additional administrative roles were assigned during this assessment.

## Lab Documentation

Each practical exercise is documented using the following structure:

- **Objective:** What the exercise aims to accomplish.
- **Environment:** Hardware, operating system and services involved.
- **Procedure:** Steps performed during the exercise.
- **Results:** Outcome and verification.
- **Troubleshooting:** Issues encountered and how they were resolved, where applicable.
- **Reflection:** Key concepts learned and practical relevance to IT Support.

Supporting documentation, configuration notes and diagrams are maintained throughout the project.

## Completion Goals

**Status: All objectives completed**

- [x] Establish a personal Microsoft 365 environment.
- [x] Establish a Microsoft 365 business test environment.
- [x] Configure administrative access.
- [x] Navigate Microsoft 365 Admin Center.
- [x] Understand Microsoft Entra ID and cloud identities.
- [x] Practise cloud user administration.
- [x] Create and manage security groups.
- [x] Understand authentication and MFA fundamentals.
- [x] Explore Temporary Access Pass.
- [x] Understand Security Defaults and Conditional Access fundamentals.
- [x] Understand administrative roles and least-privilege principles.
- [x] Complete five practical administrative assessment tickets.
- [x] Document the completed phases and practical exercises.

The focus is practical administrative competence and understanding how a small business manages cloud identities and access to its Microsoft 365 environment, rather than simply collecting certifications or memorizing features.

## Help Desk Troubleshooting – Separate Repository

Realistic end-user troubleshooting simulations are documented separately in the **Help Desk Simulation Labs** repository.

These exercises cover scenarios such as:

- Cloud account access problems.
- Password and MFA troubleshooting.
- Outlook configuration and synchronization issues.
- Mailbox access and permissions.
- OneDrive synchronization problems.
- Microsoft 365 licensing issues.
- Teams and SharePoint access problems.

This separation keeps the current project focused on building and administering the environment, while the dedicated simulation repository demonstrates practical troubleshooting and end-user support skills.

## Progress

| Phase | Description | Status |
|---|---|---|
| Phase 1 | Microsoft 365 Personal Environment | Completed |
| Phase 2 | Microsoft 365 Business Environment | Completed |
| Phase 3 | Microsoft Entra ID Administration | Completed |
| Phase 4 | Final Practical Assessment | Completed |

## Reflection

This project expands upon the practical IT support skills developed through previous Windows, macOS, networking and Active Directory labs.

By working with Microsoft 365 and Microsoft Entra ID, the project introduced cloud-based identity management, authentication and modern business administration concepts.

The practical exercises provided experience creating and managing cloud users, configuring security groups, investigating authentication problems, using Temporary Access Pass and understanding administrative permissions.

The final assessment brought these concepts together through realistic administrative scenarios relevant to an entry-level IT Support / Help Desk position.

**Lab 11 – COMPLETED.**

The next project will build upon this foundation by exploring Microsoft 365 services in greater depth, including Exchange Online, Microsoft Teams, SharePoint and OneDrive administration.
