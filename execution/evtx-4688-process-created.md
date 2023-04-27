# Security/4688: A new process has been created
This event, logged to the Security channel, indicates a process was created on the system.

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)

### Analysis Value
 - [x] Account - Logon ID
 - [x] Account - Security Identifier (SID)
 - [x] Execution - Command Line Options
 - [x] Execution - Permissions / Account
 - [x] Execution - Process Tree
 - [x] Execution - Evidence of Execution
 - [x] Execution - Time

## Operating System Availability
 - [x] Windows 11 (⚠️ Requires manual enable)
 - [x] Windows 10 (⚠️ Requires manual enable)
 - [x] Windows 8 (⚠️ Requires manual enable)
 - [x] Windows 7 (⚠️ Requires manual enable)
 - [x] Windows Vista (⚠️ Requires manual enable)
 - [x] Windows XP (⚠️ Requires manual enable)
 - [x] Windows Server 2019 (⚠️ Requires manual enable)
 - [x] Windows Server 2016 (⚠️ Requires manual enable)
 - [x] Windows Server 2012 R2 (⚠️ Requires manual enable)
 - [x] Windows Server 2012 (⚠️ Requires manual enable)
 - [x] Windows Server 2008 R2 (⚠️ Requires manual enable)
 - [x] Windows Server 2008 (⚠️ Requires manual enable)
 - [x] Windows Server 2003 R2 (⚠️ Requires manual enable)
 - [x] Windows Server 2003 (⚠️ Requires manual enable)

## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Security.evtx`

## Artifact Interpretation

### Time of Execution
The timestamp of the event indicates the time at which a process was started.

### Executing Account
The following fields can be used to determine what account spawned the process:

 - [Security ID](/README.md/#account---security-identifier-sid)
 - `Account Name`
 - `Account Domain`
  
### Correlation to Logon Sessions
The [Logon ID](/README.md/#account---logon-id) field may be used to correlate this activity with a logon session recorded by event IDs such as:
 - [`4624: An account was successfully logged on`](/account/evtx-4624-successful-logon.md)
 - `4647: User initiated logoff `
 - `4634: An account was logged off `

### Process Command Line
In the event that Process Tracking is enabled (defaults to disabled), the full command line will be available in the `Process Command Line` field. 

In order for this to be available, the system's policy must be enabled under `Computer Configuration/Administrative Templates/System/Audit Process Creation/Include command line in process creation events`.

### Process Genealogy
The `New Process ID` and `Creator Process ID` fields may be used to create a timeline of processes.

### Process Permissions
The `Token Elevation Type` field relates to UAC. The following interpretations are made possible by this field:

 - `%%1936` indicates that UAC is disabled on the system, or that the local administrator account launched the process.
 - `%%1937` indicates that the user manually ran the process as an administrator, or that the program requested administrative privileges upon execution.
 - `%%1938` indicates that the process did not run with administrator privileges. 