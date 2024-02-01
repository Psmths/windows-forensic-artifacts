# Security/4648: Logon using explicit credentials
This event, logged to the Security channel, indicates a logon was completed using explicit credentials. Explicit credentials refers to credentials that are not currently active and have been explicitly selected by an attacker. It is logged on the **source** system and may indicate lateral movement tactics such as the use of `runas` or access to remote file shares.

> [!NOTE]
> In windows XP, the corresponding Event ID is `552`.

### Behavioral Indications
 - [x] Behavioral - Lateral Movement (TA0008)

### Analysis Value
 - [x] Account - Login History
 - [x] Account - Logon ID
 - [x] Account - Security Identifier (SID)
 - [x] Network Activity - Destination Identification

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
| Subject / Security ID | SID of account that used the explicit credentials |
| Subject / Account Name | Name of account that used the explicit credentials |
| Subject / Logon ID | [Logon ID](/README.md/#account---logon-id) of session for account that used the explicit credentials |
| Account Whose Credentials Were Used / Account Name | Account name for the explicit credentials |
| Target Server / Target Server Name | The name of the **destination** endpoint the credentials were used on |
| Process Information / Process ID | Hex PID of the process that used the explicit credentials |
| Process Information / Process Name | The command line of the process that used the explicit credentials |
| Network Information / Network Address | IP address of **source** endpoint |

> [!NOTE]
> The SID may be translated by event viewer. To view the raw SID, look at the event's XML data, which has the following fields available:

This event may be correlated with events found on the destination endpoint, such as event [4624: An account was successfully logged on](/account/evtx-4624-successful-logon.md). In the case of runas activity, a 4624 event will be registered with Logon type 9.