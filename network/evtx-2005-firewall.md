# Microsoft-Windows-Windows Firewall With Advanced Security/Firewall/2005: Firewall Rule Modified
This event indicates that a firewall rule has been modified. The contents of this event will contain the new parameters of the firewall rule.

> [!NOTE]
> In recent builds of Windows 11, this event has been replaced by a new event ID, [Microsoft-Windows-Windows Firewall With Advanced Security/Firewall/2073: Firewall Rule Modified](/network/evtx-2073-firewall-windows-11.md).

### Analysis Value
 - [x] Account - Security Identifier (SID)
 - [x] Execution - Full Path
 - [x] Execution - Permissions / Account
 - [x] Execution - Process Tree
 - [x] Execution - Evidence of Execution
 - [x] Network Activity - Firewall Activity


## Operating System Availability
 - [x] Windows 11 (⚠️ Event ID changed to 2073)
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista

## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Microsoft-Windows-Windows Firewall With Advanced Security%4Firewall.evtx`

## Artifact Interpretation
### Account - Security Identifier (SID)
The `System/Security/UserID` and `EventData/ModifyingUser` fields of this event provides the [Security Identifier (SID)](/README.md/#account---security-identifier-sid) of the account that modified the firewall rule.

### Execution - Full Path
The full image path for the process that modified the firewall rule will be available in the `EventData/ModifyingApplication` field of this event. 

### Execution - Permissions / Account
The `System/Security/UserID` and `EventData/ModifyingUser` fields of this event provides the [Security Identifier (SID)](/README.md/#account---security-identifier-sid) of the account that modified the firewall rule.

### Execution - Process Tree
The Process and Thread ID of the process that modified the firewall rule will be available in the `System/Execution/ProcessID` and `System/Execution/ThreadID` fields of this event, respectively. 

### Execution - Evidence of Execution
The presence of this event indicates that the executable present in the `EventData/ModifyingApplication` field of this event was run.

### Network Activity - Firewall Activity
The presence of this event indicates that the firewall rule specified by the event was modified.

### Additional Fields
The following additional fields are available in this event, and they indicate the new parameters for the firewall rule that was modified:

| XML Path                  | Interpretation                                                            |
| ------------------------- | ------------------------------------------------------------------------- |
| EventData/RuleId          | The firewall rule's GUID                                          |
| EventData/RuleName        | The name for the firewall rule as it appears in the Windows Firewall      |
| EventData/Direction       | 1 for inbound rules and 2 for outbound rules                              |
| EventData/Profiles        | What profiles (Private/Domain/Public) this rule applies to.               |
| EventData/Active          | 0 for disabled rules and 1 for enabled rules                              |
| EventData/Action          | 3 for allow and 2 for block                                               |
| EventData/SecurityOptions | 0 for none and 1 for require authentication                               |
| EventData/ApplicationPath | If the rule applies only to a specific application it will be listed here |
| EventData/ServiceName     | If the rule applies only to a specific service it will be listed here     |


## Example - Windows 10
On an example system, an existing Windows Firewall rule was modified from within the Windows Defender Firewall with Advanced Security control panel, causing the following event to be logged:

```
- 
<Event
	xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
-   <System>
		<Provider Name="Microsoft-Windows-Windows Firewall With Advanced Security" Guid="{d1bc9aff-2abf-4d71-9146-ecb2a986eb85}" />
		<EventID>2005</EventID>
		<Version>0</Version>
		<Level>4</Level>
		<Task>0</Task>
		<Opcode>0</Opcode>
		<Keywords>0x8000020000000000</Keywords>
		<TimeCreated SystemTime="2023-05-04T17:19:02.0655370Z" />
		<EventRecordID>6668</EventRecordID>
		<Correlation />
		<Execution ProcessID="2352" ThreadID="6516" />
		<Channel>Microsoft-Windows-Windows Firewall With Advanced Security/Firewall</Channel>
		<Computer>HLPC01</Computer>
		<Security UserID="S-1-5-19" />
	</System>
-   <EventData>
		<Data Name="RuleId">{8736B31E-8792-452E-8D2D-C45621F236AF}</Data>
		<Data Name="RuleName">Open SSH Port 22</Data>
		<Data Name="Origin">1</Data>
		<Data Name="ApplicationPath"></Data>
		<Data Name="ServiceName" />
		<Data Name="Direction">1</Data>
		<Data Name="Protocol">6</Data>
		<Data Name="LocalPorts">22</Data>
		<Data Name="RemotePorts">*</Data>
		<Data Name="Action">2</Data>
		<Data Name="Profiles">2147483647</Data>
		<Data Name="LocalAddresses">*</Data>
		<Data Name="RemoteAddresses">*</Data>
		<Data Name="RemoteMachineAuthorizationList" />
		<Data Name="RemoteUserAuthorizationList" />
		<Data Name="EmbeddedContext" />
		<Data Name="Flags">0</Data>
		<Data Name="Active">0</Data>
		<Data Name="EdgeTraversal">0</Data>
		<Data Name="LooseSourceMapped">0</Data>
		<Data Name="SecurityOptions">0</Data>
		<Data Name="ModifyingUser">S-1-5-21-3471133136-2963561160-3931775028-1001</Data>
		<Data Name="ModifyingApplication">C:\Windows\System32\mmc.exe</Data>
		<Data Name="SchemaVersion">542</Data>
		<Data Name="RuleStatus">65536</Data>
		<Data Name="LocalOnlyMapped">0</Data>
	</EventData>
</Event>
```

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>

## Example - Windows 11
On an example system, an existing Windows Firewall rule was modified from within the Windows Defender Firewall with Advanced Security control panel, causing the following event to be logged:

```
- 
<Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
-   <System>
		<Provider Name="Microsoft-Windows-Windows Firewall With Advanced Security" Guid="{d1bc9aff-2abf-4d71-9146-ecb2a986eb85}" />
		<EventID>2073</EventID>
		<Version>0</Version>
		<Level>4</Level>
		<Task>0</Task>
		<Opcode>0</Opcode>
		<Keywords>0x8000020000000000</Keywords>
		<TimeCreated SystemTime="2023-09-27T01:09:54.9122530Z" />
		<EventRecordID>550</EventRecordID>
		<Correlation />
		<Execution ProcessID="2704" ThreadID="3624" />
		<Channel>Microsoft-Windows-Windows Firewall With Advanced Security/Firewall</Channel>
		<Computer>W11</Computer>
		<Security UserID="S-1-5-19" />
	</System>
-   <EventData>
		<Data Name="RuleId">{0D458E97-4EC5-4C5C-A5A4-F9F73E769168}</Data>
		<Data Name="RuleName">Open SSH Port 22</Data>
		<Data Name="Origin">1</Data>
		<Data Name="ApplicationPath" />
		<Data Name="ServiceName" />
		<Data Name="Direction">1</Data>
		<Data Name="Protocol">6</Data>
		<Data Name="LocalPorts">22</Data>
		<Data Name="RemotePorts">*</Data>
		<Data Name="Action">3</Data>
		<Data Name="Profiles">2147483647</Data>
		<Data Name="LocalAddresses">*</Data>
		<Data Name="RemoteAddresses">*</Data>
		<Data Name="RemoteMachineAuthorizationList" />
		<Data Name="RemoteUserAuthorizationList" />
		<Data Name="EmbeddedContext" />
		<Data Name="Flags">0</Data>
		<Data Name="Active">0</Data>
		<Data Name="EdgeTraversal">0</Data>
		<Data Name="LooseSourceMapped">0</Data>
		<Data Name="SecurityOptions">0</Data>
		<Data Name="ModifyingUser">S-1-5-21-937911350-1118943250-2293061635-1001</Data>
		<Data Name="ModifyingApplication">C:\Windows\System32\mmc.exe</Data>
		<Data Name="SchemaVersion">544</Data>
		<Data Name="RuleStatus">65536</Data>
		<Data Name="LocalOnlyMapped">0</Data>
		<Data Name="ErrorCode">0</Data>
	</EventData>
</Event>
```

<sup><sub>This example was produced on Windows 11, Version 10.0.22621 Build 22621</sub></sup>