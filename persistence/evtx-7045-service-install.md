# System/7045: Service Installed
This event, logged to the System channel, is logged when a new service is installed on the system. 

### Behavioral Indications
 - [x] Behavioral - Persistence (TA0003)

### Analysis Value
 - [x] Account - Security Identifier (SID)
 - [x] File - Path

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista

## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\System.evtx`

## Artifact Interpretation
The presence of this event in the System log indicates that a Service was installed on the system at the time the event was logged. There is no indication from this event alone that it was installed locally on the system itself, and services may be installed remotely leveraging utilities such as `sc.exe`.

### Account - Security Identifier (SID)
The `System/Security/UserID` field of this event provides the [Security Identifier (SID)](/README.md/#account---security-identifier-sid) of the account that installed the new service.

### File - Path
The `EventData\ImagePath` field of this event indicates the full path to the executable that will be run when the new service is started.ss

## Analysis Tips
In the event that the new service was installed remotely, a [Security/4624: An account was successfully logged on](account/evtx-4624-successful-logon.md) event may be logged before the new service is installed with a `LogonType` of 3. 

## Example
In the following example, the following command was executed on a domain controller:

```
sc.exe \\WKS10-01 create mynewservice binpath= c:\temp\example.exe start= auto displayname= "My new service"
```

This installed a new service on the system WKS10-01, generating the following [Security/4624: An account was successfully logged on](account/evtx-4624-successful-logon.md) event:

```
-
<Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">-
	<System>
		<ProviderName="Microsoft-Windows-Security-Auditing"Guid="{54849625-5478-4994-a5ba-3e3b0328c30d}"/>
		<EventID>4624</EventID>
		<Version>2</Version>
		<Level>0</Level>
		<Task>12544</Task>
		<Opcode>0</Opcode>
		<Keywords>0x8020000000000000</Keywords>
		<TimeCreatedSystemTime="2023-05-08T22:04:15.0931035Z"/>
		<EventRecordID>9719</EventRecordID>
		<CorrelationActivityID="{96cea955-81f8-0004-8ea9-ce96f881d901}"/>
		<ExecutionProcessID="900"ThreadID="3128"/>
		<Channel>Security</Channel>
		<Computer>WKS10-01.hlab.com</Computer>
		<Security/>
	</System>-
	<EventData>
		<DataName="SubjectUserSid">S-1-0-0</Data>
		<DataName="SubjectUserName">-</Data>
		<DataName="SubjectDomainName">-</Data>
		<DataName="SubjectLogonId">0x0</Data>
		<DataName="TargetUserSid">S-1-5-21-3829912423-625253200-3062624365-1105</Data>
		<DataName="TargetUserName">mvanburanadm</Data>
		<DataName="TargetDomainName">HLAB</Data><DataName="TargetLogonId">0xe4d79</Data>
		<DataName="LogonType">3</Data>
		<DataName="LogonProcessName">NtLmSsp</Data>
		<DataName="AuthenticationPackageName">NTLM</Data>
		<DataName="WorkstationName">HLDC01-WS2K19</Data>
		<DataName="LogonGuid">{00000000-0000-0000-0000-000000000000}</Data>
		<DataName="TransmittedServices">-</Data>
		<DataName="LmPackageName">NTLMV2</Data>
		<DataName="KeyLength">128</Data>
		<DataName="ProcessId">0x0</Data>
		<DataName="ProcessName">-</Data>
		<DataName="IpAddress">172.16.100.10</Data>
		<DataName="IpPort">49757</Data>
		<DataName="ImpersonationLevel">%%1833</Data>
		<DataName="RestrictedAdminMode">-</Data>
		<DataName="TargetOutboundUserName">-</Data>
		<DataName="TargetOutboundDomainName">-</Data>
		<DataName="VirtualAccount">%%1843</Data>
		<DataName="TargetLinkedLogonId">0x0</Data>
		<DataName="ElevatedToken">%%1842</Data>
	</EventData>
</Event>
```

As well as the following `System/7045: Service Installed` event in the `System` channel:

```
-
<Event
	xmlns="http://schemas.microsoft.com/win/2004/08/events/event">-
	<System>
		<ProviderName="ServiceControlManager"Guid="{555908d1-a6d7-4695-8e1e-26931d2012f4}"EventSourceName="ServiceControlManager"/>
		<EventIDQualifiers="16384">7045
		</EventID>
		<Version>0</Version>
		<Level>4</Level>
		<Task>0</Task>
		<Opcode>0</Opcode>
		<Keywords>0x8080000000000000</Keywords>
		<TimeCreatedSystemTime="2023-05-08T22:04:15.0831494Z"/>
		<EventRecordID>1033</EventRecordID>
		<Correlation/>
		<ExecutionProcessID="880"ThreadID="968"/>
		<Channel>System</Channel>
		<Computer>WKS10-01.hlab.com</Computer>
		<SecurityUserID="S-1-5-21-3829912423-625253200-3062624365-1105"/>
	</System>-
	<EventData>
		<DataName="ServiceName">Mynewservice</Data>
		<DataName="ImagePath">c:\temp\example.exe</Data>
		<DataName="ServiceType">usermodeservice</Data>
		<DataName="StartType">autostart</Data>
		<DataName="AccountName">LocalSystem</Data>
	</EventData>
</Event>
```

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>