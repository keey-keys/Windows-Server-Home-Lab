# Phase 4 — Group Policy

## Objective
Create and link Group Policy Objects to enforce security settings 
and user restrictions across the domain and specific OUs.

## GPOs Created

| GPO Name | Linked To | Purpose |
|---|---|---|
| Password Policy | Domain (lab.local) | Enforce password complexity |
| Student Restrictions | Students OU | Block control panel, disable USB |

## Steps Completed

### 1. Installed GPMC Feature
```powershell
Install-WindowsFeature -Name GPMC -IncludeManagementTools
```

### 2. Created and Linked GPOs via PowerShell
GUI (gpmc.msc) was unavailable due to UTM display limitations — 
configured entirely via PowerShell which reflects enterprise 
scripted deployment practices.
```powershell
New-GPO -Name "Password Policy" -Comment "Domain wide password policy"
New-GPLink -Name "Password Policy" -Target "DC=lab,DC=local"

New-GPO -Name "Student Restrictions"
New-GPLink -Name "Student Restrictions" -Target "OU=Students,OU=ISD,DC=lab,DC=local"
```

### 3. Configured Registry-Based Policy Settings
```powershell
# Block Control Panel for students
Set-GPRegistryValue -Name "Student Restrictions" `
-Key "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" `
-ValueName "NoControlPanel" -Type DWord -Value 1

# Disable USB storage
Set-GPRegistryValue -Name "Student Restrictions" `
-Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\RemovableStorageDevices" `
-ValueName "Deny_All" -Type DWord -Value 1
```

### 4. Verified GPO Inheritance
```powershell
Get-GPInheritance -Target "OU=Students,OU=ISD,DC=lab,DC=local"
```
Result confirmed Students OU inherits 3 policies:
Student Restrictions + Password Policy + Default Domain Policy

## Key Concepts Learned
- GPO inheritance — child OUs inherit parent GPO settings
- Why gpresult shows different results per user context
- Registry-based policy settings via PowerShell
- Difference between Computer Configuration and User Configuration
- How to verify GPO links without the GUI
