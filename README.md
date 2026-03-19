# Windows Server 2022 Home Lab

A fully functional Windows Server home lab built to simulate a K-12 
school district IT environment. Built on macOS using UTM virtualization.

## Lab Overview

| Component | Details |
|---|---|
| OS | Windows Server 2022 Evaluation |
| Virtualization | UTM on macOS Intel |
| Domain | lab.local |
| Domain Controller | DC01 (192.168.1.10) |
| Roles Installed | AD DS, DNS, DHCP, Windows Server Backup |

## Phases

### Phase 1 — Active Directory & Domain Controller
Installed AD DS, promoted server to Domain Controller, 
configured domain lab.local with DNS.
[View Documentation](Phase1-Active-Directory/README.md)

### Phase 2 — DHCP
Installed and authorized DHCP server, created scope, 
configured options for DNS and gateway.
[View Documentation](Phase2-DHCP/README.md)

### Phase 3 — Users, Groups & OUs
Built OU structure mirroring a school district, created users 
manually and via bulk CSV import, configured security groups.
[View Documentation](Phase3-Users-Groups-OUs/README.md)

### Phase 4 — Group Policy
Created and linked GPOs for password policy, student 
restrictions, and drive mapping via PowerShell.
[View Documentation](Phase4-Group-Policy/README.md)

### Phase 5 — PowerShell Automation Scripts
Built 4 automation scripts for inactive account detection, 
account unlocking, disk monitoring, and user audit reporting.
[View Documentation](Phase5-PowerShell-Scripts/README.md)

### Phase 6 — Backup & Recovery
Configured Windows Server Backup, simulated data loss, 
and recovered files from a mounted VHDX backup image.
[View Documentation](Phase6-Backup-Recovery/README.md)

## Skills Demonstrated
- Active Directory administration
- DNS & DHCP configuration
- Group Policy management
- PowerShell scripting and automation
- Backup and disaster recovery
- Windows Server 2022 administration
- Virtualization (UTM)
- Technical documentation

## Related Projects
- [AD Automation Scripts](https://github.com/keey-keys/AD-Automation-Scripts)
- [Server Maintenance Toolkit](https://github.com/keey-keys/Server-Maintenance-Toolkit)

## Purpose
Built as part of an IT portfolio while working as an IT Specialist 
and preparing for Systems Administrator roles in K-12 education.
