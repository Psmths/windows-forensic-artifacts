# Amcache.hve
The Amcache hive stores metadata regarding launched executables on an endpoint. 

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)

### Analysis Value
 - [x] Execution - First Executed
 - [x] Execution - Evidence of Execution
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
| 0x006 | File Size |
| 0x011 | Last Modified Timestamp |
| 0x012 | Created Timestamp |
| 0x015 | File Path |
| 0x101 | File SHA-1 Hash |

For the SHA-1 hash, remove the leading 4 zeroes. 

For each numeric key, the last write time represents the first execution timestamp for the application. 