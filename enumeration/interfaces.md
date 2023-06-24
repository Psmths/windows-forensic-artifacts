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

> ℹ️ More information on [{CURRENT_CONTROL_SET}](/enumeration/select.md)

> ℹ️ More information on [{INTERFACE_GUID}](/enumeration/network-cards.md)

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
