# Prefetch
Windows Prefetch is utilized to improve application performance by pre-loading resources when an application is launched. It can be used to determine:

 - Evidence of execution
 - Most recent execution
 - (Potential) first execution

## Operating System Availability
 - Windows 10
 - Windows 8
 - Windows 7
 - Windows Vista
 - Windows XP

## Artifact Location(s)
- `C:\Windows\Prefetch`

## Artifact Parsers
 - PECmd.exe (Eric Zimmerman)
 - pf.exe (TZWorks)

## Artifact Interpretation
The name of the prefetch file takes on the format: `{executable_name}-{hash}.pf`, where `executable_name` is the name of the executable file that was run, and `hash` provides a hash of the executable's path and the command line used to launch the executable. 

### Earliest Execution
The creation timestamp of the prefetch file indicates the *earliest* time the executable was run on the system. Because Windows only stores the last 128 entries (Windows XP/Vista/7) or 1024 entries (Windows 8/10), applications that haven't been run after some time may be rolled out of this directory, and re-created when they are run again. 

### Most Recent Execution
The most recent execution is indicated by the modification timestamp of the prefetch file. Additionally, on Windows 8/10, the last 8 execution times are stored within the prefetch file and can be parsed. 

## Caveats
Prefetch files are written approximately 10 seconds after execution. Subtract 10 seconds from the timestamp to get an approximate time.