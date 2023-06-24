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
🔋 Live System:
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList\{USER_SID}`

🔌 Offline system:
- File: `%SystemRoot%\System32\Config\SOFTWARE`
- Key: `SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList\{USER_SID}`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
The value `ProfileImagePath` of type `REG_EXPAND_SZ` contains the full path to a user's home directory, which will typically be named after the user. 

The value `Sid` of type `REG_BINARY` will contain the binary representation of the user's SID. 

## Example
The following example shows how the username `user` maps to the SID `S-1-5-21-3471133136-2963561160-3931775028-1001`.
```
PS> Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList\S-1-5-21-3471133136-2963561160-3931775028-1001" -Name *

ProfileImagePath                        : C:\Users\user1
Flags                                   : 0
FullProfile                             : 1
State                                   : 256
Sid                                     : {1, 5, 0, 0...}
LocalProfileLoadTimeLow                 : 1077375513
LocalProfileLoadTimeHigh                : 31041204
ProfileAttemptedProfileDownloadTimeLow  : 0
ProfileAttemptedProfileDownloadTimeHigh : 0
ProfileLoadTimeLow                      : 0
ProfileLoadTimeHigh                     : 0
LocalProfileUnloadTimeLow               : 4123181936
LocalProfileUnloadTimeHigh              : 31029421
RunLogonScriptSync                      : 0
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>