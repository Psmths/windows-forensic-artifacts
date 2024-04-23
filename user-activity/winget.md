# Windows Package Manager winget Activity
The Windows Package Manager utility, also known as winget, is a utility available on Windows 10/11 that allows for the quick installation of applications through the command line. It is functionally a Windows App, and is therefore installed under `C:\Program Files\WindowsApps`. The per-user application data is found under `%LOCALAPPDATA%\Packages\Microsoft.DesktopAppInstaller_8wekyb3d8bbwe`.

### Analysis Value
 - [x] Execution - Command Line Options
 - [x] User Activity

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10

## Artifact Location(s)
- `%LOCALAPPDATA%\Packages\Microsoft.DesktopAppInstaller_8wekyb3d8bbwe\LocalState\DiagOutputDir\*`
- `%LOCALAPPDATA%\Packages\Microsoft.DesktopAppInstaller_8wekyb3d8bbwe\LocalState\Microsoft.Winget.Source_8wekyb3d8bbwe\installed.db`

## Artifact Interpretation
There are two great locations for analyzing winget activity, the winget logs files and the winget user database.

### DiagOutputDir Log Files
The winget log files are found under `%LOCALAPPDATA%\Packages\Microsoft.DesktopAppInstaller_8wekyb3d8bbwe\LocalState\DiagOutputDir`. These log files provide a complete terminal input/output log of any winget command that was issued. This will include a full timestamp of the command, the winget version, as well as the command and any arguments passed to it.

### installed.db User Database
The winget user database is located at `%LOCALAPPDATA%\Packages\Microsoft.DesktopAppInstaller_8wekyb3d8bbwe\LocalState\Microsoft.Winget.Source_8wekyb3d8bbwe\installed.db`. This is an SQLite database that contains information on the applications that are installed. The Date Modified timestamp of this SQLite file represents that last time an app was installed, updated, or removed.

There are a handful of tables within this database. Two notable ones are the `metadata` and `names` tables, the first of which contains the last write timestamp of the database, and the second of which contains the names of all the applications installed through winget.

## Example
In this example, a user has installed Windows Terminal through winget. The command they used to perform the installation was `winget install Microsoft.WindowsTerminal`. This causes the following log to be generated at `C:\Users\user\AppData\Local\Packages\Microsoft.DesktopAppInstaller_8wekyb3d8bbwe\LocalState\DiagOutputDir\WinGet-2024-04-22-15-03-59.592.log` (truncated):

```
2024-04-22 15:01:19.653 [CORE] WinGet, version [1.7.10861], activity [{A25CE866-E937-45BB-842E-81D0BD72E555}]
2024-04-22 15:01:19.653 [CORE] OS: Windows.Desktop v10.0.19045.4291
2024-04-22 15:01:19.653 [CORE] Command line Args: winget  install Microsoft.WindowsTerminal
2024-04-22 15:01:19.653 [CORE] Package: Microsoft.DesktopAppInstaller v1.22.10861.0
2024-04-22 15:01:19.653 [CORE] IsCOMCall:0; Caller: winget-cli
2024-04-22 15:01:19.658 [CLI ] WinGet invoked with arguments: 'install' 'Microsoft.WindowsTerminal'
```

Accordingly, since this is the most recent application installed through winget, the `installed.db` modification timestamp is `2024-04-22-15-03`. In the names table, we see an entry for `Windows Terminal`. The `lastwritetime` key under the `metadata` table contains the value `1713823286`, corresponding to `2024-04-22 15:01:26`, which is when Windows Terminal finished installing. 