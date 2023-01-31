# Group Membership Registry Key
The Group Membership Registry Key is a registry key that stores information about all the groups a user is a part of. It is located in the Windows registry under a user's profile and contains the group security identifiers (SIDs) and a count of how many groups a user is joined to.

### Behavioral Indications
 - [x] Behavioral - Persistence (TA0003)

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
- `HKEY_USERS\{USER_SID}\SOFTWARE\Microsoft\Windows\CurrentVersion\Group Policy\GroupMembership`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
Two distinct values are present within this key. The `Count` REG_DWORD value is a count of how many groups the user is a member of. 

The corresponding Group Security Identifiers (SIDs) are enumerated as REG_SZ values of the format `GroupX`. 