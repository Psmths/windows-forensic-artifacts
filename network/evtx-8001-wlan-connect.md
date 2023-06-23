# Microsoft-Windows-WLAN-AutoConfig/Operational/8001: Successfully Connected to a Wireless Network
This event indicates that the system has connected successfully to a wireless area network.

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
When logged, this event indicates that the system has successfully connected to a wireless area network. This event provides the following additional information regarding the connection:

| XML Path                              | Interpretation  |
| ------------------------------------- | --------------- |
| EventData/InterfaceGuid               | The wireless adapter's [Interface GUID](/enumeration/network-cards.md) |
| EventData/InterfaceDescription        | The name of the wireless adapter |
| EventData/SSID                        | The SSID of the network that was connected to |
| EventData/AuthenticationAlgorithm     | The security protocol used to connect to the wireless network (i.e. WEP, WPA2, etc.) |
| EventData/ConnectionMode              | If the wireless network was set to be joined automatically, this will read "Automatic connection with a profile", otherwise, this will read "Connection to a secure network without a profile" |

Together with the event [Microsoft-Windows-WLAN-AutoConfig/Operational/8003: Successfully Disconnected from a Wireless Network](network/evtx-8003-wlan-disconnect), this can be used to determine a timestan during which a system was connected to a wireless area network.

## Example
```
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-WLAN-AutoConfig" Guid="{9580d7dd-0379-4658-9870-d5be7d52d6de}" /> 
  <EventID>8001</EventID> 
  <Version>0</Version> 
  <Level>4</Level> 
  <Task>24010</Task> 
  <Opcode>190</Opcode> 
  <Keywords>0x8000000020000600</Keywords> 
  <TimeCreated SystemTime="2022-10-27T04:37:47.1583133Z" /> 
  <EventRecordID>6</EventRecordID> 
  <Correlation /> 
  <Execution ProcessID="3132" ThreadID="3188" /> 
  <Channel>Microsoft-Windows-WLAN-AutoConfig/Operational</Channel> 
  <Computer>HLPC01</Computer> 
  <Security UserID="S-1-5-18" /> 
  </System>
- <EventData>
  <Data Name="InterfaceGuid">{a7d8885d-10c1-43d4-9e1e-0a7b2678f020}</Data> 
  <Data Name="InterfaceDescription">Intel(R) Wi-Fi 6 AX200 160MHz</Data> 
  <Data Name="ConnectionMode">Connection to a secure network without a profile</Data> 
  <Data Name="ProfileName">SSID_TEST</Data> 
  <Data Name="SSID">SSID_TEST</Data> 
  <Data Name="BSSType">Infrastructure</Data> 
  <Data Name="PHYType">802.11ax</Data> 
  <Data Name="AuthenticationAlgorithm">WPA2-Personal</Data> 
  <Data Name="CipherAlgorithm">AES-CCMP</Data> 
  <Data Name="OnexEnabled">0</Data> 
  <Data Name="ConnectionId">0x2</Data> 
  <Data Name="NonBroadcast">false</Data> 
  </EventData>
  </Event>
```

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>