# Amcache.hve
The Amcache hive stores metadata regarding executables present on an endpoint. 

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)

### Analysis Value
 - [x] Execution - First Executed <sup>[1]</sup>
 - [x] Execution - Evidence of Execution <sup>[1]</sup>
 - [x] File - Hash
 - [x] File - Last Modified
 - [x] File - Path

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7 (⚠️ Most recent service packs)

## Artifact Location(s)
- `C:\Windows\AppCompat\Programs\Amcache. hve`

## Artifact Parsers
 - amcacheparser.exe (Eric Zimmerman)
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
The following key structure can be used to interpret this artifact. Note that the aforementioned parsers will extract this information automatically.

| Key | Interpretation | 
| - | - |
| 1 | Publisher Name |
| 6 | File Size |
| 11 | Last Modified Timestamp |
| 12 | Created Timestamp |
| 15 | File Path |
| 101 | File SHA-1 Hash |

For the SHA-1 hash, remove the leading 4 zeroes. 

For each numeric key, the last write time represents the first execution timestamp for the application. 

## Caveats
<sup>[1]</sup> This artifact alone does not absolutely indicate program execution. There are some instances where an item may be found in the Amcache hive that has not been executed. Consider enriching analysis with other execution artifacts to positively identify execution.

<span style="color:red">⚠️⚠️⚠️**There is a limit to the size of the data that gets hashed to produce this artifact's SHA-1 hash.** If the size of the binary exceeds approximately 30MB in size, only the first 30MB will be hashed. The result is that the SHA-1 hash will not be valid for that binary.⚠️⚠️⚠️</span>

## Analysis Tips

### Executables in Suspicious Locations
Search for amcache hive entries for executables that reside in suspicious locations, such as:

 - User `Downloads` directories
 - Other user profile directories such as `Desktop` or `Documents`
 - `C:\Temp`
 - `C:\PerfLogs`

### Suspicious Publishers
Typically, executables running under system directories such as `C:\Windows\System32` or `C:\Windows\SysWOW64` should have `microsoft` listed as the publisher. Because the Amcache hive provides publisher information, search for executables in these paths that were not published by Microsoft.

### IOC Queries
Assuming IOCs for malicious software are available, searching the Amcache hive for such indicators (SHA-1 hashes, executable name, publisher, etc.) may provide quick wins.
