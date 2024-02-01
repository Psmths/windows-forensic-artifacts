# Microsoft-Windows-PowerShell/Operational/4104: PowerShell Script Block Logging
Now enabled by default, this event is logged whenever a script is run through PowerShell. 

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)

### Analysis Value
 - [x] Execution - Process Tree
 - [x] Execution - First Executed
 - [x] File - Path
 - [x] Account - Security Identifier (SID)
 
## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7

## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Microsoft-Windows-PowerShell%4Operational.evtx`

## Artifact Interpretation

The `Level` field (available in PowerShell5.0) may indicate a suspicious script. If its value is `Warning` this indicates the script was flagged as suspicious based on its contents. 

The `ScriptBlock ID` is used to uniquely identify a script across multiple `4104` events. Large scripts may be split across multiple events, to reconstruct the full script, concatenate all the events with this unique ID together. 

The `Path` field indicates the full path to the executed script. 

The `ProcessID` field indicates the parent process ID (PID) that executed the script. 

The `UserID` field contains the SID of the user that executed the script. 

### Time of Execution
The timestamp of the event indicates the time at which the script, identified by the field `ScriptBlock ID` was executed for the first time. 

### Process Genealogy
The `Creator Process ID` field may be cross-referenced with [Security/4688: A new process has been created](execution/evtx-4688-process-created.md).

## Caveats
This event is only logged the first time that a script is executed.

This event will not capture the resultant output of an executed script as module logging does. 

## Example

```
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-PowerShell" Guid="{a0c1853b-5c40-4b15-8766-3cf1c58f985a}" /> 
  <EventID>4104</EventID> 
  <Version>1</Version> 
  <Level>5</Level> 
  <Task>2</Task> 
  <Opcode>15</Opcode> 
  <Keywords>0x0</Keywords> 
  <TimeCreated SystemTime="2023-11-06T20:24:01.8911267Z" /> 
  <EventRecordID>194</EventRecordID> 
  <Correlation ActivityID="{7af08a74-10ed-0003-e08b-f07aed10da01}" /> 
  <Execution ProcessID="4328" ThreadID="4600" /> 
  <Channel>Microsoft-Windows-PowerShell/Operational</Channel> 
  <Computer>DESKTOP-TUMSOE7</Computer> 
  <Security UserID="S-1-5-21-1392459375-2353216063-1350843065-1001" /> 
  </System>
- <EventData>
  <Data Name="MessageNumber">1</Data> 
  <Data Name="MessageTotal">1</Data> 
  <Data Name="ScriptBlockText">Invoke-WebRequest -Uri "https://example.com" -OutFile "C:\Temp\test.exe"</Data> 
  <Data Name="ScriptBlockId">808e4d85-977e-4418-9218-59dd2aa1b0ef</Data> 
  <Data Name="Path" /> 
  </EventData>
  </Event>
```