# Run/RunOnce Registry Keys
The `Run` and `RunOnce` keys specify what programs will start during a logon event. These keys are located in both the NTUSER.dat and SOFTWARE hives.

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
 - `%SystemRoot%\System32\Config\SOFTWARE`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)
 - AutoRuns (Sysinternals)

## Artifact Interpretation
### NTUSER.DAT
The following keys will contain full paths to the executables that will start on **logon**:

- `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Run`
- `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\RunOnce`

Disabled autoruns will appear in a sub-key named "AutorunsDisabled."

***NOTE:** On a live system, the HKEY_CURRENT_USER registry hive is the loaded NTUSER.dat hive.*

### SOFTWARE
The following keys will contain full paths to the executables that will start on **logon**:
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Explorer\Run`