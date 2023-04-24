# CurrentVersion Registry Key
The `CurrentVersion` registry key will provide you with the current operating system's version, service pack, and date of installation. In order to determine what the active `CurrentVersion` is, analyze the [Select](/enumeration/select.md) registry key. 


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
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion`

🔌 Offline system:
- File: `%SystemRoot%\System32\config\SOFTWARE`
- Key: `SOFTWARE\Microsoft\Windows NT\CurrentVersion`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
The `ProductName` value will provide the OS, such as `Windows Server 2019`.

The `ReleaseId` value will provide the version of the specified OS. 

The `InstallDate` is an Epoch timestamp of when the OS was either first installed or received a major update, or was reset. 