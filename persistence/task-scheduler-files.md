# Task Scheduler Files
Task Scheduler Files are XML files that provide information regarding scheduled tasks on an endpoint. They contain information such as:

 - Scheduled task name
 - Scheduled task registration timestamp
 - Scheduled task conditions
 - Scheduled task command line arguments
 - Scheduled task author
 - Scheduled task designated execution account/user

These files are created when a new task is scheduled on the endpoint. 

This artifact is similar to and replaces `.job` files on Windows XP, and provides more information. 

## Operating System Availability
 - Windows 10
 - Windows 8
 - Windows 7
 - Windows Vista

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