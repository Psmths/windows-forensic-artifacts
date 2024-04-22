# Microsoft-Windows-GroupPolicy/Operational/4005: Starting manual processing of policy for user
This event indicates that the Group Policy for a specific user has been updated manually, typically as a result of issuing the command `gpudate /force`. This event is also triggered by periodic updates to the Group Policy as well. This is as opposed to [Microsoft-Windows-GroupPolicy/Operational/4001: Starting user logon Policy processing](/group-policy/evtx-4001-user-logon-gpo-processing-start.md), which is a user GPO processing started when a user logs in.

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
| EventData/PrincipalSamName            | The SAM name for the user account for which the GPO update was started. |

In the event that new User Group Policy Objects were found for the user in question, [System/1503: The Group Policy settings for the user were processed successfully](/group-policy/evtx-1503-user-gpo-success.md) will be generated with the same `System/Correlation ActivityID `.

If new User Group Policy Objects were found, [Microsoft-Windows-GroupPolicy/Operational/5312: List of applicable Group Policy objects](/group-policy/evtx-5312-list-of-gpo.md) will be generated with the same `System/Correlation ActivityID `, providing a list of all the applicable GPOs.

## Example
```
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-GroupPolicy" Guid="{aea1b4fa-97d1-45f2-a64c-4d69fffd92c9}" /> 
  <EventID>4005</EventID> 
  <Version>1</Version> 
  <Level>4</Level> 
  <Task>0</Task> 
  <Opcode>1</Opcode> 
  <Keywords>0x4000000000000000</Keywords> 
  <TimeCreated SystemTime="2024-04-22T19:48:03.3598321Z" /> 
  <EventRecordID>544</EventRecordID> 
  <Correlation ActivityID="{20ff7f77-1fab-41db-af00-72215ae5c002}" /> 
  <Execution ProcessID="13044" ThreadID="1160" /> 
  <Channel>Microsoft-Windows-GroupPolicy/Operational</Channel> 
  <Computer>DESKTOP-88KIKM6.example.lan</Computer> 
  <Security UserID="S-1-5-18" /> 
  </System>
- <EventData>
  <Data Name="System/Correlation ActivityID ">{20ff7f77-1fab-41db-af00-72215ae5c002}</Data> 
  <Data Name="PrincipalSamName">EXAMPLEDOMAIN\user</Data> 
  <Data Name="IsMachine">0</Data> 
  <Data Name="IsDomainJoined">true</Data> 
  <Data Name="IsBackgroundProcessing">true</Data> 
  <Data Name="IsAsyncProcessing">false</Data> 
  <Data Name="IsServiceRestart">false</Data> 
  <Data Name="ReasonForSyncProcessing">0</Data> 
  </EventData>
  </Event>
```

<sup><sub>This example was produced on Windows 10, Version 10.0.19045 Build 19045</sub></sup>


