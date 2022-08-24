# TaskScheduler/Operational Log
The TaskScheduler/Operational event log channel provides detailed tracing of scheduled tasks on an endpoint. 

### Behavioral Indications
 - [x] Behavioral - Persistence (TA0003)
 - [x] Behavioral - Lateral Movement (TA0008)

### Analysis Value
 - [x] Execution - Command Line Options
 - [x] Execution - First Executed
 - [x] Execution - Last Executed
 - [x] Execution - Evidence of Execution
 - [x] Execution - Time
 - [x] File - Path


## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7

## Artifact Location(s)
 - `%SystemRoot%\System32\Winevt\Logs\Microsoft-Windows-TaskScheduler%4Operational.evtx`

## Artifact Interpretation
The following event IDs are useful to hunt for persistent implants on an endpoint:

| Event ID | Description | Information |
| - | - | - |
| 106 | Scheduled Task Created | Origin Account/User |
| 140 | Scheduled Task Updated | Origin Account/User |
| 141 | Scheduled Task Deleted | Origin Account/User |
| 200 | Scheduled Task Executed | Executable file path |
| 201 | Scheduled Task Execution Completed | Executable file path |

Deleted scheduled tasks provide a high-fidelity indicator of suspicious activity as scheduled task deletion is a rare event on Windows endpoints. 

## Caveats
Logging for these events is disabled by default and must be enabled to provide these artifacts. 