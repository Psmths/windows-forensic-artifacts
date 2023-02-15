# Group Membership Registry Key
The Group Membership Registry Key is a registry key that stores information about all the groups a user is a part of. It is located in the Windows registry under a user's profile and contains the group security identifiers (SIDs) and a count of how many groups a user is joined to.

### Analysis Value
 - [x] Account - Group Membership

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
- File: `C:\Users\{USER_HOME_DIRECTORY}\NTUSER.DAT`
- Key: `SOFTWARE\Microsoft\Windows\CurrentVersion\Group Policy\GroupMembership`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
Two distinct values are present within this key. The `Count` REG_DWORD value is a count of how many groups the user is a member of. 

The corresponding Group Security Identifiers (SIDs) are enumerated as REG_SZ values of the format `GroupX`. 