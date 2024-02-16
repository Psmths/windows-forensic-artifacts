# Group Membership Registry Key
The Group Membership Registry Key is a registry key that stores information about all the groups a user is a part of. It is located in the Windows registry under a user's profile and contains the group security identifiers (SIDs) and a count of how many groups a user is joined to.

### Analysis Value
 - [x] Account - Group Membership
 - [x] Endpoint - Enumeration

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista
 - [x] Windows XP

## Artifact Location(s)
This registry key resides in the NTUSER.DAT file, and on a live system is loaded into HKEY_USERS.

🔋 Live System:
- `HKEY_USERS\{USER_SID}\SOFTWARE\Microsoft\Windows\CurrentVersion\Group Policy\GroupMembership`

🔌 Offline system:
- File: `%UserProfile%\NTUSER.DAT`
- Key: `SOFTWARE\Microsoft\Windows\CurrentVersion\Group Policy\GroupMembership`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
Two distinct values are present within this key. The `Count` REG_DWORD value is a count of how many groups the user is a member of. 

The corresponding Group Security Identifiers (SIDs) are enumerated as REG_SZ values of the format `GroupX`. 

## Example
```
PS> New-PSDrive -PSProvider Registry -Name HKU -Root HKEY_USERS
PS> > Get-ItemProperty -Path "HKU:\S-1-5-21-3471133136-2963561160-3931775028-1001\SOFTWARE\Microsoft\Windows\CurrentVersion\Group Policy\GroupMembership" -Name *

Group0       : S-1-5-21-3471133136-2963561160-3931775028-513
Group1       : S-1-1-0
Group2       : S-1-5-114
Group3       : S-1-5-32-544
Group4       : S-1-5-32-559
Group5       : S-1-5-32-545
Group6       : S-1-5-4
Group7       : S-1-2-1
Group8       : S-1-5-11
Group9       : S-1-5-15
Group10      : S-1-5-113
Group11      : S-1-2-0
Group12      : S-1-5-64-10
Group13      : S-1-16-12288
Count        : 14
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>