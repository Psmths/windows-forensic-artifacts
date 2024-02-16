# Tracing Registry Keys
Tracing registry keys can be used to indicate that a program has initiated a network connection leveraging the Windows Remote Access Server (RAS) through the `rasapi32.dll` and `rasman.dll` libraries.

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)
 
### Analysis Value
 - [x] Execution - Evidence of Execution
 - [x] Network - Evidence of Network Activity

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7

## Artifact Location(s)
🔋 Live System:
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Tracing`

🔌 Offline system:
- File: `%SystemRoot%\System32\Config\SOFTWARE`
- Key: `SOFTWARE\Microsoft\Tracing`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
Within the `SOFTWARE\Microsoft\Tracing` key, there may be multiple subkeys with the following name formats of interest:

 - `{EXECUTABLE_FILENAME}_RASMANCS`
 - `{EXECUTABLE_FILENAME}_RASAPI32`

These filenames will not include the executable extension `.exe`.

The Last Write Timestamp of the registry key provides the first time an executable has loaded `rasapi32.dll` and `rasman.dll` in order to establish a remote network connection, typically to download a file. 

> [!IMPORTANT]
> Subsequent activity of this nature will not update the Last Write Timestamp of the registry key.