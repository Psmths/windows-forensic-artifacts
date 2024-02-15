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

## Artifact Location(s)
> [!NOTE]
> The `Run` and `RunOnce` keys may be located in two distinct locations. Startup items for individual users will be located in their personal registry hive, NTUSER.dat, whereas startup items applying to all users of a system will be located in the SOFTWARE registry hive.

### NTUSER.dat Hive
🔋 Live System:
- `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Run`
- `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\RunOnce`

🔌 Offline system:
- File: `%UserProfile%\NTUSER.dat` (Windows Vista - 10)
- File: `C:\Documents and Settings\{USER_NAME}\NTUSER.dat` (Windows XP)
- Key: `SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
- Key: `SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`

### SOFTWARE Hive
🔋 Live System:
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Explorer\Run`

🔌 Offline system:
- File: `%SystemRoot%\System32\Config\SOFTWARE`
- Key: `SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
- Key: `SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`
- Key: `SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Explorer\Run`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)
 - AutoRuns (Sysinternals)

## Artifact Interpretation
### NTUSER.DAT Location
The following keys will contain full paths to the executables that will start on **logon** for the account that owns this particular `NTUSER.DAT` hive:

- `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Run`
- `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\RunOnce`

Disabled autoruns will appear in a sub-key named "AutorunsDisabled."

> [!NOTE]
> On a live system, the HKEY_CURRENT_USER registry hive is the loaded NTUSER.dat hive.

### SOFTWARE Location
The following keys will contain full paths to the executables that will start on **logon** for any user on the system:
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Explorer\Run`

## Analysis Tips
It is possible to obtain evidence of execution for processes that have executed as a result of these registry keys using the [Microsoft-Windows-Shell-Core/Operational/9707: Command Execution Started](/execution/evtx-9707-shell-core.md) event.

## Caveats
If the system is booted into Safe Mode, these keys will be ignored. A `RunOnce` key preceeded by an asterisk will ignore this restriction.

The `RunOnce` entry is typically deleted before the command is executed, regardless of its return value. If preceeded by and exclamation point, the `RunOnce` key will be deleted after the command has executed, and only if the command returned successfully. 