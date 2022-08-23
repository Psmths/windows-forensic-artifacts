# Registry Autostart Keys
There are several registry keys on a Windows system that allow for programs to execute during user logon events or a system boot. Evidence of persistence may be gathered from these forensic artifacts. These keys are located in both the NTUSER.dat hive as well as the SOFTWARE and SYSTEM hives.

## Operating System Availability
 - Windows 10
 - Windows 8
 - Windows 7
 - Windows Vista
 - Windows XP

## Artifact Location(s)

#### NTUSER.dat
- `C:\Documents and Settings\{username}\NTUSER.dat` (Windows XP)
- `C:\Users\{username}\NTUSER.dat` (Windows Vista - 10)

***NOTE:** On a live system, the HKEY_CURRENT_USER registry hive is the loaded NTUSER.dat hive.*

#### SOFTWARE
 - `C:\Windows\System32\Config\SOFTWARE`

#### SYSTEM
 - `C:\Windows\System32\Config\SYSTEM`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)
 - AutoRuns (Sysinternals)

## Artifact Interpretation
### NTUSER.DAT / HKEY_CURRENT_USER (HKCU)
The following keys will contain full paths to the executables that will start on **logon**:

- `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Run`
- `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\RunOnce`

Disabled autoruns will appear in a sub-key named "AutorunsDisabled."

### SOFTWARE
The following keys will contain full paths to the executables that will start on **logon**:
- `SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
- `SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`

### SYSTEM
The following keys will contain full paths to the executables:

- `SYSTEM\CurrentControlSet\Services`

For each key, there will be a value named `Start.` Services with the value set to 0x02 will start at **boot**.