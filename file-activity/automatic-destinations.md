# AutomaticDestinations Jumplists
AutomaticDestinations are jumplist files created by Windows when an application is launched. As these are jumplist files, they are in a binary format known as Object Linking and Embedding Compound File (OLECF). AutomaticDestinations can be thought of as containers for different LNK files from which forensic evidence may be recovered. 

Jumplists are used to store common/recent locations and "tasks" on the taskbar for individual programs. From a forensic perspective they are useful in identifying files and folders that were created or accessed by users. 

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)
 
### Analysis Value
 - [x] Execution - First Executed ⚠️
 - [x] Execution - Last Executed ⚠️
 - [x] Execution - Permissions / Account
 - [x] Execution - Evidence of Execution
 - [x] Account - Username
 - [x] File - Path

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7

## Artifact Location(s)
 - `%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations`

## Artifact Parsers
 - JLECmd (Eric Zimmerman)

## Artifact Interpretation
The file name for these artifacts are based on the AppID of the program they are related to. For example, an AutomaticDestinations entry for `explorer.exe` will have the prefix `F01B4D95CF55D32A`. Thus, the full file name would be `f01b4d95cf55d32a.automaticDestinations-ms`. A good resource to leverage to translate AppIDs by Eric Zimmerman can be found [here](https://github.com/EricZimmerman/JumpList/blob/master/JumpList/Resources/AppIDs.txt).

### Execution - First Executed
The creation time of each AutomaticDestinations file corresponds to the first known time that an application **opened a file** while being executed. For instance, if a user simply opens Excel, but never opens a file, there will be no AutomaticDestinations file created. If, however, they proceed to open a file (or save a new file), an AutomaticDestinations file will be created with a creation timestamp corresponding to the time the first file was opened (or a new file was saved). 

### Execution - Last Executed
The modification time of each AutomaticDestinations file corresponds to the last known time that an application was executed **and opened a file**. For instance, the modification timestamp will change under the following example set of circumstances:

 - A user navigated to a directory using explorer and opened an Excel workbook by double-clicking on it
 - A user went to Excel on the taskbar and opened an Excel workbook listed under the `Recent` list. 
 - A user opened Excel, and then opened a workbook from within Excel.
 - A user opened Excel, and saved a new workbook from within Excel.

The timestamp will not be updated in the case that a user simply opened Excel without opening another file. 

### Execution - Permissions / Account
Given that the AutomaticDestinations files are stored in a user's %AppData% directory, this effectively ties execution of an application to a particular user account. For instance, if the AutomaticDestinations file for Excel exists in the following path: `C:\Users\john.doe\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\b8ab77100df80ab2.automaticDestinations-ms`, this implies that the user `john.doe` executed `Microsoft Office Excel x64`.

### Execution - Evidence of Execution
The presence of an AutomaticDestinations file for an application implies that the application was executed and was used to open a file or save a new file.

### File - Path
Within each AutomaticDestinations file is a list of recent files that were accessed, as well as a count of how many times each file was accessed using that specific application. Alongside this is the timestamp corresponding to the last known time each file was accessed by the application. 

For instance, when using JLECmd.exe (Eric Zimmerman) to analyze an AutomaticDestinations file for Excel, the following is seen:

```
Processing C:\Users\john.doe\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\b8ab77100df80ab2.automaticDestinations-ms

--- AppId information ---
  AppID: b8ab77100df80ab2
  Description: Microsoft Office Excel x64

--- DestList entries ---
Entry #: 2
  MRU: 0
  Path: C:\temp\test.xlsx
  Pinned: False
  Created on:    2023-06-24 15:53:43
  Last modified: 2023-06-24 17:56:09
  Hostname: HLPC01
  Interaction count: 2

--- Lnk information ---
  Absolute path: My Computer\C:\temp\test.xlsx
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>

We can conclude:
 - `Microsoft Office Excel x64` was executed
 - `C:\temp\test.xlsx` existed at some point on the local disk
 - The user `john.doe` executed Excel and accessed `C:\temp\test.xlsx`
 - `C:\temp\test.xlsx` was opened using `Microsoft Office Excel x64`
 - `C:\temp\test.xlsx` was accessed twice using Excel
 - The last known time Excel was used to access `C:\temp\test.xlsx` was at `2023-06-24 17:56:09`

We can gather more information given the Creation/Modification timestamps of the AutomaticDestinations file `b8ab77100df80ab2.automaticDestinations-ms` itself:

 - The creation timestamp of the file `b8ab77100df80ab2.automaticDestinations-ms` is `2022-02-12 15:22:00`, indicating that (given the AutomaticDestinations files were not deleted) Excel was first used to access files at `2022-02-12 15:22:00`
 - The modification timestamp of the file is `2023-06-24 17:56:09`, indicating that this was the last time Excel was used to access a file. As this is the `Last modified` timestamp of the entry for `C:\temp\test.xlsx`, we can conclude this is the last known file that Excel accessed on this system, corroborated by its `MRU` value of 0. 

As the AutomaticDestinations file is a collection of LNK files, JLECmd offers an additional option to parse these as well through the `--ld` flag. We can get a lot more information this way, for example, accessing a file (`Y:\Documents\new.xlsx`) on a mapped network share (at `192.168.0.20`):

```
Entry #: 3
  MRU: 0
  Path: Y:\Documents\new.xlsx
  Pinned: False
  Created on:    1582-10-15 00:00:00
  Last modified: 2023-06-24 18:06:38
  Hostname:
  Mac Address: fc:0c:00:00:00:00
  Interaction count: 4

--- Lnk information ---

--- Link information ---
Flags: CommonNetworkRelativeLinkAndPathSuffix

  Network share information
    Device name: Y:
    Share name: \\192.168.0.20\Documents
    Provider type: WnncNetLanman
    Share flags: 3

  Common path: Documents\new.xlsx
  Absolute path: My Computer\Y:\Documents\new.xlsx
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>

In this case, the file was accessed 4 times, and the most recent time it was accessed using Excel was at `2023-06-24 18:06:38`.