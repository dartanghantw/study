## Course
https://thoughtworks.udemy.com/course/exam-azure-2/learn/lecture/30727572#overview

### Topics

### Infos

# Access Review (Azure AD)

## What It Is
- **Periodic verification** of user access rights to resources, groups, and applications
- Ensures users only have access they **actually need** (principle of least privilege)
- Part of Azure AD Identity Governance

## Key Purposes
- **Remove stale access** - People who changed roles or left but still have permissions
- **Compliance** - Meet regulatory requirements for access certification
- **Security** - Reduce attack surface by eliminating unnecessary permissions

## How It Works
- **Reviewers** (managers, resource owners, or users themselves) periodically review access
- Can be **one-time** or **recurring** (weekly, monthly, quarterly, annually)
- Reviewers **approve** (keep access) or **deny** (remove access)
- Can be **automated** - Auto-remove access if not approved within timeframe

## Common Scenarios
- Review membership in **Azure AD groups**
- Review access to **enterprise applications**
- Review assignments to **Azure resources** (subscriptions, resource groups)
- Review **privileged roles** (like Global Admin)

## Configuration Options
- Set **reviewers**: specific users, managers, resource owners, or self-review
- Define **scope**: which users/groups to review
- Set **frequency**: how often reviews occur
- Configure **actions**: what happens after review (manual or auto-apply)

## Best Practices
- Schedule reviews for **critical resources** more frequently
- Use **automatic decisions** to deny access if reviewer doesn't respond
- Review **privileged access** quarterly or more often
- Enable **recommendations** to help reviewers make decisions

# Privileged Identity Management (PIM)

## What It Is
- **Just-in-time (JIT)** privileged access to Azure AD and Azure resources
- Users get **temporary elevated permissions** only when needed
- Replaces **permanent admin assignments** with **eligible assignments**

## Core Concept
- Users are **eligible** for roles but don't have them active by default
- Must **activate** the role when needed (time-limited, typically hours)
- Reduces risk by minimizing standing admin privileges

## Key Benefits
- **Zero standing access** - No permanent admins unless absolutely necessary
- **Time-bound access** - Roles expire automatically
- **Approval workflow** - Can require approval before activation
- **Audit trail** - Complete logging of who activated what and when
- **MFA enforcement** - Require MFA to activate privileged roles

## Activation Process
1. User requests role activation in PIM
2. May require **justification** (business reason)
3. May require **approval** from designated approvers
4. May require **MFA** verification
5. Role activates for **specified duration** (e.g., 8 hours)
6. Role **automatically expires** after time limit

## PIM for Azure AD Roles
- Manage privileged Azure AD roles (Global Admin, User Admin, etc.)
- Control who can activate directory-level permissions

## PIM for Azure Resources
- Manage RBAC roles on subscriptions, resource groups, resources
- Control who can activate Owner, Contributor, etc.

## Configuration Options
- **Eligible assignments** - Can activate the role
- **Active assignments** - Permanent (still tracked by PIM)
- **Activation duration** - Max time role stays active (1-24 hours)
- **Approval required** - Yes/no for activation
- **MFA required** - On activation, on assignment, or both
- **Justification required** - Force users to explain why they need access
- **Notifications** - Alert when roles are activated/assigned

## Access Reviews Integration
- PIM includes **built-in access reviews** for privileged roles
- Regularly verify eligible users still need access
- Auto-remove eligibility if not approved

## Best Practices
- Make all admin roles **eligible** (not active) by default
- Require **approval** for highly privileged roles (Global Admin, Owner)
- Set **short activation durations** (2-4 hours for most roles)
- Always require **MFA** for activation
- Enable **alerts** for suspicious activation patterns
- Conduct **quarterly reviews** of eligible assignments

## Common Use Cases
- Dev team needs **temporary Contributor** access to production
- Security admin activates **Global Admin** only for critical changes
- Support staff activates **User Admin** to reset passwords during shift

## Licensing
- Requires **Azure AD Premium P2** license

# Administrative Units (AU)

## What It Is
- **Container for Azure AD objects** (users, groups, devices)
- Allows **delegated administration** to a **subset** of the directory
- Think of it as "departmental boundaries" within Azure AD

## Core Purpose
- **Scope administrative permissions** to specific parts of the organization
- Admin can manage only users/groups **within their AU** (not entire directory)
- Enables **decentralized management** without giving full directory access

## Key Concept
- **Restrict scope** - Admins only see/manage objects in their assigned AU
- Objects can belong to **multiple AUs** simultaneously
- AUs can contain **users, groups, and devices** (not applications)

## Common Scenarios
- **Geographic division** - Separate AUs for US, Europe, Asia offices
- **Departmental** - HR, Finance, IT departments each have their own AU
- **Business unit** - Different subsidiaries or brands
- **School/University** - Different faculties or campuses

## How It Works
1. Create AU (e.g., "Sales Department")
2. Add members (users/groups) to the AU
3. Assign admin roles **scoped to that AU** (e.g., User Admin for Sales AU only)
4. Admin can only manage objects within their assigned AU

## Role Assignment Scoping
- Assign Azure AD roles with **AU scope** instead of directory scope
- Example: "User Administrator" for only the "Marketing AU"
- Supported roles: User Admin, Groups Admin, Helpdesk Admin, Password Admin, etc.

## Member Types
- **Users** - Individual user accounts
- **Groups** - Security and Microsoft 365 groups
- **Devices** - Registered/joined devices

## Key Differences vs Groups
- **Groups** = Collection for permissions/access to resources
- **AUs** = Administrative boundary for delegating management
- Groups grant access; AUs scope admin capabilities

## Restrictions
- **Restricted management AU** - Special type where members can only be managed by Global Admins
- Prevents AU-scoped admins from modifying critical accounts

## Dynamic Membership
- Use **rules** to automatically populate AUs based on user attributes
- Example: All users where `department = "Sales"` → Sales AU
- Similar to dynamic groups but for admin boundaries

## Best Practices
- Use AUs for **large organizations** with decentralized IT
- Align AUs with **organizational structure**
- Use **restricted management AUs** for executive/privileged accounts
- Combine with **PIM** for just-in-time AU admin access
- Document AU structure clearly

## Licensing
- Available in **Azure AD Free** and higher tiers

## Example Use Case
- University has "Engineering Faculty" AU
- Faculty IT admin assigned as **User Administrator** for that AU only
- Can reset passwords, create accounts for engineering students/staff
- **Cannot** access or manage users in "Medical Faculty" AU

# Microsoft Entra Connect (Azure AD Connect)

## What It Is
- **Identity synchronization tool** between on-premises Active Directory and Azure AD (Entra ID)
- Bridges **on-prem AD** with **cloud** for hybrid identity
- Formerly called "Azure AD Connect" (rebranded to Entra Connect)

## Core Purpose
- **Sync users, groups, and passwords** from on-prem AD to Azure AD
- Enable **single identity** across on-premises and cloud resources
- Users sign in with **same credentials** for both environments

## Key Components
- **Sync engine** - Handles object synchronization
- **AD FS (optional)** - Federated authentication
- **Health monitoring** - Azure AD Connect Health service

## Authentication Methods

### 1. Password Hash Synchronization (PHS)
- **Most common** and recommended
- Syncs **hash of password hash** to Azure AD
- Authentication happens **in the cloud**
- Fast, simple, supports seamless SSO
- **Most resilient** - works even if on-prem is down

### 2. Pass-Through Authentication (PTA)
- Authentication happens **on-premises**
- Agent validates passwords against on-prem AD
- No passwords stored in cloud
- Requires on-prem **availability** for sign-in

### 3. Federated Authentication (AD FS)
- Uses **federation server** (AD FS or third-party)
- Most complex, highest maintenance
- Needed for specific scenarios (smart cards, third-party MFA)

## Seamless SSO
- Users **automatically signed in** when on corporate network
- No need to type password for cloud apps
- Works with PHS and PTA

## Sync Features
- **User synchronization** - Accounts from on-prem to cloud
- **Group synchronization** - Security and distribution groups
- **Device synchronization** - Hybrid Azure AD join
- **Attribute filtering** - Choose which attributes to sync
- **OU filtering** - Select specific OUs to sync

## Password Writeback
- Change passwords **in the cloud** → written back to on-prem AD
- Enables **self-service password reset (SSPR)** for hybrid users
- Requires Azure AD Premium P1

## Group Writeback
- Create Microsoft 365 groups in cloud → written back to on-prem AD
- On-prem apps can see cloud-created groups

## Device Options
- **Hybrid Azure AD Join** - Devices joined to both on-prem AD and Azure AD
- Enables seamless access to cloud and on-prem resources

## Filtering Options
- **Domain filtering** - Sync only specific domains
- **OU filtering** - Sync only specific organizational units
- **Attribute filtering** - Exclude certain attributes
- **Group-based filtering** - Sync only members of specific groups (not recommended for production)

## Staging Mode
- Run server in **staging mode** for testing
- Imports and processes data but **doesn't export** to Azure AD
- Useful for disaster recovery setup

## High Availability
- **Staging server** - Secondary server ready to take over
- **Multiple sync servers** - Not active-active (one active, others staging)
- For true HA, combine with PHS (most resilient)

## Common Issues
- **Duplicate objects** - Same object synced from multiple sources
- **Sync errors** - Attribute conflicts, invalid data
- **UPN mismatches** - On-prem UPN doesn't match Azure AD requirements
- **Password sync delays** - Can take up to 2 minutes

## Installation Requirements
- **Windows Server** (2016 or later recommended)
- **.NET Framework** 4.6.2+
- **SQL Server** (Express included, or use full SQL)
- **Global Administrator** credentials for Azure AD
- **Enterprise Admin** credentials for on-prem AD

## Best Practices
- Use **Password Hash Sync** as primary or backup authentication method
- Enable **Seamless SSO** for better user experience
- Implement **staging server** for high availability
- Monitor with **Azure AD Connect Health**
- Keep Entra Connect **up to date** (auto-upgrade recommended)
- Use **selective sync** to only sync necessary objects
- Test changes in **staging mode** first

## Entra Connect Cloud Sync
- **Lightweight alternative** to Entra Connect
- Agent-based (no sync server needed)
- Managed from the cloud
- Better for **multi-forest** scenarios
- Less feature-rich than full Entra Connect

## Licensing
- Basic sync features: **Free**
- Advanced features (password writeback, group writeback): **Azure AD Premium P1/P2**

## Key Difference: Sync vs Federation
- **Sync** - Credentials synchronized, auth can happen in cloud
- **Federation** - Credentials stay on-prem, auth always happens on-prem