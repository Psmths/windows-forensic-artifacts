# Image File Execution Options Registry Keys
Image File Execution Options (IFEO) is a registry key that allows users to attach debuggers to programs. Attackers may leverage this registry key to establish persistence, as code execution can be triggered by execution and/or termination of a particular program on an endpoint. 

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
🔋 Live System:
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\`
- `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\`
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SilentProcessExit\`

🔌 Offline system:
- File: `%SystemRoot%\System32\Config\SOFTWARE`
- Key: `SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\`
- Key: `SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\`
- Key: `SOFTWARE\Microsoft\Windows NT\CurrentVersion\SilentProcessExit\`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
Normally, applications listed in the Image File Execution Options registry key will only have a `MitigationOptions` value present.

There are two common ways an attacker may leverage IFEO to establish a persistent foothold, the first being specifying a debugger for a particular application. This is accomplished by creating a new value of type `REG_SZ` named `Debugger`. When this key is present, the application specified in the `Debugger` key will be attached to the target application. When the target application is launched, the debugger will be launched as well. 

The second method is using the `GlobalFlag` of type `REG_DWORD`,  `ReportingMode` of type `REG_DWORD` and `MonitorProcess` of type `REG_SZ` values. The prerequisites for persistence using this method are:

 - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\{TARGET_APPLICATION}\GlobalFlag` must be set to `0x200`
 - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SilentProcessExit\{TARGET_APPLICATION}\ReportingMode` must be set to `0x1`
 - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SilentProcessExit\{TARGET_APPLICATION}\MonitorProcess` must specify an application to execute upon termination of the `{TARGET_APPLICATION}`

The application specified by the `MonitorProcess` key, assuming these prerequisites are met, will execute upon termination of the `{TARGET_APPLICATION}`, with a process parent of `WerFault.exe`.

The silent process exit method does not appear to work if the `GlobalFlag` key is located under `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\{TARGET_APPLICATION}`.

## Example - GlobalFlag Method
The following registry values are present on a system:

| Value Path | Value Data | Value Type |
| - | - | - |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\excel.exe\GlobalFlag` | `0x200` | `REG_DWORD` |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SilentProcessExit\excel.exe\ReportingMode` | `0x1` | `REG_DWORD` |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SilentProcessExit\excel.exe\MonitorProcess` | `C:\Windows\System32\cmd.exe` | `REG_SZ` |

When Microsoft Excel is exited, `cmd.exe` is spawned, as shown in the below Sysmon event:

```
Process Create:
RuleName: -
UtcTime: 2024-02-15 19:10:39.168
ProcessGuid: {3d0d6187-61af-65ce-5007-000000004c00}
ProcessId: 22256
Image: C:\Windows\System32\cmd.exe
CommandLine: C:\Windows\System32\cmd.exe
CurrentDirectory: C:\Windows\system32\
User: DESKTOP-DR5P34J\user
ParentProcessGuid: {3d0d6187-61af-65ce-4f07-000000004c00}
ParentProcessId: 4364
ParentImage: C:\Windows\System32\WerFault.exe
ParentCommandLine: "C:\Windows\system32\WerFault.exe" -s -t 10540 -i 20812 -e 20812 -c 0
ParentUser: DESKTOP-DR5P34J\user
```