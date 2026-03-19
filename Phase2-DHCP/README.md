# Phase 2 — DHCP Configuration

## Objective
Install and configure DHCP server to automatically assign IP 
addresses to client machines joining the domain.

## DHCP Scope Design

| Setting | Value |
|---|---|
| Scope Name | Lab Network |
| Start Range | 192.168.1.100 |
| End Range | 192.168.1.200 |
| Subnet Mask | 255.255.255.0 |
| DNS Server | 192.168.1.10 (DC01) |
| Default Gateway | 192.168.1.1 |
| Excluded Range | 192.168.1.1 - 192.168.1.99 |

## Steps Completed

### 1. Installed DHCP Role
```powershell
Install-WindowsFeature -Name DHCP -IncludeManagementTools
```

### 2. Authorized DHCP in Active Directory
Active Directory requires DHCP servers to be authorized before 
handing out IPs — prevents rogue DHCP servers on the network.
```powershell
Add-DhcpServerInDC -DnsName "DC01.lab.local" -IPAddress 192.168.1.10
```

### 3. Created DHCP Scope
```powershell
Add-DhcpServerv4Scope -Name "Lab Network" `
-StartRange 192.168.1.100 -EndRange 192.168.1.200 `
-SubnetMask 255.255.255.0 -State Active
```

### 4. Configured Scope Options
```powershell
Set-DhcpServerv4OptionValue -ScopeId 192.168.1.0 -Router 192.168.1.1
Set-DhcpServerv4OptionValue -ScopeId 192.168.1.0 -DnsServer 192.168.1.10
Set-DhcpServerv4OptionValue -ScopeId 192.168.1.0 -DnsDomain "lab.local"
```

### 5. Created Exclusion Range
Reserved .1 through .99 for servers and infrastructure devices.
```powershell
Add-DhcpServerv4ExclusionRange -ScopeId 192.168.1.0 `
-StartRange 192.168.1.1 -EndRange 192.168.1.99
```

## Key Concepts Learned
- Why DHCP authorization is required in AD environments
- Importance of exclusion ranges for server IPs
- How DHCP options push DNS and gateway info to clients
- Difference between dynamic and static IP assignment
