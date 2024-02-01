# Select Registry Key
The `Select` registry key provides the number of the system's `CurrentControlSet`. The `CurrentControlSet` contains important configuration for the Windows operating system, and several different Control Sets may be available within a system's registry. 

In general, `ControlSet001` will be the most recent Control Set that has been booted under, whereas `ControlSet002` functions as a backup of a known-good state for the Control Set. 

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
- `HKEY_LOCAL_MACHINE\SYSTEM\Select`

🔌 Offline system:
- File: `%SystemRoot%\System32\config\SYSTEM`
- Key: `SYSTEM\Select`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
Within the `Select` key, the value named `Current` identifies the `CurrentControlSet` by an integer. If the value is `1` for instance, that means that the `CurrentControlSet` on a live system will point to `ControlSet001`. 

## Example
In the following example, the `Select` value's data is `1`, indicating that the CurrentControlSet is `ControlSet001`.
```
PS> Get-ItemProperty -Path "HKLM:\SYSTEM\Select" -Name *

Current       : 1
Default       : 1
Failed        : 0
LastKnownGood : 1
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>