# TimeZoneInformation Registry Key
The `TimeZoneInformation` registry key provides the current system time zone. This is useful for consolidating separate artifacts found on a system to align with one time zone, such as UTC. 

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
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation`

🔌 Offline system:
- File: `%SystemRoot%\System32\config\SYSTEM`
- Key: `SYSTEM\{CURRENT_CONTROL_SET}\Control\TimeZoneInformation`

> [!NOTE]
> More information on [{CURRENT_CONTROL_SET}](/enumeration/select.md)

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
Within the `TimeZoneInformation` registry key, the value name `TimeZoneKeyName` will contain the current system time zone. 

For examples of what this may look like, execute the command `Get-TimeZone -ListAvailable` in PowerShell and look at the `Id` key. 

The `Bias` key contains the numer of minutes between UTC and the system's selected time zone, such that `UTC = Local System Time + Bias`.

## Example
```
PS> Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\TimeZoneInformation" -Name *

Bias                        : 360
DaylightBias                : 4294967236
DaylightName                : @tzres.dll,-161
DaylightStart               : {0, 0, 3, 0...}
StandardBias                : 0
StandardName                : @tzres.dll,-162
StandardStart               : {0, 0, 11, 0...}
TimeZoneKeyName             : Central Standard Time
DynamicDaylightTimeDisabled : 0
ActiveTimeBias              : 300
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>