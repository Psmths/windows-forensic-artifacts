# AutomaticDestinations Jumplists
AutomaticDestinations Jump Lists are per-user artifacts introduced in Windows 7 that record application-associated interactions with files, folders, and other shell objects. They are part of the Windows Jump List feature, which allows users to access recently or frequently used items by right-clicking an application in the taskbar or Start menu.

AutomaticDestinations files are essentially containers holding a list of entries and associated shortcut (LNK)-like data. They use the Object Linking and Embedding (OLE) Compound File Binary Format (CFB), also known as OLE Structured Storage.

From a forensic perspective, these artifacts are valuable for identifying files and folders that were accessed through particular applications. They can contain target paths, timestamps, interaction counts, volume information, and network share information. Because they are maintained within individual user profiles, they can also help associate application activity with a particular Windows user profile.

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
- `%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\`

## Artifact Parsers
- [JLECmd](https://github.com/EricZimmerman/JLECmd) (Eric Zimmerman)
- [Jump List Explorer](https://ericzimmerman.github.io/)

## Artifact Interpretation
AutomaticDestinations files are named using the Application ID (AppID) associated with the application to which the Jump List belongs. The AppID is typically represented as a 16-character hexadecimal string.

For example, an AutomaticDestinations file associated with Windows Explorer may be named:

`f01b4d95cf55d32a.automaticDestinations-ms`

The AppID can be used to identify the application associated with the Jump List. However, not all AppIDs are documented, and applications may use explicitly assigned application identifiers or identifiers derived from application-specific information.

A useful reference for identifying known AppIDs is Eric Zimmerman's [AppIDs.txt](https://github.com/EricZimmerman/JumpList/blob/master/JumpList/Resources/AppIDs.txt).

### AutomaticDestinations vs. CustomDestinations
Windows maintains two types of Jump Lists:

- **AutomaticDestinations:** Automatically populated with entries associated with recently or frequently accessed items. These files use the `.automaticDestinations-ms` extension and contain a `DestList` stream along with embedded LNK-like data.
- **CustomDestinations:** Application-defined Jump Lists that can contain custom tasks and pinned entries. These files use the `.customDestinations-ms` extension and have a different internal structure.

### Execution - First Executed
The creation timestamp of an AutomaticDestinations file may provide an indication of when an application first began populating its Jump List. However, it should not universally be interpreted as the first time the application was executed or the first time a file was opened.

Jump List creation behavior depends on the application and how it interacts with the Windows Shell. Some applications may not create or populate a Jump List until a file is opened or saved, while others may create or update entries through different interactions.

Consequently, the creation timestamp of the AutomaticDestinations file is best treated as a potential indicator of the first recorded application interaction, rather than definitive evidence of first execution.

The `Created on` timestamp reported for an individual `DestList` entry is a separate value from the filesystem creation timestamp of the AutomaticDestinations file. The entry creation timestamp is not considered universally reliable and should be interpreted cautiously.

### Execution - Last Executed
The modification timestamp of an AutomaticDestinations file can provide an indication of when the Jump List was last updated.

For applications that regularly update their Jump Lists when files are accessed, this timestamp may correspond to the most recent interaction with an item through that application.

For example, an AutomaticDestinations file may be updated when:

- A user navigates to a directory using Windows Explorer and opens a document with the associated application.
- A user selects a document from the application's Recent list.
- A user opens a document from within the application.
- A user saves a new document from within the application.

However, application behavior varies. The modification timestamp should therefore be interpreted as the last time the Jump List file was updated, not automatically as the last time the application was executed.

The `Last modified` timestamp reported for an individual `DestList` entry is also distinct from the filesystem modification timestamp of the AutomaticDestinations file. The entry timestamp is generally updated when that particular item is interacted with.

### Execution - Evidence of Application/File Interaction
The presence of an AutomaticDestinations entry provides evidence that an item was associated with an application through the Windows Jump List mechanism.

For example, an entry in an Excel AutomaticDestinations file referencing `C:\temp\test.xlsx` indicates that the workbook was associated with Excel through the Jump List.

This is useful evidence of application-related file interaction, but it should not be treated as standalone proof that the application executable was launched at a particular time. Correlate with execution artifacts such as Prefetch, UserAssist, BAM/DAM, Amcache, and SRUM where available.

### Account - Username
AutomaticDestinations files are stored within the user's `%AppData%` directory. This provides a relationship between the Jump List and the Windows user profile in which it was found.

For example, if an Excel AutomaticDestinations file is located at:

`C:\Users\john.doe\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\b8ab77100df80ab2.automaticDestinations-ms`

The artifact is associated with the `john.doe` user profile.

This does not independently establish the identity of the individual operating the system, nor does it prove that the account owner personally performed the interaction. Consider the possibility of shared accounts, impersonation, remote sessions, and copied or restored profile data.

### File - Path
Each AutomaticDestinations file contains a `DestList` stream that maintains metadata about items associated with the application. Entries can include:

- Target file or folder path.
- Most Recently Used (MRU) position.
- Whether the entry is pinned.
- Entry creation and last-modified timestamps.
- Hostname and, when available, MAC address information.
- Interaction count.
- Embedded LNK-like information, including target metadata and volume or network share details.

The `Interaction count` indicates how many times the item was interacted with through the Jump List mechanism. It should not necessarily be interpreted as an exact count of successful file opens in every application.

The `MRU` value indicates the item's position in the Most Recently Used list. An `MRU` value of `0` generally identifies the most recently used entry; it is not a timestamp.

The timestamps contained within the embedded LNK data provide additional information about the target file, including its creation, modification, and access timestamps when available. These target timestamps are distinct from the `DestList` entry timestamps.

## Example
When using [JLECmd](https://github.com/EricZimmerman/JLECmd) to analyze an AutomaticDestinations file associated with Excel, the following output may be observed:

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

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044.</sub></sup>

From this output, we can establish:

- The Jump List is associated with `Microsoft Office Excel x64`, based on the AppID mapping.
- The entry references `C:\temp\test.xlsx`.
- The entry is not pinned.
- The entry has an interaction count of 2.
- The entry's most recent MRU position is 0.
- The `DestList` entry's last-modified timestamp is `2023-06-24 17:56:09`.
- The entry contains a hostname of `HLPC01`.

The interaction count indicates that the item was interacted with twice through the Jump List mechanism. The last-modified timestamp provides a potential last-interaction time for that entry.

The output does not, by itself, establish the first time Excel was executed, the exact number of times the workbook was successfully opened, or the filesystem existence of the workbook at the time of forensic acquisition.

In addition to the timestamps stored within individual `DestList` entries, the AutomaticDestinations file itself has filesystem creation and modification timestamps.

These timestamps can provide useful temporal context:

- **Filesystem creation timestamp:** May indicate when the Jump List file was first created. Depending on application behavior, this may correspond to the first time the application populated its Jump List.
- **Filesystem modification timestamp:** Indicates when the Jump List file was last modified. Depending on application behavior, this may correspond to a recent interaction with an item associated with the application.

For example, suppose the filesystem timestamps for the Excel AutomaticDestinations file are:

- Created: `2022-02-12 15:22:00`
- Modified: `2023-06-24 17:56:09`

The creation timestamp may indicate when the Jump List was first created or populated, while the modification timestamp indicates when the file was last updated.

If the most recently modified `DestList` entry also has a last-modified timestamp of `2023-06-24 17:56:09` and an `MRU` value of `0`, this provides additional support that the corresponding file was the most recently interacted-with item recorded in the Jump List.

However, the file's creation timestamp should not automatically be treated as the first time Excel accessed a file. Likewise, the file's modification timestamp should not automatically be treated as the last time Excel was executed. These interpretations depend on application behavior and should be corroborated with other artifacts.

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

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044.</sub></sup>

From this output, we can establish:

- The Jump List entry references `Y:\Documents\new.xlsx`.
- The embedded LNK information identifies the network share as `\\192.168.0.20\Documents`.
- The file was accessed through a mapped network drive (`Y:`).
- The entry has an interaction count of 4.
- The entry's last-modified timestamp is `2023-06-24 18:06:38`.
- The entry is the most recently used item in the list, as indicated by `MRU: 0`.
