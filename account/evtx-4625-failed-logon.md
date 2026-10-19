# Security/4625: An account failed to log on 
This logon event indicates an account has failed to authenticate to the endpoint. It is logged on the **destination** endpoint. It closely mirrors the information logged for the event [4624: An account was successfully logged on](/account/evtx-4624-successful-logon.md).

> [!NOTE]
> This event is a **Logon Event**, meaning it is logged on the system that is being authenticated to. 

### Behavioral Indications
 - [x] Behavioral - Lateral Movement (TA0008)

### Analysis Value
 - [x] Account - Login History
 - [x] Account - Security Identifier (SID)
 - [x] Network Activity - Source Identification

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista
 - [x] Windows XP

## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Security.evtx`

## Artifact Interpretation
The following fields may be interpreted from this artifact:

| Field Name | Interpretation |
| - | - |
| Logon Type | Logon type. See [Logon Types](#logon-types) |
| Account For Which Logon Failed / Security ID | SID of account that authenticated |
| Account For Which Logon Failed / Account Name | Name of account that authenticated |
| Network Information / Source Network Address | IP address of source endpoint |

> [!NOTE]
> The SID may be translated by event viewer. To view the raw SID, look at the event's XML data, which has the following fields available:

| XML Field Name | Interpretation |
| - | - |
| EventData/LogonType | Logon type. See [Logon Types](#logon-types) |
| EventData/TargetUserSid | SID of account that authenticated |
| EventData/TargetUserName | Name of account that authenticated |
| EventData/IpAddress | IP address of source endpoint |

### Logon Types
The following logon types are commonly seen:

| Logon Type | Description |
| - | - |
| 2 | **Interactive** — User logged on locally at the computer's console. |
| 3 | **Network** — User or computer accessed the computer over a network, such as through SMB. May also be associated with RDP-related authentication, but does not by itself identify an RDP logon. |
| 7 | **Unlock** — User unlocked an existing workstation session. May appear in investigations involving RDP session reconnection or unlocking, but is not specific to RDP. |
| 9 | **NewCredentials** — Caller cloned its current token and specified new credentials for outbound connections, such as with `runas /netonly`. |
| 10 | **RemoteInteractive** — User logged on remotely using Remote Desktop Services (RDP) or Terminal Services. The clearest of these logon types for identifying an RDP interactive logon. |