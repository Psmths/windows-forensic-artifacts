# ShimCache / Application Compatibility Cache
ShimCache is a registry artifact of the application compatibility database to provide backwards-compatibility between operating system versions. It may provide forensic evidence of execution on a system.

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)

### Analysis Value
 - [x] Execution - Evidence of Execution
 - [x] File - Last Modified
 - [x] File - Path
 - [x] File - Size

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista
 - [x] Windows XP
 - [x] Windows Server 2019
 - [x] Windows Server 2016
 - [x] Windows Server 2012 R2
 - [x] Windows Server 2012
 - [x] Windows Server 2008 R2
 - [x] Windows Server 2008
 - [x] Windows Server 2003 R2
 - [x] Windows Server 2003

## Artifact Location(s)
🔋 Live System:
 - Windows XP: `SYSTEM\CurrentControlSet\Control\SessionManager\AppCompatibility\AppCompatCache`
 - Windows Vista/7/8/10/11: `SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache\AppCompatCache`

🔌 Offline system:
 - File: `%SystemRoot%\System32\config\SYSTEM`
 - Windows XP: `SYSTEM\{CURRENT_CONTROL_SET}\Control\SessionManager\AppCompatibility\AppCompatCache`
 - Windows Vista/7/8/10: `SYSTEM\{CURRENT_CONTROL_SET}\Control\Session Manager\AppCompatCache\AppCompatCache`

> [!NOTE]
> More information on [{CURRENT_CONTROL_SET}](/enumeration/select.md)

## Artifact Parsers
 - [ShimCacheParser.py](https://github.com/mandiant/ShimCacheParser)
 - Volatility's shimcachemem plugin

## Caveats
An entry into the ShimCache alone is not conclusive enough to prove execution, and this artifact should be cross-referenced with other similar artifacts to be certain of execution.

The ShimCache registry data is written on system shutdown only and the data extracted directly from the registry may be incomplete. Under these circumstances it is necessary to procure a memory dump to exctract a complete dataset from this forensic artifact. 
