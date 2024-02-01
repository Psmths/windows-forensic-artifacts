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
 - [x] Network Activity - Source Identification

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Server 2019
 - [x] Windows Server 2016
 - [x] Windows Server 2012 R2
 - [x] Windows Server 2012
 - [x] Windows Server 2008 R2

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

This activity is also logged in the Security channel with more granular information, as follows:
| Event ID | Description |
| - | - |
| 4698 | Scheduled Task Created |
| 4702 | Scheduled Task Updated |
| 4699 | Scheduled Task Deleted |

## Caveats
Logging for these events is disabled by default and must be enabled to provide these artifacts.

## Analysis Tips

### Deleted Scheduled Tasks
Scheduled task deletion is a rare event on Windows systems and provides an easy to query, high-fidelity indicator of suspicious activity. The following event IDs may be queried:

 - `Windows-TaskScheduler\Operational Event 141: Scheduled Task Deleted`
 - `Security Event 4699: Scheduled Task Deleted`

### Software Installation/Uninstallation
When applications are installed on a Windows system, they will sometimes create a scheduled task to run their update functionality, making the Task Scheduler Operational log a possible option for cross-validation of other application installation artifacts such as the `Uninstall` registry key. 

### Lateral Movement through Remote Scheduled Task Installation
In the event that tasks are remotely scheduled, as is commonly seen during lateral movement attempts, this activity may be identified by observing Type 3 logons via [4624: An account was successfully logged on](/account/evtx-4624-successful-logon.md) events in close proximity to task creation.