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
 - [x] Network Activity - Source Identification

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
| `Task/Registration Info/Date` | Date the task was scheduled, may or may not be present. |
| `Task/Registration Info/Author` | Author of the task |
| `Task/Triggers` | Triggers for the scheduled task. Could be on a calendar schedule, on boot, on log-on, etc.  |
| `Task/Actions` | Action taken by the scheduled task |
| `Task/Principals` | Authentication used for the task during execution |

In the event that a scheduled task was remotely created (an excellent indicator of lateral movement), the `Task/Registration Info/Author` field may provide the originating endpoint. In cases when the Windows Task Scheduler interface or the `schtasks` command was used to manually create a task, this field will be populated by the user account that performed this action.

> [!NOTE]
> The task scheduler file for a particular task will be created when the task is created. Therefore, the creation timestamp of the file should correspond to the creation time of the scheduled task. When the scheduled task is modified, the modification timestamp of the corresponding file will also be updated. 

## Example
The following example is a scheduled task created by Mozilla Firefox. From this task file we can conclude:

 - The command that will be executed is `C:\Program Files\Mozilla Firefox\default-browser-agent.exe do-task "308046B0AF4A39CB"`
 - This command will attempt to run every day beginning on `2023-12-26`
 - This command will run with the privileges of the user `DESKTOP-DR5P34J\user`
 - The creation and modification timestamp of this scheduled task is `12-26-2023 10:52 AM`, meaning this is when the scheduled task was created and it has not been updated since. 

```
<?xml version="1.0" encoding="UTF-16"?>
<Task version="1.2" xmlns="http://schemas.microsoft.com/windows/2004/02/mit/task">
  <RegistrationInfo>
    <Author>Mozilla</Author>
    <Description>The Default Browser Agent task checks when the default changes from Firefox to another browser. If the change happens under suspicious circumstances, it will prompt users to change back to Firefox no more than two times. This task is installed automatically by Firefox, and is reinstalled when Firefox updates. To disable this task, update the “default-browser-agent.enabled” preference on the about:config page or the Firefox enterprise policy setting “DisableDefaultBrowserAgent”.</Description>
    <URI>\Mozilla\Firefox Default Browser Agent 308046B0AF4A39CB</URI>
  </RegistrationInfo>
  <Triggers>
    <CalendarTrigger>
      <StartBoundary>2023-12-26T16:51:33Z</StartBoundary>
      <Enabled>true</Enabled>
      <ScheduleByDay>
        <DaysInterval>1</DaysInterval>
      </ScheduleByDay>
    </CalendarTrigger>
  </Triggers>
  <Settings>
    <MultipleInstancesPolicy>IgnoreNew</MultipleInstancesPolicy>
    <DisallowStartIfOnBatteries>false</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>false</StopIfGoingOnBatteries>
    <AllowHardTerminate>true</AllowHardTerminate>
    <StartWhenAvailable>true</StartWhenAvailable>
    <RunOnlyIfNetworkAvailable>false</RunOnlyIfNetworkAvailable>
    <IdleSettings>
      <Duration>PT10M</Duration>
      <WaitTimeout>PT1H</WaitTimeout>
      <StopOnIdleEnd>true</StopOnIdleEnd>
      <RestartOnIdle>false</RestartOnIdle>
    </IdleSettings>
    <AllowStartOnDemand>true</AllowStartOnDemand>
    <Enabled>true</Enabled>
    <Hidden>false</Hidden>
    <RunOnlyIfIdle>false</RunOnlyIfIdle>
    <WakeToRun>false</WakeToRun>
    <ExecutionTimeLimit>PT12H5M</ExecutionTimeLimit>
    <Priority>7</Priority>
  </Settings>
  <Actions Context="Author">
    <Exec>
      <Command>C:\Program Files\Mozilla Firefox\default-browser-agent.exe</Command>
      <Arguments>do-task "308046B0AF4A39CB"</Arguments>
    </Exec>
  </Actions>
  <Principals>
    <Principal id="Author">
      <UserId>DESKTOP-DR5P34J\user</UserId>
      <LogonType>InteractiveToken</LogonType>
      <RunLevel>LeastPrivilege</RunLevel>
    </Principal>
  </Principals>
</Task>
```

Another example, this time of a scheduled task created manualy through the Windows Task Scheduler control panel. Note that in this case the `Task/Registration Info/Author` field is populated with a user account corresponding to the user that created the scheduled task. Note that the `Task/Principals/RunLevel` field is set to `HighestAvailable`, indicating this task will launch the command with higher privileges. 

```
<?xml version="1.0" encoding="UTF-16"?>
<Task version="1.2" xmlns="http://schemas.microsoft.com/windows/2004/02/mit/task">
  <RegistrationInfo>
    <Date>2024-04-04T15:56:48.1199881</Date>
    <Author>DESKTOP-DR5P34J\user</Author>
    <URI>\Example Task</URI>
  </RegistrationInfo>
  <Triggers>
    <LogonTrigger>
      <Enabled>true</Enabled>
    </LogonTrigger>
  </Triggers>
  <Principals>
    <Principal id="Author">
      <RunLevel>HighestAvailable</RunLevel>
      <UserId>DESKTOP-DR5P34J\user</UserId>
      <LogonType>Password</LogonType>
    </Principal>
  </Principals>
  <Settings>
    <MultipleInstancesPolicy>IgnoreNew</MultipleInstancesPolicy>
    <DisallowStartIfOnBatteries>true</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>true</StopIfGoingOnBatteries>
    <AllowHardTerminate>true</AllowHardTerminate>
    <StartWhenAvailable>false</StartWhenAvailable>
    <RunOnlyIfNetworkAvailable>false</RunOnlyIfNetworkAvailable>
    <IdleSettings>
      <StopOnIdleEnd>true</StopOnIdleEnd>
      <RestartOnIdle>false</RestartOnIdle>
    </IdleSettings>
    <AllowStartOnDemand>true</AllowStartOnDemand>
    <Enabled>true</Enabled>
    <Hidden>false</Hidden>
    <RunOnlyIfIdle>false</RunOnlyIfIdle>
    <WakeToRun>false</WakeToRun>
    <ExecutionTimeLimit>P3D</ExecutionTimeLimit>
    <Priority>7</Priority>
  </Settings>
  <Actions Context="Author">
    <Exec>
      <Command>C:\Temp\example.exe</Command>
    </Exec>
  </Actions>
</Task>
```