# ProfileList Registry Key
The ProfileList Registry Key is a registry key that contains subkeys named by user SIDs present on the endpoint. Within each SID subkey are values that can assist in matching SIDs to usernames during dead-disk forensics. 

### Analysis Value
 - [x] Account - Security Identifier (SID)
 - [x] Account - Username

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista
 - [x] Windows XP

## Artifact Location(s)
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList\{USER_SID}`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
The value `ProfileImagePath` of type `REG_EXPAND_SZ` contains the full path to a user's home directory, which will typically be named after the user. 

The value `Sid` of type `REG_BINARY` will contain the binary representation of the user's SID. 