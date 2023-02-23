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