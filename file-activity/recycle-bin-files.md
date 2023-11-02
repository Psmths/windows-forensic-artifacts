# Recycle Bin $I/$R Files
In modern versions of Windows, when a file is deleted, it is sent to the Recycle Bin first. Under the hood this constitutes the creation of two unique files prepended by `$I` and `$R` and appended with the original file's extension such as:

 - `$RMYY8AS.txt`
 - `$IMYY8AS.txt`

The `$I` file contains information about the deleted file. The `$R` file contains the full contents of that deleted file.

### Analysis Value
 - [x] Account - Security Identifier (SID)
 - [x] File - Deletion
 - [x] File - Size
 - [x] File - Path

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista

## Artifact Location(s)
 - `C:\$Recycle.Bin\{USER_SID}`

## Artifact Parsers
 - KAPE (Extraction and Parsing)
 - [RBCmd](https://github.com/EricZimmerman/RBCmd)

## Artifact Interpretation
The presence of this artifact indicates that a file was deleted and sent to the recycle bin.

The user who deleted the file will have their SID shown as the parent directory for the `$I` and `$R` files, for example:

 - `C:\$Recycle.Bin\S-1-5-21-3471133136-2963561160-3931775028-1000\$RMYY8AS.txt`
 - `C:\$Recycle.Bin\S-1-5-21-3471133136-2963561160-3931775028-1000\$IMYY8AS.txt`

In this case, `S-1-5-21-3471133136-2963561160-3931775028-1000` is the User SID. 

The `$R` file contains the full contents of the original deleted file. 

The `$I` file contains the following data:

 - Size of the original file
 - Full path of the original file
 - Deletion time and date

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>