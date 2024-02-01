# TerminalServices-RDPClient/Operational/1024: RDP ClientActiveX is trying to connect to the server
This event, logged to the TerminalServices-RDPClient/Operational channel, is logged when an RDP session is attempted to a remote endpoint. 

> [!IMPORTANT]  
> This event is logged on the **source** endpoint. 

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
This artifact can provide the **destination** IP address (or hostname) of an *attempted* RDP session. It will also provide the SID of the user who initiated the *attempted* connection, as well as the ProcessID associated with this activity. 

## Caveats
This event is logged regardless of success or failure of the RDP session, and must be cross-referenced with other events such as [4624: An account was successfully logged on](/account/evtx-4624-successful-logon.md) on the destination host.

If available, a successful RDP authentication is indicated by the event ID `TerminalServices-RDPClient/Operational/1027: Connected to domain`. To correlate these two Event IDs, compare their `Correlation ActivityID` field values.

When the RDP session is ended, either due to a failure to connect, a failure to successfully authenticate, or a manual close of the session, `TerminalServices-RDPClient/Operational/1105: The multi-transport connection has been disconnected` and `TerminalServices-RDPClient/Operational/1026: RDP ClientActiveX has been disconnected` should be logged, and is likewise able to be correlated by its `Correlation ActivityID` field. This allows for determining a time span during which an RDP session was in progress.

## Example
In the following example, the user with SID `S-1-5-21-3471133136-2963561160-3931775028-1001` attempted to RDP to a system at IP address `192.168.116.74`. The connection was not successfull, resulting in `TerminalServices-RDPClient/Operational/1026: RDP ClientActiveX has been disconnected` being logged with the same `Correlation ActivityID` value of `{780cf827-0ed1-4f4b-924c-3b14e7660000}`. 

```
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-TerminalServices-ClientActiveXCore" Guid="{28aa95bb-d444-4719-a36f-40462168127e}" /> 
  <EventID>1024</EventID> 
  <Version>0</Version> 
  <Level>4</Level> 
  <Task>101</Task> 
  <Opcode>10</Opcode> 
  <Keywords>0x4000000000000000</Keywords> 
  <TimeCreated SystemTime="2023-07-14T14:06:31.9398747Z" /> 
  <EventRecordID>1641</EventRecordID> 
  <Correlation ActivityID="{780cf827-0ed1-4f4b-924c-3b14e7660000}" /> 
  <Execution ProcessID="11136" ThreadID="3624" /> 
  <Channel>Microsoft-Windows-TerminalServices-RDPClient/Operational</Channel> 
  <Computer>HLPC01</Computer> 
  <Security UserID="S-1-5-21-3471133136-2963561160-3931775028-1001" /> 
  </System>
- <EventData>
  <Data Name="Name">Server Name</Data> 
  <Data Name="Value">192.168.116.74</Data> 
  <Data Name="CustomLevel">Info</Data> 
  </EventData>
  </Event>
```

```
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-TerminalServices-ClientActiveXCore" Guid="{28aa95bb-d444-4719-a36f-40462168127e}" /> 
  <EventID>1026</EventID> 
  <Version>0</Version> 
  <Level>4</Level> 
  <Task>101</Task> 
  <Opcode>11</Opcode> 
  <Keywords>0x4000000000000000</Keywords> 
  <TimeCreated SystemTime="2023-07-14T14:06:33.6543161Z" /> 
  <EventRecordID>1643</EventRecordID> 
  <Correlation ActivityID="{780cf827-0ed1-4f4b-924c-3b14e7660000}" /> 
  <Execution ProcessID="11136" ThreadID="3624" /> 
  <Channel>Microsoft-Windows-TerminalServices-RDPClient/Operational</Channel> 
  <Computer>HLPC01</Computer> 
  <Security UserID="S-1-5-21-3471133136-2963561160-3931775028-1001" /> 
  </System>
- <EventData>
  <Data Name="Name">Disconnect Reason</Data> 
  <Data Name="Value">1</Data> 
  <Data Name="CustomLevel">Info</Data> 
  </EventData>
  </Event>
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>