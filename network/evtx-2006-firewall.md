# Microsoft-Windows-Windows Firewall With Advanced Security/Firewall/2006: Firewall Rule Deleted
This event indicates that a firewall rule has been deleted.

### Analysis Value
 - [x] Account - Security Identifier (SID)
 - [x] Execution - Full Path
 - [x] Execution - Permissions / Account
 - [x] Execution - Process Tree
 - [x] Execution - Evidence of Execution
 - [x] Network Activity - Firewall Activity


## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista


## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Microsoft-Windows-Windows Firewall With Advanced Security%4Firewall.evtx`

## Artifact Interpretation

### Account - Security Identifier (SID)
The `System/Security/UserID` and `EventData/ModifyingUser` fields of this event provides the [Security Identifier (SID)](/README.md/#account---security-identifier-sid) of the account that deleted the firewall rule.

### Execution - Full Path
The full image path for the process that deleted the firewall rule will be available in the `EventData/ModifyingApplication` field of this event. 

### Execution - Permissions / Account
The `System/Security/UserID` and `EventData/ModifyingUser` fields of this event provides the [Security Identifier (SID)](/README.md/#account---security-identifier-sid) of the account that deleted the firewall rule.

### Execution - Process Tree
The Process and Thread ID of the process that deleted the firewall rule will be available in the `System/Execution/ProcessID` and `System/Execution/ThreadID` fields of this event, respectively. 

### Execution - Evidence of Execution
The presence of this event indicates that the executable present in the `EventData/ModifyingApplication` field of this event was run.

### Network Activity - Firewall Activity
The presence of this event indicates that the firewall rule specified by the event was deleted.

### Additional Fields
The following additional fields are available in this event, and they indicate the new parameters for the firewall rule that was deleted:

| XML Path                  | Interpretation                                                            |
| ------------------------- | ------------------------------------------------------------------------- |
| EventData/RuleId          | The firewall rule's GUID                                          |
| EventData/RuleName        | The name for the firewall rule as it appears in the Windows Firewall      |

## Example
On an example system, an existing Windows Firewall rule was deleted from within the Windows Defender Firewall with Advanced Security control panel, causing the following event to be logged:

```
- 
<Event
	xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
-   <System>
		<Provider Name="Microsoft-Windows-Windows Firewall With Advanced Security" Guid="{d1bc9aff-2abf-4d71-9146-ecb2a986eb85}" />
		<EventID>2006</EventID>
		<Version>0</Version>
		<Level>4</Level>
		<Task>0</Task>
		<Opcode>0</Opcode>
		<Keywords>0x8000020000000000</Keywords>
		<TimeCreated SystemTime="2023-05-04T17:26:06.0482710Z" />
		<EventRecordID>6669</EventRecordID>
		<Correlation />
		<Execution ProcessID="2352" ThreadID="18980" />
		<Channel>Microsoft-Windows-Windows Firewall With Advanced Security/Firewall</Channel>
		<Computer>HLPC01</Computer>
		<Security UserID="S-1-5-19" />
	</System>
-   <EventData>
		<Data Name="RuleId">{8736B31E-8792-452E-8D2D-C45621F236AF}</Data>
		<Data Name="RuleName">Open SSH Port 22</Data>
		<Data Name="ModifyingUser">S-1-5-21-3471133136-2963561160-3931775028-1001</Data>
		<Data Name="ModifyingApplication">C:\Windows\System32\mmc.exe</Data>
	</EventData>
</Event>
```

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>