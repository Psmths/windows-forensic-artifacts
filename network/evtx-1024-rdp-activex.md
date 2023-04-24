# TerminalServices-RDPClient/Operational/1024
This event, logged to the TerminalServices-RDPClient/Operational channel, is logged when an RDP session is attempted to a remote endpoint. This event is unique as it is logged on the **source** endpoint. 

### Behavioral Indications
 - [x] Behavioral - Lateral Movement (TA0008)

### Analysis Value
 - [x] Execution - Process Tree
 - [x] Account - Security Identifier (SID)
 - [x] Network - Evidence of Network Activity
 - [x] Network Activity - Destination Identification

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista

## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Microsoft-Windows-TerminalServices-RDPClient%4Operational.evtx`

## Artifact Interpretation
This artifact can provide the **destination** IP address of an *attempted* RDP session. It will also provide the SID of the user who initiated the *attempted* connection, as well as the ProcessID associated with this activity. 

## Caveats
This event is logged regardless of success or failure of the RDP session, and must be cross-referenced with other events such as [`4624: An account was successfully logged on`](/account/evtx-4624-successful-logon.md) on the destination host. 

## Example
![Example Image](/media/examples/evtx-1024-example.png)