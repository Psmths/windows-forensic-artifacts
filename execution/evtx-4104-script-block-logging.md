# Microsoft-Windows-PowerShell/Operational/4104: PowerShell Script Block Logging
This event records the contents of a PowerShell script block when Script Block Logging is enabled. It can provide valuable evidence of commands and code processed by PowerShell, including potentially malicious activity. Script Block Logging is configuration-dependent and should not be assumed to be enabled by default across all Windows or PowerShell installations.

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

The following fields may be interpreted from this artifact:

| Field Name | Interpretation |
| - | - |
| System / TimeCreated | Timestamp associated with the event. It indicates when the event was recorded, not necessarily when the script first began executing. |
| System / Execution / ProcessID | Process ID of the process that emitted the event. Correlate with process-creation events to identify the associated PowerShell process. |
| System / Security / UserID | SID associated with the event's security context, when present. |
| EventData / MessageNumber | Sequence number of this event's fragment within a multi-event script block. |
| EventData / MessageTotal | Total number of event fragments for the script block. |
| EventData / ScriptBlockText | Script-block content recorded by PowerShell. |
| EventData / ScriptBlockId | Identifier used to correlate events belonging to the same script block. |
| EventData / Path | Script file path when available. This field may be empty for script blocks that do not originate from a script file. |

The `Level` field should not be interpreted as a reliable indicator that a script is malicious. Event level values describe event severity or classification; they do not independently establish that a script was flagged as suspicious based on its contents.

Large script blocks may be split across multiple Event ID 4104 events. Use `ScriptBlockId` to identify related events and `MessageNumber` and `MessageTotal` to order and verify the fragments. Reassemble the script by concatenating the fragments in sequence order. Do not assume that every script block will fit in a single event.

### Time of Execution
The event timestamp indicates when that particular event was recorded. For a script block spanning multiple events, the fragments may have separate timestamps. These timestamps should not automatically be treated as the exact start or completion time of script execution.

Correlate Event ID 4104 with process-creation events, PowerShell engine events, and other available telemetry to establish a more complete timeline.

### Process Genealogy
The `System / Execution / ProcessID` field identifies the process that emitted the event. Correlate it with process-creation telemetry, such as [Security/4688: A new process has been created](execution/evtx-4688-process-created.md), to investigate the associated process and its parent.

Note that the process ID in Event ID 4104 is not itself a parent process ID. Event ID 4688 contains process-creation information that can help establish process genealogy.

## Caveats
- Event ID 4104 depends on Script Block Logging being enabled and applicable policy and PowerShell configuration. Its absence does not prove that PowerShell was not used.
- A script block may generate multiple Event ID 4104 records. Use `ScriptBlockId`, `MessageNumber`, and `MessageTotal` to reconstruct fragmented content.
- Script Block Logging is distinct from Module Logging and transcription. Module Logging records pipeline execution details for configured modules, while transcription can record session input and output. Neither should be treated as a direct substitute for the others.

## Example

```
<Event
  xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
  <System>
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
  <EventData>
    <Data Name="MessageNumber">1</Data>
    <Data Name="MessageTotal">1</Data>
    <Data Name="ScriptBlockText">Invoke-WebRequest -Uri "https://example.com" -OutFile "C:\Temp\test.exe"</Data>
    <Data Name="ScriptBlockId">808e4d85-977e-4418-9218-59dd2aa1b0ef</Data>
    <Data Name="Path" />
  </EventData>
</Event>
```