# Microsoft-Windows-TerminalServices-LocalSessionManager/Operational/21: Session logon succeeded
This event, logged to the Microsoft-Windows-TerminalServices-LocalSessionManager/Operational channel, is logged when an RDP connection is successfully authenticated. 

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
- `%SystemRoot%\System32\Winevt\Logs\Microsoft-Windows-TerminalServices-LocalSessionManager%4Operational.evtx`

## Artifact Interpretation
### Account - Username
This event logs only the username and domain that the RDP connection was attempting to establish a session for. It is located in the XML path `UserData\EventXML\User`.

### Network Activity - Evidence of Network Activity
The presence of this event indicates that an RDP connection was established and authenticated to the system on which this event was logged.

### Network Activity - Source Identification
This artifact can provide the **source** IP address of an RDP session. This information will be in the XML path `UserData\EventXML\Address` of the event.

### SessionID Correlation
This event logs a Session ID, available in the XML path `UserData\EventXML\SessionID`. This may be used to correlate activity between other events logged in this channel.

### ActivityID Correlation
This event logs an ActivityID, available in the XML path `System\Correlation ActivityID`. This may be used to correlate activity between other events logged that are related to this activity, such as:

 - [Microsoft-Windows-TerminalServices-RemoteConnectionManager/Operational/1149](/network/terminal-services-remote-1149.md) to determine when the connection was established

### Determining Timeline of RDP Activity
Together with [Microsoft-Windows-TerminalServices-LocalSessionManager/Operational/24: Session logon succeeded](/network/terminal-services-local-24.md), by correlating the `SessionID` field of both events, one can determine the start and end time of an RDP session.

## Example
```
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-TerminalServices-LocalSessionManager" Guid="{5d896912-022d-40aa-a3a8-4fa5515c76d7}" /> 
  <EventID>21</EventID> 
  <Version>0</Version> 
  <Level>4</Level> 
  <Task>0</Task> 
  <Opcode>0</Opcode> 
  <Keywords>0x1000000000000000</Keywords> 
  <TimeCreated SystemTime="2023-07-12T12:01:05.5944806Z" /> 
  <EventRecordID>1520</EventRecordID> 
  <Correlation ActivityID="{f4204024-08a2-45cd-951b-f756f64b0000}" /> 
  <Execution ProcessID="1440" ThreadID="18112" /> 
  <Channel>Microsoft-Windows-TerminalServices-LocalSessionManager/Operational</Channel> 
  <Computer>HLPC01</Computer> 
  <Security UserID="S-1-5-18" /> 
  </System>
- <UserData>
- <EventXML xmlns="Event_NS">
  <User>HLPC01\john.doe</User> 
  <SessionID>4</SessionID> 
  <Address>192.168.180.57</Address> 
  </EventXML>
  </UserData>
  </Event>
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>