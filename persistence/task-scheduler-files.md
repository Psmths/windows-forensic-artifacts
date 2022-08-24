# Task Scheduler Files
Task Scheduler Files are XML files that provide information regarding scheduled tasks on an endpoint. These files are created when a new task is scheduled on the endpoint. This artifact is similar to and replaces `.job` files on Windows XP, but provides more information. 

### Behavioral Indications
 - [x] Behavioral - Persistence (TA0003)
 - [x] Behavioral - Lateral Movement (TA0008)

### Analysis Value
 - [x] Execution - Command Line Options
 - [x] Execution - First Executed
 - [x] Execution - Last Executed
 - [x] Execution - Permissions / Account
 - [x] Execution - Evidence of Execution
 - [x] Execution - Time
 - [x] File - Path


## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista

## Artifact Location(s)
 - `%SystemRoot%\System32\Tasks` for tasks scheduled by 64-bit processes
 - `%SystemRoot%\SysWOW64\Tasks` for tasks scheduled by 32-bit processes

## Artifact Interpretation
| XML Path | Interpretation |
| - | - |
| `Task/Registration Info/Date` | Date the task was scheduled |
| `Task/Registration Info/Author` | Author of the task. Can be local or remote. |
| `Task/Triggers` | Triggers for the scheduled task |
| `Task/Actions` | Action taken by the scheduled task |
| `Task/Principals` | Authentication used for the task during execution |

In the event that a scheduled task was remotely created (an excellent indicator of lateral movement), the `Task/Registration Info/Author` field will provide the originating endpoint. 