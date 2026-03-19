# Phase 3 — Users, Groups & OUs

## Objective
Build an Organizational Unit structure mirroring a K-12 school 
district, create user accounts individually and via bulk import, 
and configure security groups.

## OU Structure
```
ISD/
├── Staff/
│   ├── Teachers/
│   └── Admins/
└── Students/
```

## Steps Completed

### 1. Created OU Structure
```powershell
New-ADOrganizationalUnit -Name "ISD" -Path "DC=lab,DC=local"
New-ADOrganizationalUnit -Name "Staff" -Path "OU=ISD,DC=lab,DC=local"
New-ADOrganizationalUnit -Name "Students" -Path "OU=ISD,DC=lab,DC=local"
New-ADOrganizationalUnit -Name "Teachers" -Path "OU=Staff,OU=ISD,DC=lab,DC=local"
New-ADOrganizationalUnit -Name "Admins" -Path "OU=Staff,OU=ISD,DC=lab,DC=local"
```

### 2. Created Individual Users
```powershell
New-ADUser -Name "John Smith" -SamAccountName "jsmith" `
-UserPrincipalName "jsmith@lab.local" `
-Path "OU=Teachers,OU=Staff,OU=ISD,DC=lab,DC=local" `
-AccountPassword (ConvertTo-SecureString "P@ssword123!" -AsPlainText -Force) `
-Enabled $true
```

### 3. Bulk User Creation From CSV
Created users.csv with FirstName, LastName, Username, Role columns
then ran bulk import script routing users to correct OUs based on role.

### 4. Created Security Groups
```powershell
New-ADGroup -Name "KISD-Teachers" -GroupScope Global `
-GroupCategory Security -Path "OU=Staff,OU=ISD,DC=lab,DC=local"
```

## Users Created

| Name | Username | Role | OU |
|---|---|---|---|
| John Smith | jsmith | Teacher | Teachers |
| Jane Doe | jdoe | Student | Students |
| Sarah Johnson | sjohnson | Teacher | Teachers |
| Mike Williams | mwilliams | Teacher | Teachers |
| Emily Brown | ebrown | Student | Students |
| Chris Davis | cdavis | Student | Students |
| Ashley Miller | amiller | Student | Students |

## Troubleshooting Notes
- Encountered naming conflict when creating Teachers group — 
  OU named Teachers already existed. Resolved by prefixing 
  groups with KISD- following real district naming conventions
- Always verify OU paths with Get-ADOrganizationalUnit -Filter * 
  before running user creation scripts

## Key Concepts Learned
- OU design for role-based organization
- Bulk provisioning via CSV — real world approach
- Security group scope types (Global, DomainLocal, Universal)
- Naming convention best practices to avoid conflicts
