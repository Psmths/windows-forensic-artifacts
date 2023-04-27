# Security/4778: Session reconnected
This event, logged to the Security channel, is logged when an RDP session is reconnected. This event is logged on the **destination** endpoint. 

⚠️ In windows XP, the corresponding Event ID is `682`.

### Behavioral Indications
 - [x] Behavioral - Lateral Movement (TA0008)

### Analysis Value
 - [x] Account - Login History
 - [x] Account - Logon ID
 - [x] Network Activity - Source Identification

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista

## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Security.evtx`

## Artifact Interpretation
This artifact can provide the **source** client name and IP address of the connecting endpoint. 

The fields may be interpreted as follows:

| Field Name | Interpretation |
| - | - |
| Subject / Account Name | The name of the account for this RDP session |
| Subject / Logon ID | [Logon ID](/README.md/#account---logon-id) for this session that can be used to correlate with other events |
| Additional Information / Client Name | The name of the source host |
| Additional Information / Client Address | The IP address of the source host |

## Caveats
This event is not unique to RDP sessions. During Fast User Switching events, this event will be logged with the session name set to `Console` and the Client Address set to `LOCAL`. For RDP sessions, the session name will look like: `RDP-Tcp#1`.