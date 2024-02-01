# ComputerName Registry Key
The `ComputerName` registry key will provide the Computer Name of the endpoint. 

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
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName`

🔌 Offline system:
- File: `%SystemRoot%\System32\config\SYSTEM`
- Key: `SYSTEM\{CURRENT_CONTROL_SET}\Control\ComputerName\ComputerName`

> [!NOTE]
> More information on [{CURRENT_CONTROL_SET}](/enumeration/select.md)

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
The `ComputerName` value's data field will provide the system's configured Computer Name. 

## Example
```
PS> Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName" -Name *

ComputerName : HLPC01
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>