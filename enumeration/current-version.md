# CurrentVersion Registry Key
The `CurrentVersion` registry key will provide you with the current operating system's version, service pack, and date of installation.

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

## Example
```
PS> Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion" -Name *

SystemRoot                : C:\Windows
BaseBuildRevisionNumber   : 1
BuildBranch               : vb_release
BuildGUID                 : ffffffff-ffff-ffff-ffff-ffffffffffff
BuildLab                  : 19041.vb_release.191206-1406
BuildLabEx                : 19041.1.amd64fre.vb_release.191206-1406
CompositionEditionID      : Enterprise
CurrentBuild              : 19044
CurrentBuildNumber        : 19044
CurrentMajorVersionNumber : 10
CurrentMinorVersionNumber : 0
CurrentType               : Multiprocessor Free
CurrentVersion            : 6.3
EditionID                 : Professional
EditionSubManufacturer    :
EditionSubstring          :
EditionSubVersion         :
InstallationType          : Client
InstallDate               : 1666804042
ProductName               : Windows 10 Pro
ReleaseId                 : 2009
SoftwareType              : System
UBR                       : 3086
PathName                  : C:\Windows
ProductId                 : 00330-80000-00000-AA949
DisplayVersion            : 21H2
RegisteredOwner           : user1
RegisteredOrganization    :
InstallTime               : 133112776429018501
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>