# Background Activity Montitor
The Background Activity Monitor and Desktop Activity Montior registry artifacts provide evidence of execution on an endpoint. It is only available in Windows 10 and Windows 11.

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)

### Analysis Value
 - [x] Execution - Last Executed
 - [x] Execution - Permissions / Account
 - [x] Execution - Evidence of Execution
 - [x] File - Path

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10

## Artifact Location(s)
- `SYSTEM\CurrentControlSet\Services\bam\state\UserSettings\{User SID}`
- `SYSTEM\CurrentControlSet\Services\dam\state\UserSettings\{User SID}`

On older version of Windows 10, this artifact has a different path:

- `SYSTEM\CurrentControlSet\Services\bam\UserSettings\{User SID}`
- `SYSTEM\CurrentControlSet\Services\dam\UserSettings\{User SID}`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
The `Execution Time` as seen in RegistryExplorer represents the most recent time of execution for the binary. **Based on testing, this execution time is written upon process termination.** The `Program` field represents the full path the the binary. 

In the event that you are parsing or interpreting this artifact manually, the following CyberChef recipe can be used to convert Windows FILETIME timestamps to a date and time:

```
[
  { "op": "From Hex",
    "args": ["Auto"] },
  { "op": "To Hex",
    "args": ["None", 0] },
  { "op": "Windows Filetime to UNIX Timestamp",
    "args": ["Milliseconds (ms)", "Hex (little endian)"] },
  { "op": "From UNIX Timestamp",
    "args": ["Milliseconds (ms)"] }
]
```

## Analysis Tips

### Executables in Suspicious Locations
Search for amcache hive entries for executables that reside in suspicious locations, such as:

 - User `Downloads` directories
 - Other user profile directories such as `Desktop` or `Documents`
 - `C:\Temp`
 - `C:\PerfLogs`

## Example
![Example Image](/media/examples/bam.png)