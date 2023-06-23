# Security/5156: The Windows Filtering Platform has permitted a connection
This event indicates that the Windows Filtering Platform (WFP) has allowed an inbound or outbound connection to be placed. This is a high-volume event and is, by default, disabled. 

To enable it, the System Audit Polict `Windows Filtering Platform Connection` must be configured for Success and/or Failure auditing. This allows for the logging of events when:

 - The Windows Firewall Service blocks an application from accepting incoming connections on the network.
 - The WFP allows a connection.
 - The WFP blocks a connection.
 - The WFP permits a bind to a local port.
 - The WFP blocks a bind to a local port.
 - The WFP allows a connection.
 - The WFP blocks a connection.
 - The WFP permits an application or service to listen on a port for incoming connections.
 - The WFP blocks an application or service to listen on a port for incoming connections.

### Analysis Value
 - [x] Execution - Evidence of Execution
 - [x] File - Path
 - [x] Network - Evidence of Network Activity
 - [x] Network Activity - Destination Identification
 - [x] Network Activity - Source Identification
 - [x] Network Activity - Firewall Activity

## Operating System Availability
 - [x] Windows 11 (⚠️ Requires audit policy configuration)
 - [x] Windows 10 (⚠️ Requires audit policy configuration)
 - [x] Windows 8 (⚠️ Requires audit policy configuration)
 - [x] Windows 7 (⚠️ Requires audit policy configuration)

## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Security.evtx`

## Artifact Interpretation
When logged, this event indicates that Windows Filtering Platform has allowed either an inbound or outbound connection to take place. The following information is provided by the event:

| XML Path                              | Interpretation  |
| ------------------------------------- | --------------- |
| EventData/ProcessID               | The Process ID of the process that spawned or accepted the connection |
| EventData/Application        | The full path of the process that spawned or accepted the connection |
| EventData/Direction                        | %%14592 (Inbound), %%14593 (Outbound) |
| EventData/SourceAddress     | The source IP address of the connection |
| EventData/SourcePort              | The source port of the connection |
| EventData/DestAddress              | The destination IP address of the connection |
| EventData/DestPort              | The destination port of the connection |
| EventData/Protocol              | The protocol used for the connection (i.e 6 for TCP, 1 for ICMP, etc.) |

Some common protocols:

 - 1: ICMP
 - 6: TCP
 - 17: UDP

The complete list of protocols is assigned and made available from IANA (Internet Assigned Numbers Authority).

This event is logged even if the network connection never left the local system (i.e, localhost to localhost network traffic). In this case, the source and destination IP addresses will both be `127.0.0.1`.

## Example
In the following example, a DNS request was made on the local system, causing this event to be logged. Note that the `Protocol` field is `17` for UDP.
```
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-Security-Auditing" Guid="{54849625-5478-4994-a5ba-3e3b0328c30d}" /> 
  <EventID>5156</EventID> 
  <Version>1</Version> 
  <Level>0</Level> 
  <Task>12810</Task> 
  <Opcode>0</Opcode> 
  <Keywords>0x8020000000000000</Keywords> 
  <TimeCreated SystemTime="2023-06-23T00:28:36.6473989Z" /> 
  <EventRecordID>15500373</EventRecordID> 
  <Correlation /> 
  <Execution ProcessID="4" ThreadID="9184" /> 
  <Channel>Security</Channel> 
  <Computer>HLPC01</Computer> 
  <Security /> 
  </System>
- <EventData>
  <Data Name="ProcessID">2780</Data> 
  <Data Name="Application">\device\harddiskvolume5\windows\system32\svchost.exe</Data> 
  <Data Name="Direction">%%14593</Data> 
  <Data Name="SourceAddress">10.100.65.234</Data> 
  <Data Name="SourcePort">50704</Data> 
  <Data Name="DestAddress">10.100.0.10</Data> 
  <Data Name="DestPort">53</Data> 
  <Data Name="Protocol">17</Data> 
  <Data Name="FilterRTID">89854</Data> 
  <Data Name="LayerName">%%14611</Data> 
  <Data Name="LayerRTID">48</Data> 
  <Data Name="RemoteUserID">S-1-0-0</Data> 
  <Data Name="RemoteMachineID">S-1-0-0</Data> 
  </EventData>
  </Event>
```

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>

In the following example, the local system received DHCP traffic. The local system is not a DHCP server, but this traffic was seen regardless as DHCP typically works by broadcasting traffic. This is an example of how noisy this event can become.
```
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-Security-Auditing" Guid="{54849625-5478-4994-a5ba-3e3b0328c30d}" /> 
  <EventID>5156</EventID> 
  <Version>1</Version> 
  <Level>0</Level> 
  <Task>12810</Task> 
  <Opcode>0</Opcode> 
  <Keywords>0x8020000000000000</Keywords> 
  <TimeCreated SystemTime="2023-06-23T00:28:35.7459571Z" /> 
  <EventRecordID>15500366</EventRecordID> 
  <Correlation /> 
  <Execution ProcessID="4" ThreadID="7868" /> 
  <Channel>Security</Channel> 
  <Computer>HLPC01</Computer> 
  <Security /> 
  </System>
- <EventData>
  <Data Name="ProcessID">2148</Data> 
  <Data Name="Application">\device\harddiskvolume5\windows\system32\svchost.exe</Data> 
  <Data Name="Direction">%%14592</Data> 
  <Data Name="SourceAddress">0.0.0.0</Data> 
  <Data Name="SourcePort">68</Data> 
  <Data Name="DestAddress">255.255.255.255</Data> 
  <Data Name="DestPort">67</Data> 
  <Data Name="Protocol">17</Data> 
  <Data Name="FilterRTID">65886</Data> 
  <Data Name="LayerName">%%14610</Data> 
  <Data Name="LayerRTID">44</Data> 
  <Data Name="RemoteUserID">S-1-0-0</Data> 
  <Data Name="RemoteMachineID">S-1-0-0</Data> 
  </EventData>
  </Event>
```

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>