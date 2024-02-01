# Security/4688: A new process has been created
This event, logged to the Security channel, indicates a process was created on the system.

> [!NOTE]  
> In windows XP/Windows Server 2003, the corresponding Event ID is `592`.

> [!NOTE]  
>  These events require the `Audit Process Creation` policy to be configured, which is by default not configured. 

> [!NOTE]  
>  For the events to contain the full command line for each logged process, an additional policy called `Include command line in process creation events` must also be configured. 

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)

### Analysis Value
 - [x] Account - Logon ID
 - [x] Account - Security Identifier (SID)
 - [x] Execution - Command Line Options (⚠️ Requires audit policy configuration)
 - [x] Execution - Permissions / Account
 - [x] Execution - Process Tree
 - [x] Execution - Evidence of Execution
 - [x] Execution - Time

## Operating System Availability
 - [x] Windows 11 (⚠️ Requires audit policy configuration)
 - [x] Windows 10 (⚠️ Requires audit policy configuration)
 - [x] Windows 8 (⚠️ Requires audit policy configuration)
 - [x] Windows 7 (⚠️ Requires audit policy configuration)
 - [x] Windows Vista (⚠️ Requires audit policy configuration)
 - [x] Windows XP (⚠️ Requires audit policy configuration, Event ID `592`)
 - [x] Windows Server 2019 (⚠️ Requires audit policy configuration)
 - [x] Windows Server 2016 (⚠️ Requires audit policy configuration)
 - [x] Windows Server 2012 R2 (⚠️ Requires audit policy configuration)
 - [x] Windows Server 2012 (⚠️ Requires audit policy configuration)
 - [x] Windows Server 2008 R2 (⚠️ Requires audit policy configuration)
 - [x] Windows Server 2008 (⚠️ Requires audit policy configuration)
 - [x] Windows Server 2003 R2 (⚠️ Requires audit policy configuration, Event ID `592`)
 - [x] Windows Server 2003 (⚠️ Requires audit policy configuration, Event ID `592`)

## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Security.evtx`

## Artifact Interpretation

### Account - Logon ID
The `EventData/SubjectLogonId` field contains the [Logon ID](/README.md/#account---logon-id), which may be used to correlate this activity with a logon session recorded by event IDs such as:

 - [4624: An account was successfully logged on](/account/evtx-4624-successful-logon.md)
 - `4647: User initiated logoff `
 - `4634: An account was logged off `

### Execution - Evidence of Execution
The presence of a `4688` event indicates that the application listed in the field `EventData\NewProcessName` was executed. 

### Execution - Time
The timestamp of the event indicates the time at which the process listed in the field `EventData\NewProcessName` was executed.

### Execution - Permissions / Account
The following fields can be used to determine what account spawned the process:

| XML Path                  | Interpretation                                                            |
| ------------------------- | ------------------------------------------------------------------------- |
| EventData/SubjectUserSid | The [Security ID](/README.md/#account---security-identifier-sid) of the account that executed the process |
| EventData/SubjectUserName | The username of the account that executed the process |
| EventData/SubjectDomainName | The domain name of the account that executed the process |

The `EventData\TokenElevationType` field relates to UAC. The following interpretations are made possible by this field:

 - `%%1936` indicates that UAC is disabled on the system, or that the local administrator account launched the process.
 - `%%1937` indicates that the user manually ran the process as an administrator, or that the program requested administrative privileges upon execution.
 - `%%1938` indicates that the process did not run with administrator privileges. 

> [!NOTE]  
>  In the event that UAC was used to elevate permissions in order to execute an application, this information will be stored in the `Target` fields instead. 

### Execution - Command Line Options
In the event that Process Tracking is enabled (defaults to disabled), the full command line will be available in the `Process Command Line` field. 

In order for this to be available, the system's policy must be enabled under `Computer Configuration/Administrative Templates/System/Audit Process Creation/Include command line in process creation events`.

### Execution - Process Tree
The `New Process ID` and `Creator Process ID` fields may be used to create a timeline of processes.

## Examples
In the following example, `notepad.exe` was launched by a user through Explorer.

```
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-Security-Auditing" Guid="{54849625-5478-4994-a5ba-3e3b0328c30d}" /> 
  <EventID>4688</EventID> 
  <Version>2</Version> 
  <Level>0</Level> 
  <Task>13312</Task> 
  <Opcode>0</Opcode> 
  <Keywords>0x8020000000000000</Keywords> 
  <TimeCreated SystemTime="2023-06-25T19:46:45.3571632Z" /> 
  <EventRecordID>16044222</EventRecordID> 
  <Correlation /> 
  <Execution ProcessID="4" ThreadID="7776" /> 
  <Channel>Security</Channel> 
  <Computer>HLPC01</Computer> 
  <Security /> 
  </System>
- <EventData>
  <Data Name="SubjectUserSid">S-1-5-21-3471133136-2963561160-3931775028-1001</Data> 
  <Data Name="SubjectUserName">john.doe</Data> 
  <Data Name="SubjectDomainName">HLPC01</Data> 
  <Data Name="SubjectLogonId">0xd91ae</Data> 
  <Data Name="NewProcessId">0x5194</Data> 
  <Data Name="NewProcessName">C:\Windows\System32\notepad.exe</Data> 
  <Data Name="TokenElevationType">%%1938</Data> 
  <Data Name="ProcessId">0x25d0</Data> 
  <Data Name="CommandLine">"C:\Windows\system32\notepad.exe"</Data> 
  <Data Name="TargetUserSid">S-1-0-0</Data> 
  <Data Name="TargetUserName">-</Data> 
  <Data Name="TargetDomainName">-</Data> 
  <Data Name="TargetLogonId">0x0</Data> 
  <Data Name="ParentProcessName">C:\Windows\explorer.exe</Data> 
  <Data Name="MandatoryLabel">S-1-16-8192</Data> 
  </EventData>
  </Event>
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>

In the following example, `regedit.exe` was launched by a user through the command line. Note the `TokenElevationType` has a value of `%%1937`, indicating they were likely prompted for administrative credentials/permissions. This also causes the user information to be stored in the `Target` fields, as opposed to the `Subject` fields as seen in the previous example.
```
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-Security-Auditing" Guid="{54849625-5478-4994-a5ba-3e3b0328c30d}" /> 
  <EventID>4688</EventID> 
  <Version>2</Version> 
  <Level>0</Level> 
  <Task>13312</Task> 
  <Opcode>0</Opcode> 
  <Keywords>0x8020000000000000</Keywords> 
  <TimeCreated SystemTime="2023-06-25T19:48:53.8848049Z" /> 
  <EventRecordID>16044760</EventRecordID> 
  <Correlation /> 
  <Execution ProcessID="4" ThreadID="9852" /> 
  <Channel>Security</Channel> 
  <Computer>HLPC01</Computer> 
  <Security /> 
  </System>
- <EventData>
  <Data Name="SubjectUserSid">S-1-5-18</Data> 
  <Data Name="SubjectUserName">HLPC01$</Data> 
  <Data Name="SubjectDomainName">WORKGROUP</Data> 
  <Data Name="SubjectLogonId">0x3e7</Data> 
  <Data Name="NewProcessId">0x4c7c</Data> 
  <Data Name="NewProcessName">C:\Windows\regedit.exe</Data> 
  <Data Name="TokenElevationType">%%1937</Data> 
  <Data Name="ProcessId">0x4e24</Data> 
  <Data Name="CommandLine">"C:\Windows\regedit.exe"</Data> 
  <Data Name="TargetUserSid">S-1-5-21-3471133136-2963561160-3931775028-1001</Data> 
  <Data Name="TargetUserName">john.doe</Data> 
  <Data Name="TargetDomainName">HLPC01</Data> 
  <Data Name="TargetLogonId">0xd9173</Data> 
  <Data Name="ParentProcessName">C:\Windows\System32\cmd.exe</Data> 
  <Data Name="MandatoryLabel">S-1-16-12288</Data> 
  </EventData>
  </Event>
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>
