# Microsoft-Windows-TerminalServices-RemoteConnectionManager/Operational/1149
This event, logged to the Microsoft-Windows-TerminalServices-RemoteConnectionManager/Operational channel, is logged when an RDP connection is established. 

> [!WARNING]
> This event does not indicate a successfully authenticated RDP session has taken place, only that the channel has been established for an RDP attempt to be made.

> [!IMPORTANT]  
> This event is logged on the **destination** endpoint. 

### Behavioral Indications
 - [x] Behavioral - Lateral Movement (TA0008)

### Analysis Value
 - [x] Account - Username
 - [x] Network Activity - Evidence of Network Activity
 - [x] Network Activity - Source Identification

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista
 - [x] Windows Server 2019
 - [x] Windows Server 2016
 - [x] Windows Server 2012 R2
 - [x] Windows Server 2012
 - [x] Windows Server 2008 R2
 - [x] Windows Server 2008

## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Microsoft-Windows-TerminalServices-RemoteConnectionManager%4Operational.evtx`

## Artifact Interpretation
### Account - Username
This event logs only the username that the RDP connection was attempting to establish a session for. It is located in the XML path `UserData\EventXML\Param1`.

### Network Activity - Evidence of Network Activity
The presence of this event indicates that an RDP connection was established to the system on which this event was logged.

### Network Activity - Source Identification
This artifact can provide the **source** IP address of an RDP *connection*. This information will be in the XML path `UserData\EventXML\Param3` of the event.

### ActivityID Correlation
This event logs an ActivityID, available in the XML path `System\Correlation ActivityID`. This may be used to correlate activity between other events logged that are related to this activity, such as:

 - [Microsoft-Windows-TerminalServices-LocalSessionManager/Operational/21: Session logon succeeded](/network/terminal-services-local-21.md)

## Caveats
This event is logged regardless of success or failure of the RDP session, and must be cross-referenced with other events such as:

 - [4624: An account was successfully logged on](/account/evtx-4624-successful-logon.md)
 - [Microsoft-Windows-TerminalServices-LocalSessionManager/Operational/21: Session logon succeeded](/network/terminal-services-local-21.md)

## Example
```
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-TerminalServices-RemoteConnectionManager" Guid="{c76baa63-ae81-421c-b425-340b4b24157f}" /> 
  <EventID>1149</EventID> 
  <Version>0</Version> 
  <Level>4</Level> 
  <Task>0</Task> 
  <Opcode>0</Opcode> 
  <Keywords>0x1000000000000000</Keywords> 
  <TimeCreated SystemTime="2023-07-12T12:01:19.3418899Z" /> 
  <EventRecordID>241</EventRecordID> 
  <Correlation ActivityID="{f4206c2f-b0bf-4c54-aad2-c7d2769b0000}" /> 
  <Execution ProcessID="10680" ThreadID="14208" /> 
  <Channel>Microsoft-Windows-TerminalServices-RemoteConnectionManager/Operational</Channel> 
  <Computer>HLPC01</Computer> 
  <Security UserID="S-1-5-20" /> 
  </System>
- <UserData>
- <EventXML xmlns="Event_NS">
  <Param1>john.doe</Param1> 
  <Param2 /> 
  <Param3>192.168.180.57</Param3> 
  </EventXML>
  </UserData>
  </Event>
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>