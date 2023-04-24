# Image File Execution Options Registry Keys
Image File Execution Options (IFEO) is a registry key that allows users to attach debuggers to programs. Attackers may leverage this registry key to establish persistence, as code execution can be triggered by execution (and exiting) of a particular program on an endpoint. 

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
File: `%SystemRoot%\System32\config\SOFTWARE`

- `SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\`
- `SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
Within the aforementioned registry locations exist keys named after certain executables on the endpoint. Placing a STRING value within these keys named `Debugger` allows an endpoint to specify an arbitrary executable that will be executed when the process is started.

## Examples
![Example Image](/media/examples/ifeo.png)