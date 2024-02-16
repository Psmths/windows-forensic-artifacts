# Terminal Server Client Registry Keys
These registry keys store information regarding options selected by a user during the establishment of an RDP session, such as:

 - Ignoring an untrusted certificate
 - Username used to log in to the remote system via RDP

### Behavioral Indications
 - [x] Behavioral - Lateral Movement (TA0008)

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

## Example
The following example log, captured by Procmon on a Windows 10 system, shows the activity that takes place when a test user establishes a first-time connection to a remote system and authenticates successfully. In this instance, the user selected to not be notified of the untrusted certificate in the future, hence the creation of the `CertHash` key, and authenticated with the username `user`.

| Time of Day | Process Name | Operation    | Path                                                                                                 | Detail                                                                               |
|-------------|--------------|--------------|------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|
| 7:45:57 PM  | mstsc.exe    | RegCreateKey | HKCU\SOFTWARE\Microsoft\Terminal Server Client\Servers\desktop-ks82313.example.internal              | Desired Access: Write, Disposition: REG_CREATED_NEW_KEY                              |
| 7:45:57 PM  | mstsc.exe    | RegSetValue  | HKCU\SOFTWARE\Microsoft\Terminal Server Client\Servers\desktop-ks82313.example.internal\CertHash     | Type: REG_BINARY, Length: 32, Data: CB D1 82 3E 90 20 CB B0 EE A3 E0 64 35 31 95 F0  |
| 7:45:57 PM  | mstsc.exe    | TCP Connect  | desktop-ks82313.example.internal:59521 -> desktop-ks82313.example.internal:ms-wbt-server             | Length: 0, mss: 1460, sackopt: 1, tsopt: 0, wsopt: 1, rcvwin: 131400, rcvwinscale: 8 |
| 7:45:59 PM  | mstsc.exe    | RegSetValue  | HKCU\SOFTWARE\Microsoft\Terminal Server Client\Servers\desktop-ks82313.example.internal\UsernameHint | Type: REG_SZ, Length: 10, Data: user                                                 |