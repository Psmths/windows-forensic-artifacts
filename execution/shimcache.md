# ShimCache / Application Compatibility Cache
ShimCache is a registry artifact of the application compatibility database to provide backwards-compatibility between operating system versions.

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

## Artifact Location(s)
- Windows XP: `SYSTEM\CurrentControlSet\Control\SessionManager\AppCompatibility\AppCompatCache`
- Windows Vista/7/8/10: `SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache\AppCompatCache`

## Artifact Parsers
 - [ShimCacheParser.py](https://github.com/mandiant/ShimCacheParser)
 - Volatility's shimcachemem plugin

## Caveats
The ShimCache registry data is written on system shutdown only and the data extracted directly from the registry may be incomplete. Under these circumstances it is necessary to procure a memory dump to exctract a complete dataset from this forensic artifact. 