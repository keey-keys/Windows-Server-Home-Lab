# Phase 6 — Backup & Recovery

## Objective
Configure Windows Server Backup, perform a successful backup 
to a separate virtual disk, simulate data loss, and recover 
files from a VHDX backup image.

## Environment
| Component | Details |
|---|---|
| Backup Tool | Windows Server Backup |
| Source Data | C:\ImportantData |
| Backup Target | E:\ (20GB virtual disk) |
| Backup Format | VHDX image |

## Steps Completed

### 1. Installed Windows Server Backup
```powershell
Install-WindowsFeature -Name Windows-Server-Backup -IncludeManagementTools
```

### 2. Created Test Data
```powershell
New-Item -Path "C:\ImportantData" -ItemType Directory
"Staff payroll data Q1 2026" | Out-File "C:\ImportantData\Payroll.txt"
"Student roster 2026" | Out-File "C:\ImportantData\StudentRoster.txt"
"Network configuration backup" | Out-File "C:\ImportantData\NetworkConfig.txt"
```

### 3. Ran Backup
```powershell
$Policy = New-WBPolicy
$FileSpec = New-WBFileSpec -FileSpec "C:\ImportantData"
Add-WBFileSpec -Policy $Policy -FileSpec $FileSpec
$BackupLocation = New-WBBackupTarget -VolumePath "E:"
Add-WBBackupTarget -Policy $Policy -Target $BackupLocation
Start-WBBackup -Policy $Policy
```

### 4. Simulated Disaster
```powershell
Remove-Item -Path "C:\ImportantData" -Recurse -Force
```

### 5. Located and Mounted VHDX
Windows Server Backup stores data as VHDX images not loose files.
Located backup at:
E:\WindowsImageBackup\DC01\Backup 2026-03-19 034637\

Mounted VHDX and assigned drive letter via diskpart:
```powershell
Mount-DiskImage -ImagePath "E:\WindowsImageBackup\DC01\...\backup.vhdx"
# Then assigned drive letter G: via diskpart
```

### 6. Recovered Files
```powershell
New-Item -Path "C:\ImportantData" -ItemType Directory -Force
Copy-Item "G:\ImportantData\*" -Destination "C:\ImportantData\" -Force
```

### 7. Verified Recovery
All 3 files restored with original contents intact.

## Troubleshooting Notes
- Start-WBFileRecovery cmdlet had parameter binding issues — 
  resolved by manually mounting the VHDX and copying files directly
- VHDX mounted without a drive letter — assigned manually via diskpart
- Windows Server Backup stores data as VHDX not individual files — 
  requires mounting the image to access contents

## Key Concepts Learned
- Windows Server Backup policy configuration
- VHDX backup format and manual mounting process
- Diskpart volume management
- Importance of separate backup target drives
- Recovery verification procedures
