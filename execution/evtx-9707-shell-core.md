# Microsoft-Windows-Shell-Core/Operational/9707: Command Execution Started
This event indicates that a logon task, found in [Run/RunOnce Keys](/persistence/reg-run-runonce.md) has executed.

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)
 - [x] Behavioral - Persistence (TA0003)

### Analysis Value
 - [x] Account - Security Identifier (SID)
 - [x] Execution - Command Line Options
 - [x] Execution - Permissions / Account
 - [x] Execution - Process Tree
 - [x] Execution - Evidence of Execution
 - [x] Execution - Time

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista


## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Microsoft-Windows-Shell-Core%4Operational.evtx`

## Artifact Interpretation

### Account - Security Identifier (SID)
The `System/Security/UserID` field of this command provides the [Security Identifier (SID)](/README.md/#account---security-identifier-sid) of the account that triggered the command to run.

### Execution - Command Line Options
The full command line options for the command that was run will be available in the `EventData/Command` field of this event. 

### Execution - Process Tree
The Process and Thread ID of the command that was run will be available in the `System/Execution/ProcessID` and `System/Execution/ThreadID` fields of this event, respectively. 

### Execution - Evidence of Execution
The presence of this event indicates that the given command was run and that an entry either exists as a [Run Key](/persistence/reg-run-runonce.md) or existed as a [RunOnce Key](/persistence/reg-run-runonce.md).

### Execution - Time
The timestamp of the event indicates the time at which the command was run.

## Example
On an example system, the following registry key exists:

```
Path: HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Run\exampletask
Value: "C:\Temp\example.exe" -silent
```

During a user logon, the following `Microsoft-Windows-Shell-Core/Operational/9707` event is logged:

```
- 
<Event
	xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
-   <System>
		<Provider Name="Microsoft-Windows-Shell-Core" Guid="{30336ed4-e327-447c-9de0-51b652c86108}" />
		<EventID>9707</EventID>
		<Version>0</Version>
		<Level>4</Level>
		<Task>9707</Task>
		<Opcode>1</Opcode>
		<Keywords>0x2000000004010000</Keywords>
		<TimeCreated SystemTime="2023-04-30T15:57:41.0345751Z" />
		<EventRecordID>21933</EventRecordID>
		<Correlation />
		<Execution ProcessID="5072" ThreadID="11548" />
		<Channel>Microsoft-Windows-Shell-Core/Operational</Channel>
		<Computer>HLPC01</Computer>
		<Security UserID="S-1-5-21-3471133136-2963561160-3931775028-1001" />
	</System>
-   <EventData>
		<Data Name="Command">example.exe" -silent</Data>
	</EventData>
</Event>
```

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>