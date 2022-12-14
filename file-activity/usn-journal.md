# USN Journal
The USN Journal is an artifact present on NTFS volumes that functions as the filesystem's journal. This artifact contains high-level records of operations taken on the filesystem. This artifact is present in volume shadow copies which may provide additional historical data. 

### Analysis Value
 - [x] File - Creation
 - [x] File - Deletion
 - [x] File - Last Modified
 - [x] File - Path
 - [x] File - Size


## Operating System Availability
 - NTFS Volumes

## Artifact Location(s)
 - `$Extend\$UsnJrnl\$J`

## Artifact Parsers
 - Velociraptor
 - jp (TZWorks)
 - MFTEcmd (Eric Zimmerman)
 - KAPE can be used to extract

## Artifact Interpretation
Below are several common USN Journal events:

| Event Code | Value | Description |
| - | - | - |
| USN_REASON_FILE_CREATE | 0x00000100 | File or directory has been created |
| USN_REASON_FILE_DELETE | 0x00000200 | File or directory has been deleted |
| USN_REASON_RENAME_NEW_NAME | 0x00002000 | File has been renamed (provides the new name) |
| USN_REASON_RENAME_OLD_NAME | 0x00001000 | File has been renamed (provides the old name) |
| USN_REASON_STREAM_CHANGE | 0x00200000 | An Alternate Data Stream has been added or removed or renamed from a file |
| USN_REASON_NAMED_DATA_EXTEND | 0x00000020 | An Alternate Data Stream has been added to |
| USN_REASON_DATA_EXTEND | 0x00000002 | File modification |
| USN_REASON_DATA_OVERWRITE | 0x00000001 | File modification |
| USN_REASON_DATA_TRUNCATION | 0x00000004 | File modification |
| USN_REASON_BASIC_INFO_CHANGE | 0x00008000 | File attributes have been modified |
| USN_REASON_SECURITY_CHANGE | 0x00000800 | File ownership/access writes have been modified |


## Caveats
The USN Journal is limited in size and therefore only recent activity will be reflected in this file. Depending on the amount of filesystem activity on a volume this artifact may only provide several days of coverage.