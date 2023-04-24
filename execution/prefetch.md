# Prefetch
Windows Prefetch is utilized to improve application performance by pre-loading resources when an application is launched.

In addition to providing evidence of execution, the prefetch artifact provides a list of modules/files that have been accessed by the process in the 10 seconds following spawning. 

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)

### Analysis Value
 - [x] Execution - First Executed
 - [x] Execution - Last Executed
 - [x] Execution - Evidence of Execution


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
The name of the prefetch file takes on the format: `{executable_name}-{hash}.pf`, where `executable_name` is the name of the executable file that was run, and `hash` provides a hash of the executable's path and the command line used to launch the executable. 

### Earliest Execution
The creation timestamp of the prefetch file indicates the **potential** *earliest* time the executable was run on the system.

### Most Recent Execution
The most recent execution is indicated by the modification timestamp of the prefetch file. Additionally, on Windows 8/10, the last 8 execution times are stored within the prefetch file and can be parsed. 



## Caveats
Prefetch files are written approximately 10 seconds after execution. Subtract 10 seconds from the prefetch filesystem timestamps to get an approximate time.

Regarding the "potential" earliest/first execution: Because Windows only stores the last 128 entries (Windows XP/Vista/7) or 1024 entries (Windows 8/10), applications that haven't been run after some time may be rolled out of this directory, and re-created when they are run again.

You may see multiple entries for the same executable on a system. This indicates that the executable was run from separate locations. 

## Analysis Tips

### Evidence of Deleted Files
Because the Prefetch artifact stores files referenced by a program, it may be used to identify deleted files as they will persist in this artifact.