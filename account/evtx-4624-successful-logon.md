# Security/4624: An account was successfully logged on
This event indicates an account has successfuly authenticated to the endpoint. It is logged on the **destination** endpoint. 

This event is a **Logon Event**, meaning it is logged on the system that is being authenticated to. 

> [!NOTE]
> In windows XP, the corresponding Event ID is `528`.

### Behavioral Indications
 - [x] Behavioral - Lateral Movement (TA0008)

### Analysis Value
 - [x] Account - Login History
 - [x] Account - Logon ID
 - [x] Account - Security Identifier (SID)
 - [x] Account - Username
 - [x] Network Activity - Source Identification


## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista
 - [x] Windows XP

## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Security.evtx`

## Artifact Interpretation

### Account - Security Identifier (SID)
The `EventData/TargetUserSid` field contains the SID of the account that authenticated. 

### Account - Logon ID
The `EventData/TargetLogonId` field contains the [Logon ID](/README.md/#account---logon-id) of the session that was authenticated. This field is of interest as it can be used to cross-reference other events found in the Windows Event Log and tie activity to a particular logon session.

### Account - Username
The `EventData/TargetUserName` will contain the username associated with the authenticating account. 

### Network Activity - Source Identification
The `EventData/IpAddress` field of this event will contain the **source** address for the session. 

 - For local logons, such as the user signing into the system through native keyboard and mouse, the `EventData/IpAddress` will be `127.0.0.1` or a null value. 

## Analysis Tips

### Analysis of LogonType
The `EventData/LogonType` provides information regarding what type of logon occurred. The following `LogonType` values are available:

| Logon Type | Description |
| - | - |
| 2 | **Interactive** (user loggong on through screen or virtual console) |
| 3 | **Network** (RDP with NLA enabled) |
| 7 | **Unlock** (RDP reconnects or interactive unlocking) |
| 9 | **Explicit** credentials (`runas`) |
| 10 | **Remote** Interactive (RDP with NLA diasbled) |

Some analysis tips for certain situations:

# Local/Physical User Logon
When a user authenticates physically to the system, the resulting `LogonType` will typically be 2, or in the event that cached credentials were used to authenticate the session, 11. If the user has unlocked the system, there will be a logon type 7 event. 

For example, a physical logon would result in the following event being logged,

Note the values of the following fields:

| Field | Value |
| - | - |
| EventData/LogonType | 2 |
| EventData/ProcessName | C:\Windows\System32\svchost.exe |
| EventData/IpAddress | 127.0.0.1 |

```
- 
<Event
	xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
-   <EventData>
		<Data Name="SubjectUserSid">S-1-5-18</Data>
		<Data Name="SubjectUserName">HLPC01$</Data>
		<Data Name="SubjectDomainName">WORKGROUP</Data>
		<Data Name="SubjectLogonId">0x3e7</Data>
		<Data Name="TargetUserSid">S-1-5-21-3471133136-2963561160-3931775028-1001</Data>
		<Data Name="TargetUserName">user</Data>
		<Data Name="TargetDomainName">HLPC01</Data>
		<Data Name="TargetLogonId">0x34358d</Data>
		<Data Name="LogonType">2</Data>
		<Data Name="LogonProcessName">User32</Data>
		<Data Name="AuthenticationPackageName">Negotiate</Data>
		<Data Name="WorkstationName">HLPC01</Data>
		<Data Name="LogonGuid">{00000000-0000-0000-0000-000000000000}</Data>
		<Data Name="TransmittedServices">-</Data>
		<Data Name="LmPackageName">-</Data>
		<Data Name="KeyLength">0</Data>
		<Data Name="ProcessId">0x7e4</Data>
		<Data Name="ProcessName">C:\Windows\System32\svchost.exe</Data>
		<Data Name="IpAddress">127.0.0.1</Data>
		<Data Name="IpPort">0</Data>
		<Data Name="ImpersonationLevel">%%1833</Data>
		<Data Name="RestrictedAdminMode">-</Data>
		<Data Name="TargetOutboundUserName">-</Data>
		<Data Name="TargetOutboundDomainName">-</Data>
		<Data Name="VirtualAccount">%%1843</Data>
		<Data Name="TargetLinkedLogonId">0x3435b7</Data>
		<Data Name="ElevatedToken">%%1842</Data>
	</EventData>
</Event>
```

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>

In situations when cached credentials were used to authenticate a session, the physical logon might look like this:

| Field | Value |
| - | - |
| EventData/LogonType | 11 |
| EventData/ProcessName | C:\Windows\System32\svchost.exe |
| EventData/IpAddress | 127.0.0.1 |

```
- 
<Event
	xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
-   <EventData>
		<Data Name="SubjectUserSid">S-1-5-18</Data>
		<Data Name="SubjectUserName">WKS10-01$</Data>
		<Data Name="SubjectDomainName">HLAB</Data>
		<Data Name="SubjectLogonId">0x3e7</Data>
		<Data Name="TargetUserSid">S-1-5-21-3829912423-625253200-3062624365-1107</Data>
		<Data Name="TargetUserName">ablaser</Data>
		<Data Name="TargetDomainName">HLAB</Data>
		<Data Name="TargetLogonId">0x4d742</Data>
		<Data Name="LogonType">11</Data>
		<Data Name="LogonProcessName">User32</Data>
		<Data Name="AuthenticationPackageName">Negotiate</Data>
		<Data Name="WorkstationName">WKS10-01</Data>
		<Data Name="LogonGuid">{00000000-0000-0000-0000-000000000000}</Data>
		<Data Name="TransmittedServices">-</Data>
		<Data Name="LmPackageName">-</Data>
		<Data Name="KeyLength">0</Data>
		<Data Name="ProcessId">0x6ec</Data>
		<Data Name="ProcessName">C:\Windows\System32\svchost.exe</Data>
		<Data Name="IpAddress">127.0.0.1</Data>
		<Data Name="IpPort">0</Data>
		<Data Name="ImpersonationLevel">%%1833</Data>
		<Data Name="RestrictedAdminMode">-</Data>
		<Data Name="TargetOutboundUserName">-</Data>
		<Data Name="TargetOutboundDomainName">-</Data>
		<Data Name="VirtualAccount">%%1843</Data>
		<Data Name="TargetLinkedLogonId">0x0</Data>
		<Data Name="ElevatedToken">%%1843</Data>
	</EventData>
</Event>
```

<sup><sub>This example was produced on Windows 10 Pro, Version 10.0.19044 Build 19044</sub></sup>

# RunAs Activity
`RunAs` is a command-line utility used to execute programs with different permissions. Using `RunAs` to perform this action will also result in a type 2 logon. In the following example, take note of the `EventData/SubjectUserName` field, which indicated what user executed `RunAs`. The `EventData/TargetUserName` field contains the account name whose credentials were used. In addition, the `EventData/SubjectLogonId` is the same as the `EventData/TargetLogonId` in the previous example of cached credential authentication. This indicates that `HLAB\ablaser` authenticated to the system, and then used `RunAs` to run a command as `HLAB\mvanburanadm`.

```
- 
<Event
	xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
-   <EventData>
		<Data Name="SubjectUserSid">S-1-5-21-3829912423-625253200-3062624365-1107</Data>
		<Data Name="SubjectUserName">ablaser</Data>
		<Data Name="SubjectDomainName">HLAB</Data>
		<Data Name="SubjectLogonId">0x4d742</Data>
		<Data Name="TargetUserSid">S-1-5-21-3829912423-625253200-3062624365-1105</Data>
		<Data Name="TargetUserName">mvanburanadm</Data>
		<Data Name="TargetDomainName">HLAB</Data>
		<Data Name="TargetLogonId">0xc0cd5</Data>
		<Data Name="LogonType">2</Data>
		<Data Name="LogonProcessName">seclogo</Data>
		<Data Name="AuthenticationPackageName">Negotiate</Data>
		<Data Name="WorkstationName">WKS10-01</Data>
		<Data Name="LogonGuid">{22acd001-6c49-8d9e-8c4f-c1fd908d1c0e}</Data>
		<Data Name="TransmittedServices">-</Data>
		<Data Name="LmPackageName">-</Data>
		<Data Name="KeyLength">0</Data>
		<Data Name="ProcessId">0x2108</Data>
		<Data Name="ProcessName">C:\Windows\System32\svchost.exe</Data>
		<Data Name="IpAddress">::1</Data>
		<Data Name="IpPort">0</Data>
		<Data Name="ImpersonationLevel">%%1833</Data>
		<Data Name="RestrictedAdminMode">-</Data>
		<Data Name="TargetOutboundUserName">-</Data>
		<Data Name="TargetOutboundDomainName">-</Data>
		<Data Name="VirtualAccount">%%1843</Data>
		<Data Name="TargetLinkedLogonId">0xc0f24</Data>
		<Data Name="ElevatedToken">%%1842</Data>
	</EventData>
</Event>
```

<sup><sub>This example was produced on Windows 10 Pro, Version 10.0.19044 Build 19044</sub></sup>

# Remote Desktop Activity
The appearance of Remote Desktop activity from this artifact depends on several factors:

 - For new RDP logons, a type 10 logon event is logged
 - For pre-existing logons, a type 7 logon event is logged
 - Assuming Network Level Authentication (NLA) is required on the system for RDP, there will be a type 3 logon event preceeding either the type 7 or type 10 event

For example, an RDP session from another system on the local network (172.16.200.2), with NLA enabled, and with a previous RDP session that was not formally logged out would create the following two events:

Type 3 Logon Event
```
- 
<Event
	xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
-   <EventData>
		<Data Name="SubjectUserSid">S-1-0-0</Data>
		<Data Name="SubjectUserName">-</Data>
		<Data Name="SubjectDomainName">-</Data>
		<Data Name="SubjectLogonId">0x0</Data>
		<Data Name="TargetUserSid">S-1-5-21-3829912423-625253200-3062624365-1105</Data>
		<Data Name="TargetUserName">mvanburanadm</Data>
		<Data Name="TargetDomainName">HLAB</Data>
		<Data Name="TargetLogonId">0x4f5277</Data>
		<Data Name="LogonType">3</Data>
		<Data Name="LogonProcessName">NtLmSsp</Data>
		<Data Name="AuthenticationPackageName">NTLM</Data>
		<Data Name="WorkstationName">HLNAS01-WS2K19</Data>
		<Data Name="LogonGuid">{00000000-0000-0000-0000-000000000000}</Data>
		<Data Name="TransmittedServices">-</Data>
		<Data Name="LmPackageName">NTLM V2</Data>
		<Data Name="KeyLength">128</Data>
		<Data Name="ProcessId">0x0</Data>
		<Data Name="ProcessName">-</Data>
		<Data Name="IpAddress">172.16.200.2</Data>
		<Data Name="IpPort">0</Data>
		<Data Name="ImpersonationLevel">%%1833</Data>
		<Data Name="RestrictedAdminMode">-</Data>
		<Data Name="TargetOutboundUserName">-</Data>
		<Data Name="TargetOutboundDomainName">-</Data>
		<Data Name="VirtualAccount">%%1843</Data>
		<Data Name="TargetLinkedLogonId">0x0</Data>
		<Data Name="ElevatedToken">%%1842</Data>
	</EventData>
</Event>
```

Type 7 Logon Event (Unlocked)
```
- 
<Event
	xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <EventData>
		<Data Name="SubjectUserSid">S-1-5-18</Data>
		<Data Name="SubjectUserName">WKS10-01$</Data>
		<Data Name="SubjectDomainName">HLAB</Data>
		<Data Name="SubjectLogonId">0x3e7</Data>
		<Data Name="TargetUserSid">S-1-5-21-3829912423-625253200-3062624365-1105</Data>
		<Data Name="TargetUserName">mvanburanadm</Data>
		<Data Name="TargetDomainName">HLAB</Data>
		<Data Name="TargetLogonId">0x4fbf34</Data>
		<Data Name="LogonType">7</Data>
		<Data Name="LogonProcessName">User32</Data>
		<Data Name="AuthenticationPackageName">Negotiate</Data>
		<Data Name="WorkstationName">WKS10-01</Data>
		<Data Name="LogonGuid">{00000000-0000-0000-0000-000000000000}</Data>
		<Data Name="TransmittedServices">-</Data>
		<Data Name="LmPackageName">-</Data>
		<Data Name="KeyLength">0</Data>
		<Data Name="ProcessId">0x6ec</Data>
		<Data Name="ProcessName">C:\Windows\System32\svchost.exe</Data>
		<Data Name="IpAddress">172.16.200.2</Data>
		<Data Name="IpPort">0</Data>
		<Data Name="ImpersonationLevel">%%1833</Data>
		<Data Name="RestrictedAdminMode">-</Data>
		<Data Name="TargetOutboundUserName">-</Data>
		<Data Name="TargetOutboundDomainName">-</Data>
		<Data Name="VirtualAccount">%%1843</Data>
		<Data Name="TargetLinkedLogonId">0x4fbf85</Data>
		<Data Name="ElevatedToken">%%1842</Data>
	</EventData>
</Event>
```

<sup><sub>This example was produced on Windows Server 2019 Standard, Version 10.0.17763 Build 17763</sub></sup>

If there was not a previous and still active RDP connection, the Type 7 Logon event would have instead been logged as a Type 10 Logon event. 

# File Server Access
In the event that a remote system authenticates to a file server to access file shares, the resulting Logon event will be of Type 3, with the IP address of the authenticating system in the `EventData\IpAddress` field. This can be useful for auditing potential file access on network shares.