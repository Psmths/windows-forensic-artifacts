# Microsoft-Windows-GroupPolicy/Operational/4000: Starting computer boot policy processing
This event indicates that the Group Policy for a specific computer has been updated due to the computer having booted. This is as opposed to [Microsoft-Windows-GroupPolicy/Operational/4004: Starting manual processing of policy for computer](/group-policy/evtx-4004-computer-manual-gpo-processing-start.md), which is a computer GPO processing started manually or periodically.

### Analysis Value
 - [x] Network - Evidence of Network Activity

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista


## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Microsoft-Windows-GroupPolicy%4Operational.evtx`

## Artifact Interpretation

| XML Path                              | Interpretation  |
| ------------------------------------- | --------------- |
| System/Correlation ActivityID            | The ActivityID for this GPO update. This can be used to filter/correlate other events relating to this activity in both the System event log as well as the Group Policy Operational log. |
| EventData/PrincipalSamName            | The SAM name for the computer account for which the GPO update was started. |

In the event that new Computer Group Policy Objects were found for the computer in question, [System/1502: The Group Policy settings for the computer were processed successfully](/group-policy/evtx-1502-computer-gpo-success.md) will be generated with the same `System/Correlation ActivityID `.

If new Computer Group Policy Objects were found, [Microsoft-Windows-GroupPolicy/Operational/5312: List of applicable Group Policy objects](/group-policy/evtx-5312-list-of-gpo.md) will be generated with the same `System/Correlation ActivityID`, providing a list of all the applicable GPOs.

## Example
```
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-GroupPolicy" Guid="{aea1b4fa-97d1-45f2-a64c-4d69fffd92c9}" /> 
  <EventID>4000</EventID> 
  <Version>1</Version> 
  <Level>4</Level> 
  <Task>0</Task> 
  <Opcode>1</Opcode> 
  <Keywords>0x4000000000000000</Keywords> 
  <TimeCreated SystemTime="2024-04-22T03:24:12.4007717Z" /> 
  <EventRecordID>86</EventRecordID> 
  <Correlation ActivityID="{564ee5ce-c812-4a1a-81bc-1a232e935da0}" /> 
  <Execution ProcessID="1340" ThreadID="1536" /> 
  <Channel>Microsoft-Windows-GroupPolicy/Operational</Channel> 
  <Computer>DESKTOP-88KIKM6.example.lan</Computer> 
  <Security UserID="S-1-5-18" /> 
  </System>
- <EventData>
  <Data Name="System/Correlation ActivityID ">{564ee5ce-c812-4a1a-81bc-1a232e935da0}</Data> 
  <Data Name="PrincipalSamName">EXAMPLEDOMAIN\DESKTOP-88KIKM6$</Data> 
  <Data Name="IsMachine">1</Data> 
  <Data Name="IsDomainJoined">true</Data> 
  <Data Name="IsBackgroundProcessing">false</Data> 
  <Data Name="IsAsyncProcessing">false</Data> 
  <Data Name="IsServiceRestart">false</Data> 
  <Data Name="ReasonForSyncProcessing">1</Data> 
  </EventData>
  </Event>
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19045 Build 19045</sub></sup>


