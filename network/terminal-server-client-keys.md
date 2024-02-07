# Terminal Server Client Registry Keys
These registry keys store information regarding options selected by a user during the establishment of an RDP session, such as:

 - Ignoring an untrusted certificate
 - Username used to log in to the remote system via RDP

### Analysis Value
 - [x] Network - Evidence of Network Activity

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista

## Artifact Location(s)
🔋 Live System:
- `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Terminal Server Client\Servers`

🔌 Offline system:
- File: `%UserProfile%\NTUSER.DAT`
- Key: `SOFTWARE\Microsoft\Terminal Server Client\Servers`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
Under the `Servers` key there will be additional keys, one for each remote system that the local system has connected to via RDP. These keys are created when connecting to the remote system for the first time. The name of the key will be either the IP Address or the domain name used to connect to the remote system. 

The `CertHash` value is created if the remote system has an untrusted certificate, and the user establishing the RDP connection has selected to proceed anyway as well as to never be notified of the certificate error again.

The `UsernameHint` value is created afterwards, and only when a logon succeeds. It stores the most recent username that the local system used to authenticate to the remote system via RDP.