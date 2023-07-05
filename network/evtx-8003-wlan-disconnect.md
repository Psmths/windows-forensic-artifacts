# Microsoft-Windows-WLAN-AutoConfig/Operational/8003: Successfully Disconnected from a Wireless Network
This event indicates that the system has disconnected from a wireless area network.

### Analysis Value
 - [x] Network - Evidence of Network Activity
 - [x] Network - Wireless Activity

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista


## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Microsoft-Windows-WLAN-AutoConfig%4Operational.evtx`

## Artifact Interpretation
When logged, this event indicates that the system has successfully disconnected from a wireless area network. This event provides the following additional information regarding the connection:

| XML Path                              | Interpretation  |
| ------------------------------------- | --------------- |
| EventData/InterfaceGuid               | The wireless adapter's [Interface GUID](/enumeration/network-cards.md) |
| EventData/InterfaceDescription        | The name of the wireless adapter |
| EventData/SSID                        | The SSID of the network that was connected to |
| EventData/Reason              | Provides the reason this network was disconnected from |

Together with the event [Microsoft-Windows-WLAN-AutoConfig/Operational/8001: Successfully Connected to a Wireless Network](network/evtx-8001-wlan-connect), this can be used to determine a timespan during which a system was connected to a wireless area network.

## Example
```
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-WLAN-AutoConfig" Guid="{9580d7dd-0379-4658-9870-d5be7d52d6de}" /> 
  <EventID>8003</EventID> 
  <Version>0</Version> 
  <Level>4</Level> 
  <Task>24010</Task> 
  <Opcode>192</Opcode> 
  <Keywords>0x8000000040000600</Keywords> 
  <TimeCreated SystemTime="2023-06-16T03:05:19.9479495Z" /> 
  <EventRecordID>1321</EventRecordID> 
  <Correlation /> 
  <Execution ProcessID="4276" ThreadID="5544" /> 
  <Channel>Microsoft-Windows-WLAN-AutoConfig/Operational</Channel> 
  <Computer>HLPC01</Computer> 
  <Security UserID="S-1-5-18" /> 
  </System>
- <EventData>
  <Data Name="InterfaceGuid">{a7d8885d-10c1-43d4-9e1e-0a7b2678f020}</Data> 
  <Data Name="InterfaceDescription">Intel(R) Wi-Fi 6 AX200 160MHz</Data> 
  <Data Name="ConnectionMode">Automatic connection with a profile</Data> 
  <Data Name="ProfileName">SSID_TEST</Data> 
  <Data Name="SSID">SSID_TEST</Data> 
  <Data Name="BSSType">Infrastructure</Data> 
  <Data Name="Reason">The network is disconnected due to a policy disabling auto connect on this interface.</Data> 
  <Data Name="ConnectionId">0x1</Data> 
  <Data Name="ReasonCode">5</Data> 
  </EventData>
  </Event>
```

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>