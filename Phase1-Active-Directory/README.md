# Phase 1 — Active Directory & Domain Controller

## Objective
Install Active Directory Domain Services and promote Windows 
Server 2022 to a Domain Controller for domain lab.local.

## Environment
- Server Name: DC01
- Static IP: 192.168.1.10
- Domain: lab.local
- NetBIOS Name: LAB

## Steps Completed

### 1. Set Static IP
Configured static IP to ensure the Domain Controller is always 
reachable at a consistent address — required for DNS and AD stability.
```powershell
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.1.10 `
-PrefixLength 24 -DefaultGateway 192.168.1.1
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 127.0.0.1
```

### 2. Renamed Server
```powershell
Rename-Computer -NewName "DC01" -Restart
```

### 3. Installed AD DS Role
```powershell
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```

### 4. Promoted to Domain Controller
```powershell
Install-ADDSForest -DomainName "lab.local" -DomainNetbiosName "LAB" `
-InstallDns:$true -Force:$true
```

## Verification
```powershell
Get-ADDomain
Get-WindowsFeature DNS
```

## Key Concepts Learned
- Why Domain Controllers require static IPs
- AD DS role installation and domain promotion
- DNS auto-configuration during DC promotion
- DSRM password purpose and usage

## Troubleshooting Notes
- Missed the "Press any key to boot from CD" prompt on first 
  attempt — restarted VM and caught it second try
- DSRM password is separate from Administrator login password
