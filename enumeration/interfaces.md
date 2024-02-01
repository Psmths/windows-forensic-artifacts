# Interfaces Registry Key
The `Interfaces` registry key will provide information regarding the systems attached network interface adatpers, such as IP address and MAC address. 

### Analysis Value
 - [x] Endpoint - Enumeration

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista
 - [x] Windows XP
 - [x] Windows Server 2019
 - [x] Windows Server 2016
 - [x] Windows Server 2012 R2
 - [x] Windows Server 2012
 - [x] Windows Server 2008 R2
 - [x] Windows Server 2008
 - [x] Windows Server 2003 R2
 - [x] Windows Server 2003

## Artifact Location(s)
🔋 Live System:
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\{INTERFACE_GUID}`

🔌 Offline system:
- File: `%SystemRoot%\System32\config\SYSTEM`
- Key: `SYSTEM\{CURRENT_CONTROL_SET}\Services\Tcpip\Parameters\Interfaces\{INTERFACE_GUID}`

> [!NOTE]
> More information on [{CURRENT_CONTROL_SET}](/enumeration/select.md)

> [!NOTE]
> More information on [{INTERFACE_GUID}](/enumeration/network-cards.md)

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
Each interface will have its own dedicated registry key, and may contain the following values of interest:

| value               | type      | information                                                          |
| ------------------- | --------- | -------------------------------------------------------------------- |
| DhcpDomain          | REG_SZ    | DHCP option 15 - the domain name of the endpoints FQDN               |
| DhcpIPAddress       | REG_SZ    | The DHCP - provided IP address of the endpoint                       |
| DhcpServer          | REG_SZ    | The DHCP server that provided the endpoint its network configuration |
| EnableDHCP          | REG_DWORD | 0x0 if DHCP is disabled and 0x1 if DHCP is enabled                   |
| LeaseObtainedTime   | REG_DWORD | FILETIME timestamp of when the endpoint received a DHCP lease        |
| LeaseTerminatesTime | REG_DWORD | FILETIME timestamp of when the endpoint's DHCP lease expires    |

## Example
```
PS> Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\{a7d8885d-10c1-43d4-9e1e-0a7b2678f020}" -Name *

EnableDHCP                 : 1
Domain                     :
NameServer                 :
DhcpServer                 : 10.100.0.1
Lease                      : 172800
LeaseObtainedTime          : 1687622031
T1                         : 1687708431
T2                         : 1687773231
LeaseTerminatesTime        : 1687794831
AddressType                : 0
IsServerNapAware           : 0
DhcpConnForceBroadcastFlag : 0
IPAddress                  : {}
SubnetMask                 : {}
DefaultGateway             : {}
DefaultGatewayMetric       : {}
RegistrationEnabled        : 1
RegisterAdapterName        : 0
DhcpInterfaceOptions       : {252, 0, 0, 0...}
DhcpDefaultGateway         : {10.100.0.1}
DhcpNameServer             : 10.100.0.10 10.100.0.10
DhcpSubnetMaskOpt          : {255.255.0.0}
DhcpIPAddress              : 10.100.65.234
DhcpSubnetMask             : 255.255.0.0
DhcpGatewayHardware        : {10, 100, 0, 1...}
DhcpGatewayHardwareCount   : 1
```

Correlating with the [NetworkCards](/enumeration/network-cards.md) registry key:
```
PS> Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkCards\" -Name *

ServiceName  : {A7D8885D-10C1-43D4-9E1E-0A7B2678F020}
Description  : Intel(R) Wi-Fi 6 AX200 160MHz
PSPath       : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkCards\5
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>