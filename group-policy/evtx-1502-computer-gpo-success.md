# System/1502: The Group Policy settings for the computer were processed successfully
This event indicates that Group Policy Object processing for a specific computer was completed, and that new Group Policy Objects were found and applied to the computer. It is the result of either:

 - A manual/scheduled user GPO update: [Microsoft-Windows-GroupPolicy/Operational/4004: Starting manual processing of policy for computer](/group-policy/evtx-4004-computer-manual-gpo-processing-start.md)
 - A user logon GPO update: [Microsoft-Windows-GroupPolicy/Operational/4000: Starting computer boot policy processing](/group-policy/evtx-4000-computer-boot-gpo-processing-start.md)

### Analysis Value
 - [x] Network - Evidence of Network Activity

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista


## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\System.evtx`

## Artifact Interpretation

| XML Path                              | Interpretation  |
| ------------------------------------- | --------------- |
| System/Correlation ActivityID            | The ActivityID for this GPO update. This can be used to filter/correlate other events relating to this activity in both the System event log as well as the Group Policy Operational log. |
| EventData/NumberOfGroupPolicyObjects            | How many GPOs were processed. |

If new Comptuer Group Policy Objects were found, [Microsoft-Windows-GroupPolicy/Operational/5312: List of applicable Group Policy objects](/group-policy/evtx-5312-list-of-gpo.md) will be generated with the same `System/Correlation ActivityID `, providing a list of all the applicable GPOs.

## Example
```
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-GroupPolicy" Guid="{aea1b4fa-97d1-45f2-a64c-4d69fffd92c9}" /> 
  <EventID>1502</EventID> 
  <Version>0</Version> 
  <Level>4</Level> 
  <Task>0</Task> 
  <Opcode>1</Opcode> 
  <Keywords>0x8000000000000000</Keywords> 
  <TimeCreated SystemTime="2024-04-22T19:48:03.3504551Z" /> 
  <EventRecordID>563</EventRecordID> 
  <Correlation ActivityID="{d739f467-3600-41b9-8b95-3cef52425544}" /> 
  <Execution ProcessID="13044" ThreadID="4444" /> 
  <Channel>System</Channel> 
  <Computer>DESKTOP-88KIKM6.example.lan</Computer> 
  <Security UserID="S-1-5-18" /> 
  </System>
- <EventData>
  <Data Name="SupportInfo1">1</Data> 
  <Data Name="SupportInfo2">4430</Data> 
  <Data Name="ProcessingMode">0</Data> 
  <Data Name="ProcessingTimeInMilliseconds">1328</Data> 
  <Data Name="DCName">\\nndc01.example.lan</Data> 
  <Data Name="NumberOfGroupPolicyObjects">4</Data> 
  </EventData>
  </Event>
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19045 Build 19045</sub></sup>