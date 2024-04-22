# Microsoft-Windows-GroupPolicy/Operational/5312: List of applicable Group Policy objects
This event, caused by a previous Group Policy Object processing event, indicates that applicable GPOs were discovered for either a user or a computer. It provides a list of applicable GPOs that were processed.

### Analysis Value
 - [x] Network - Evidence of Network Activity

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista


## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Microsoft-Windows-GroupPolicy%4Operational.evtx`

## Artifact Interpretation

| XML Path                              | Interpretation  |
| ------------------------------------- | --------------- |
| System/Correlation ActivityID            | The ActivityID for this GPO update. This can be used to filter/correlate other events relating to this activity in both the System event log as well as the Group Policy Operational log. |
| EventData/GPOInfoList            | An XML string containing information regarding the new GPOs. |

To determine if this event was logged as a result of a Computer or User GPO update, look for either of the following events with the same `System/Correlation ActivityID`:

- User GPOs: [System/1503: The Group Policy settings for the user were processed successfully](/group-policy/evtx-1503-user-gpo-success.md)
- Comptuer GPOs: [System/1502: The Group Policy settings for the computer were processed successfully](/group-policy/evtx-1502-computer-gpo-success.md)

## Example
The following log is a result of a computer GPO update:
```
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
  <Provider Name="Microsoft-Windows-GroupPolicy" Guid="{aea1b4fa-97d1-45f2-a64c-4d69fffd92c9}" /> 
  <EventID>5312</EventID> 
  <Version>0</Version> 
  <Level>4</Level> 
  <Task>0</Task> 
  <Opcode>0</Opcode> 
  <Keywords>0x4000000000000000</Keywords> 
  <TimeCreated SystemTime="2024-04-22T19:48:02.8118477Z" /> 
  <EventRecordID>530</EventRecordID> 
  <Correlation ActivityID="{d739f467-3600-41b9-8b95-3cef52425544}" /> 
  <Execution ProcessID="13044" ThreadID="4444" /> 
  <Channel>Microsoft-Windows-GroupPolicy/Operational</Channel> 
  <Computer>DESKTOP-88KIKM6.example.lan</Computer> 
  <Security UserID="S-1-5-18" /> 
  </System>
- <EventData>
  <Data Name="DescriptionString">Power Policy DisableWebSearch Allow Unauthenticated SMB Access Default Domain Policy</Data> 
  <Data Name="GPOInfoList"><GPO ID="{07F79EFF-6675-4928-85E4-7929252F88AE}"><Name>Power Policy</Name><Version>524296</Version><SOM>LDAP://DC=example,DC=lan</SOM><FSPath>\\example.lan\SysVol\example.lan\Policies\{07F79EFF-6675-4928-85E4-7929252F88AE}\Machine</FSPath><Extensions>[{00000000-0000-0000-0000-000000000000}{9AD2BAFE-63B4-4883-A08C-C3C6196BCAFD}][{E62688F0-25FD-4C90-BFF5-F508B9D2E31F}{9AD2BAFE-63B4-4883-A08C-C3C6196BCAFD}]</Extensions></GPO><GPO ID="{D78067C3-E74E-4B8E-A3FC-C457BC32B0D3}"><Name>DisableWebSearch</Name><Version>458759</Version><SOM>LDAP://DC=example,DC=lan</SOM><FSPath>\\example.lan\SysVol\example.lan\Policies\{D78067C3-E74E-4B8E-A3FC-C457BC32B0D3}\Machine</FSPath><Extensions>[{35378EAC-683F-11D2-A89A-00C04FBBCFA2}{D02B1F72-3407-48AE-BA88-E8213C6761F1}][{7933F41E-56F8-41D6-A31C-4148A711EE93}{D02B1F72-3407-48AE-BA88-E8213C6761F1}]</Extensions></GPO><GPO ID="{0BFD7F89-67A1-402A-BED3-83F47FEDEBA3}"><Name>Allow Unauthenticated SMB Access</Name><Version>65537</Version><SOM>LDAP://DC=example,DC=lan</SOM><FSPath>\\example.lan\SysVol\example.lan\Policies\{0BFD7F89-67A1-402A-BED3-83F47FEDEBA3}\Machine</FSPath><Extensions>[{35378EAC-683F-11D2-A89A-00C04FBBCFA2}{D02B1F72-3407-48AE-BA88-E8213C6761F1}]</Extensions></GPO><GPO ID="{31B2F340-016D-11D2-945F-00C04FB984F9}"><Name>Default Domain Policy</Name><Version>196611</Version><SOM>LDAP://DC=example,DC=lan</SOM><FSPath>\\example.lan\sysvol\example.lan\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\Machine</FSPath><Extensions>[{35378EAC-683F-11D2-A89A-00C04FBBCFA2}{53D6AB1B-2488-11D1-A28C-00C04FB94F17}][{827D319E-6EAC-11D2-A4EA-00C04F79F83A}{803E14A0-B4FB-11D0-A0D0-00A0C90F574B}][{B1BE8D72-6EAC-11D2-A4EA-00C04F79F83A}{53D6AB1B-2488-11D1-A28C-00C04FB94F17}]</Extensions></GPO></Data> 
  </EventData>
  </Event>
```

We can see that one of the GPOs was `Allow Unauthenticated SMB Access`:

```
<GPO ID="{0BFD7F89-67A1-402A-BED3-83F47FEDEBA3}">
	<Name>Allow Unauthenticated SMB Access</Name>
	<Version>65537</Version>
	<SOM>LDAP://DC=exmample,DC=lan</SOM>
	<FSPath>\\example.lan\SysVol\example.lan\Policies\{0BFD7F89-67A1-402A-BED3-83F47FEDEBA3}\Machine</FSPath>
	<Extensions>[{35378EAC-683F-11D2-A89A-00C04FBBCFA2}{D02B1F72-3407-48AE-BA88-E8213C6761F1}]</Extensions>
</GPO>
```

<sup><sub>This example was produced on Windows 10, Version 10.0.19045 Build 19045</sub></sup>


