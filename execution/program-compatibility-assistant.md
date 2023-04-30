# Program Compatibility Assistant (PCA) - PcaAppLaunchDic.txt
Program Compatibility Assistant (PCA) is a feature introduced since Windows 8 intended for application compatibility purposes. New to Windows 11 (22H2 Update) is the ability to extract detailed evidence of execution from it. 

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)

### Analysis Value
 - [x] Execution - Evidence of Execution
 - [x] Execution - Last Executed
 - [x] File - Path

## Operating System Availability
 - [x] Windows 11 (⚠️ Recent builds only)

## Artifact Location(s)
- `%SystemRoot%\AppCompat\pca\PcaAppLaunchDic.txt`

## Artifact Interpretation - PcaAppLaunchDic.txt
This is a pipe-delimited text file that contains the following information:

`{Executable Full Path}|{Most Recent Execution Timestamp}`

The `{Executable Full Path}` provides the full path to the executable that was run. 

The `{Most Recent Execution Timestamp}` execution timestamp is in UTC and represents the last time the specified executable was run.

Any executable format, such as `.scr` and `.exe` may be logged in this location. This artifact will also log executables run from file shares and USB devices.

## Examples

```
\\NAS01-WS2K19\sysinternals\accesschk.exe|2023-05-03 01:55:27.884
C:\Users\user1.domain\Desktop\accesschk.exe|2023-05-03 01:56:00.799
```

<sup><sub>This example was produced on Windows 11 Pro, Version 10.0.22621 Build 22621</sub></sup>

## Caveats
Based on testing on Windows 11 Pro Build 22621, it seems that executions from the command line are not logged to this artifact.