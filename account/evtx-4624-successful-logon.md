# Security/4624: An account was successfully logged on
This event indicates an account has successfuly authenticated to the endpoint. It is logged on the **destination** endpoint. 

This event is a **Logon Event**, meaning it is logged on the system that is being authenticated to. 

⚠️ In windows XP, the corresponding Event ID is `528`.

### Behavioral Indications
 - [x] Behavioral - Lateral Movement (TA0008)

### Analysis Value
 - [x] Account - Login History
 - [x] Account - Logon ID
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
| Logon Information / Logon Type | Logon type. See [Logon Types](#logon-types) |
| New Logon / Security ID | SID of account that authenticated |
| New Logon / Account Name | Name of account that authenticated |
| New Logon / Logon ID | Logon ID for account that authenticated |
| Network Information / Source Network Address | IP address of source endpoint |

**NOTE:** The SID may be translated by event viewer. To view the raw SID, look at the event's XML data, which has the following fields available:

| XML Field Name | Interpretation |
| - | - |
| EventData/LogonType | Logon type. See [Logon Types](#logon-types) |
| EventData/TargetUserSid | SID of account that authenticated |
| EventData/TargetUserName | Name of account that authenticated |
| EventData/TargetLogonId | Logon ID for account that authenticated. Can be cross-referenced with 4634 Logoff events. |
| EventData/IpAddress | IP address of source endpoint |

### Logon Types
The following logon types are commonly seen:

| Logon Type | Description |
| - | - |
| 2 | Interactive Logon (user loggong on through screen or virtual console) |
| 3 | Network Logon |
| 7 | RDP reconnects or interactive unlocking |
| 9 | Explicit credentials (`runas`) |
| 10 | Interactive logon (RDP) |

### Caveats
Certain situations may cause this artifact to become noisy. On Domain Controllers serving many endpoints, these endpoints will periodically perform Type 3 Network Logons to the Domain Controller to retrieve group policy.