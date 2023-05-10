# Prefetch
Windows Prefetch is utilized to improve application performance by pre-loading resources when an application is launched.

In addition to providing evidence of execution, the prefetch artifact provides a list of modules/files that have been accessed by the process in the 10 seconds following spawning. 

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)

### Analysis Value
 - [x] Execution - First Executed
 - [x] Execution - Last Executed
 - [x] Execution - Evidence of Execution
 - [x] Execution - Full Path
 - [x] File - Path

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista
 - [x] Windows XP
 - [x] Windows Server 2019 (⚠️ Requires manual enable)
 - [x] Windows Server 2016 (⚠️ Requires manual enable)
 - [x] Windows Server 2012 R2 (⚠️ Requires manual enable)
 - [x] Windows Server 2012 (⚠️ Requires manual enable)
 - [x] Windows Server 2008 R2 (⚠️ Requires manual enable)
 - [x] Windows Server 2008 (⚠️ Requires manual enable)
 - [x] Windows Server 2003 R2 (⚠️ Requires manual enable)
 - [x] Windows Server 2003 (⚠️ Requires manual enable)

## Artifact Location(s)
- `%SystemRoot%\Prefetch`

## Artifact Parsers
 - PECmd.exe (Eric Zimmerman)
 - pf.exe (TZWorks)

## Artifact Interpretation
The name of the prefetch file takes on the format: `{executable_name}-{hash}.pf`, where `executable_name` is the name of the executable file that was run, and `hash` provides a hash of the executable's path and the command line used to launch the executable. If the same executable was run with different command line options, or the executable was moved and then run again, this essentially means there will be more than one prefetch entry for it. 

### Earliest Execution
The creation timestamp of the prefetch file indicates the **potential** *earliest* time the executable was run on the system. This is because the amount of prefetch files stored on the system is limited, and older files are rotated out. 

### Last Executed
The most recent execution is indicated by the modification timestamp of the prefetch file. Additionally, on Windows 8/10, the last 8 execution times are stored within the prefetch file and can be parsed. 

## Full Path
The artifact, when parsed, provides the full path to the executable that was run.

## Caveats
Prefetch files are written approximately 10 seconds after execution. Subtract 10 seconds from the prefetch filesystem timestamps to get an approximate time.

Regarding the "potential" earliest/first execution: Because Windows only stores the last 128 entries (Windows XP/Vista/7) or 1024 entries (Windows 8/10), applications that haven't been run after some time may be rolled out of this directory, and re-created when they are run again.

You may see multiple entries for the same executable on a system. This indicates that the executable was with different command line options, or (less likely) existed in different directories when executed. 

## Analysis Tips

### Evidence of Deleted Files
Because the Prefetch artifact stores files referenced by a program, it may be used to identify deleted files as they will persist in this artifact.

## Example
In the following example, the SysInternals utility `ADExplorer64` was executed from two separate locations, once from the system's disk under `C:\Temp` and once from a connected USB device, resulting in two prefetch files:

 - `ADEXPLORER64.EXE-9B0EE190.pf` : Executed three times from a connected USB device
 - `ADEXPLORER64.EXE-67B06AB8.pf` : Executed once on local disk

The files when parsed through PECmd, resulted in the following outputs:

```
Processing ADEXPLORER64.EXE-67B06AB8.pf

Created on: 2023-05-10 16:29:15
Modified on: 2023-05-10 16:28:35
Last accessed on: 2023-05-10 16:31:26

Executable name: ADEXPLORER64.EXE
Hash: 67B06AB8
File size (bytes): 32,418
Version: Windows 10 or Windows 11

Run count: 1
Last run: 2023-05-10 16:28:32

Volume information:

#0: Name: \VOLUME{01d8e97614061ec7-7c141bd8} Serial: 7C141BD8 Created: 2022-10-26 20:03:55 Directories: 9 File references: 58

Directories referenced: 9

00: \VOLUME{01d8e97614061ec7-7c141bd8}\TEMP (Keyword True)
01: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS
02: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\APPPATCH
03: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\FONTS
04: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\GLOBALIZATION
05: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\GLOBALIZATION\SORTING
06: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32
07: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32\EN-US
08: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\WINSXS\AMD64_MICROSOFT.WINDOWS.COMMON-CONTROLS_6595B64144CCF1DF_6.0.19041.1110_NONE_60B5254171F9507E

Files referenced: 49

00: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32\NTDLL.DLL
01: \VOLUME{01d8e97614061ec7-7c141bd8}\TEMP\ADEXPLORER64.EXE (Executable: True)
02: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32\KERNEL32.DLL
03: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32\KERNELBASE.DLL
04: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32\LOCALE.NLS
05: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32\APPHELP.DLL
06: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\APPPATCH\SYSMAIN.SDB
07: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32\RPCRT4.DLL
08: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32\USER32.DLL
...
```

In the case where ADExplorer64 was run from a USB device, note the presence of two volume entries. Additionally, since the executable was run three times, there are additional execution timestamps for these events as well!
```
Processing ADEXPLORER64.EXE-9B0EE190.pf

Created on: 2023-05-10 16:44:17
Modified on: 2023-05-10 16:43:59
Last accessed on: 2023-05-10 16:44:32

Executable name: ADEXPLORER64.EXE
Hash: 9B0EE190
File size (bytes): 32,266
Version: Windows 10 or Windows 11

Run count: 3
Last run: 2023-05-10 16:43:57
Other run times: 2023-05-10 16:43:54, 2023-05-10 16:43:51

Volume information:

#0: Name: \VOLUME{0000000000000000-340060b2} Serial: 340060B2 Created: 1601-01-01 00:00:00 Directories: 0 File references: 1
#1: Name: \VOLUME{01d8e97614061ec7-7c141bd8} Serial: 7C141BD8 Created: 2022-10-26 20:03:55 Directories: 7 File references: 52

Directories referenced: 7

00: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS
01: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\FONTS
02: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\GLOBALIZATION
03: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\GLOBALIZATION\SORTING
04: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32
05: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32\EN-US
06: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\WINSXS\AMD64_MICROSOFT.WINDOWS.COMMON-CONTROLS_6595B64144CCF1DF_6.0.19041.1110_NONE_60B5254171F9507E

Files referenced: 48

00: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32\NTDLL.DLL
01: \VOLUME{0000000000000000-340060b2}\ADEXPLORER64.EXE (Executable: True)
02: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32\KERNEL32.DLL
03: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32\KERNELBASE.DLL
04: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32\LOCALE.NLS
05: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32\RPCRT4.DLL
06: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32\USER32.DLL
07: \VOLUME{01d8e97614061ec7-7c141bd8}\WINDOWS\SYSTEM32\NETAPI32.DLL
...
```