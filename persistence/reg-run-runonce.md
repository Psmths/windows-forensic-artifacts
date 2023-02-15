# Run/RunOnce Registry Keys
There are several registry keys on a Windows system that allow for programs to execute during user logon events or a system boot. These keys are located in both the NTUSER.dat hive as well as the SOFTWARE and SYSTEM hives.

### Behavioral Indications
 - [x] Behavioral - Persistence (TA0003)

### Analysis Value
 - [x] File - Path

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista
 - [x] Windows XP

## Artifact File Location(s)

#### NTUSER.dat
- `C:\Users\{username}\NTUSER.dat` (Windows Vista - 10)
- `C:\Documents and Settings\{username}\NTUSER.dat` (Windows XP)

#### SOFTWARE
 - `C:\Windows\System32\Config\SOFTWARE`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)
 - AutoRuns (Sysinternals)

## Artifact Interpretation
### NTUSER.DAT / HKEY_CURRENT_USER (HKCU)
The following keys will contain full paths to the executables that will start on **logon**:

- `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Run`
- `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\RunOnce`

Disabled autoruns will appear in a sub-key named "AutorunsDisabled."

***NOTE:** On a live system, the HKEY_CURRENT_USER registry hive is the loaded NTUSER.dat hive.*

### SOFTWARE
The following keys will contain full paths to the executables that will start on **logon**:
- `SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
- `SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`