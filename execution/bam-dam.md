# Background Activity Montitor
The Background Activity Monitor and Desktop Activity Montior registry artifacts provide evidence of execution on an endpoint. It is only available in Windows 10 and Windows 11.

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)

### Analysis Value
 - [x] Account - Security Identifier (SID)
 - [x] Execution - Last Executed
 - [x] Execution - Permissions / Account
 - [x] Execution - Evidence of Execution
 - [x] File - Path
 
## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10 (⚠️ 1709 and later)


## Artifact Location(s)
🔋 Live System:

> Newer Windows 10 Versions
- BAM: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\bam\state\UserSettings\{USER_SID}`
- DAM: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\dam\state\UserSettings\{USER_SID}`

> Older Windows 10 Versions
- BAM: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\bam\UserSettings\{USER_SID}`
- DAM: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\dam\UserSettings\{USER_SID}`

🔌 Offline system:

> Newer Windows 10 Versions
- File: `%SystemRoot%\System32\config\SYSTEM`
- BAM: `SYSTEM\{CURRENT_CONTROL_SET}\Services\bam\state\UserSettings\{USER_SID}`
- DAM: `SYSTEM\{CURRENT_CONTROL_SET}\Services\dam\state\UserSettings\{USER_SID}`

> Older Windows 10 Versions
- File: `%SystemRoot%\System32\config\SYSTEM`
- BAM: `SYSTEM\{CURRENT_CONTROL_SET}\Services\bam\UserSettings\{USER_SID}`
- DAM: `SYSTEM\{CURRENT_CONTROL_SET}\Services\dam\UserSettings\{USER_SID}`

> [!NOTE]
> More information on [{CURRENT_CONTROL_SET}](/enumeration/select.md)


## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
The `Execution Time` as seen in RegistryExplorer represents the most recent time of execution for the binary in UTC. The `Program` field represents the full path the the binary. 

> [!NOTE]
> Based on testing, this execution time is written upon process creation, and again on termination.

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

### Example: Execution Timestamp
- Path: `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\bam\State\UserSettings\S-1-5-21-3471133136-2963561160-3931775028-1001`
- Key: `\Device\HarddiskVolume4\Program Files\PuTTY\putty.exe`
- Type: `REG_BINARY`
- Value: `60-9F-62-A8-FB-6C-D9-01-00-00-00-00-00-00-00-00-00-00-00-00-02-00-00-00`

The 64-Bit FILETIME timestamp `60-9F-62-A8-FB-6C-D9-01` resolves to `Wed 12 April 2023 05:00:10 UTC`, which is the last known execution of the binary `putty.exe`.

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>

## Caveats
> [!NOTE]  
> Applications that reside on file shares or detachable media such as USB drives will not produce BAM/DAM entries. Additionally, not all entries are permanent and may be purged from the Background Activity Monitor registry keys on boot after approximately one week.

> [!NOTE]  
> Applications that have been deleted from disk may also be removed from the Background Activity Monitor registry keys upon boot, but this does not happen all the time or in all cases.


## Analysis Tips

### Executables in Suspicious Locations
Search for BAM entries for executables that reside in suspicious locations, such as:

 - User `Downloads` directories
 - Other user profile directories such as `Desktop` or `Documents`
 - `C:\Temp`
 - `C:\PerfLogs`