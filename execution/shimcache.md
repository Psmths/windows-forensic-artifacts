# ShimCache / Application Compatibility Cache
ShimCache is a registry artifact that may be used to determine:
 - Evidence of execution
 - File last modification time
 - File size
 - File path

## Operating System Availability
 - Windows 10
 - Windows 8
 - Windows 7
 - Windows Vista
 - Windows XP

## Artifact Location(s)
- Windows XP: `SYSTEM\CurrentControlSet\Control\SessionManager\AppCompatibility\AppCompatCache`
- Windows Vista/7/8/10: `SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache\AppCompatCache`

## Artifact Parsers
 - [ShimCacheParser.py](https://github.com/mandiant/ShimCacheParser)
 - Volatility's shimcachemem plugin

## Caveats
The ShimCache registry data is written on system shutdown only and the data extracted directly from the registry may be incomplete. Under these circumstances it is necessary to procure a memory dump to exctract a complete dataset from this forensic artifact. 