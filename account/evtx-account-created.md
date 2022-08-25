# Security/4720: A user account was created
This event, logged to the Security channel, indicates a new user account was created on the endpoint.

⚠️ In windows XP, the corresponding Event ID is `624`.

### Behavioral Indications
 - [x] Behavioral - Persistence (TA0003)

### Analysis Value
 - [x] Account - Creation Time
 - [x] Account - Logon ID
 - [x] Account - Security Identifier (SID)

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista

## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Security.evtx`

## Artifact Interpretation
The following fields may be interpreted from this artifact:

| Field Name | Interpretation |
| - | - |
| Subject / Security ID | SID of account that created the new account |
| Subject / Account Name | Account name of account that created the new account |
| Subject / Logon ID | Logon ID of session for account that created the new account |
| New Account / Security ID | SID of the new account |
| New Account / Account Name | Name of the new account |

When viewed in Event Viewer, the SIDs will be translated to names if possible. To view the underlying SIDs, the event's XML data should be analyzed. 

When parsing the event's XML data:
| XML Path | Interpretation |
| - | - |
| EventData/TargetUserName | New account name |
| EventData/TargetSid | New account SID |
| EventData/SubjectUserSid | SID of account that created the new account |
| EventData/SubjectUserName | Account name of account that created the new account |
| EventData/SubjectLogonId | Logon ID of session for account that created the new account |

