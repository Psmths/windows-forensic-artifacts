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

### Example: File Creation and Deletion
In this example, we created a file `test.txt`, modified its contents, and then deleted it. The resulting information from the USN Journal is as follows:

| Timestamp              | Filename              | Update Reason               |
| ---------------------- | --------------------- | --------------------------- |
| 2023-04-26 23:51:09.09 | New Text Document.txt | FileCreate                  |
| 2023-04-26 23:51:09.09 | New Text Document.txt | FileCreate,Close            |
| 2023-04-26 23:51:16.16 | New Text Document.txt | RenameOldName               |
| 2023-04-26 23:51:16.16 | test.txt              | RenameNewName               |
| 2023-04-26 23:51:16.16 | test.txt              | RenameNewName,Close         |
| 2023-04-26 23:51:22.22 | test.txt              | DataExtend                  |
| 2023-04-26 23:51:22.22 | test.txt              | DataExtend,Close            |
| 2023-04-26 23:51:29.29 | $I4NTH4K.txt          | FileCreate                  |
| 2023-04-26 23:51:29.29 | $I4NTH4K.txt          | DataExtend,FileCreate       |
| 2023-04-26 23:51:29.29 | $I4NTH4K.txt          | DataExtend,FileCreate,Close |
| 2023-04-26 23:51:29.29 | test.txt              | RenameOldName               |
| 2023-04-26 23:51:29.29 | $R4NTH4K.txt          | RenameNewName               |
| 2023-04-26 23:51:29.29 | $R4NTH4K.txt          | RenameNewName,Close         |
| 2023-04-26 23:51:29.29 | $R4NTH4K.txt          | SecurityChange              |
| 2023-04-26 23:51:29.29 | $R4NTH4K.txt          | SecurityChange,Close        |

In this example, we first see that a new text file is created, called `New Text Document.txt` (`FileCreate`), indicating that it was likely created by right-clicking in Explorer. It is then renamed to `test.txt` (`RenameNewName`). Afterwards, its contents are modified (`DataExtend`). The file is then "deleted," being sent to the recycle bin. This is evidenced by the creation of the `$I` and `$R` files. As expected, the `$R4NTH4K.txt` file should contain the full contents of the deleted file, and we see that Windows simply renames the original file to this. 

> [!NOTE]
> More information on [Recycle Bin $I/$R Files](/file-activity/recycle-bin-files.md)

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>

### Example: Moving a File
In this example, a file has been moved to a different directory. In this instance, while the filename remains the same, we see that the update reasons `RenameOldName` and `RenameNewName` are present. The parent entry numbers (as well as the parent sequence numbers, not shown in this table) change, indicating that the file has been moved to a different directory. 

| Timestamp              | Parent Entry Number | Filename              | Update Reason        |
| ---------------------- | ------------------- | --------------------- | -------------------- |
| 2023-04-26 23:51:41.41 | 114291              | test2.txt             | RenameOldName        |
| 2023-04-26 23:51:41.41 | 101882              | test2.txt             | RenameNewName        |
| 2023-04-26 23:51:41.41 | 101882              | test2.txt             | RenameNewName,Close  |
| 2023-04-26 23:51:41.41 | 101882              | test2.txt             | SecurityChange       |
| 2023-04-26 23:51:41.41 | 101882              | test2.txt             | SecurityChange,Close |

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>

### Example: Correlation with Prefetch
| UpdateTimestamp        | Name                    | UpdateReasons                   |
| ---------------------- | ----------------------- | ------------------------------- |
| 2023-04-26 23:51:16.16 | New Text Document.txt   | RenameOldName                   |
| 2023-04-26 23:51:16.16 | test.txt                | RenameNewName                   |
| 2023-04-26 23:51:22.22 | test.txt                | DataExtend                      |
| 2023-04-26 23:51:22.22 | test.txt                | DataExtend,Close                |
| 2023-04-26 23:51:23.23 | NOTEPAD.EXE-9FB27C0E.pf | DataTruncation                  |
| 2023-04-26 23:51:23.23 | NOTEPAD.EXE-9FB27C0E.pf | DataExtend,DataTruncation       |
| 2023-04-26 23:51:23.23 | NOTEPAD.EXE-9FB27C0E.pf | DataExtend,DataTruncation,Close |


In this example, we see that `notepad.exe` was likely executed to edit `text.txt`. This is particularly valuable as the [Prefetch](/execution/prefetch.md) artifact only stores the last 8 execution timestamps of an application, but it is updated for each execution, meaning the USN Journal may provide additional execution timestamps that have rolled out of the Prefetch file. 

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>

## Caveats
The USN Journal is limited in size and therefore only recent activity will be reflected in this file. Depending on the amount of filesystem activity on a volume this artifact may only provide several days (or even hours) of coverage. A potential workaround to obtain more history from this artifact would be to extract a copy of it from any available Volume Shadow Copies present on the NTFS volume. 