# Services Registry Key
The Services registry key, located in the SYSTEM hive, stores information regarding installed services on the endpoint. It is useful when searching for evidence of persistence mechanisms on an endpoint. 

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
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services`
- `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services`

🔌 Offline system:
- File: `C:\Windows\system32\config\SYSTEM`
- Key: `SYSTEM\ControlSet001\Services`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
Within the `SYSTEM\ControlSet001\Services` key you will find subkeys, one for each service installed on an endpoint. 

The values within this key may be interpreted as follows:

| Value | Interpretation |
| --- | --- |
| DisplayName | The name of the service as it would appear in `services.msc` |
| Description | The description of the service as it would appear in `services.msc` |
| ImagePath | The path to the executable for this service |
| Start |  |
| Type |  |

The Last Write Timestamp for each service key represents the time at which the service was installed or modified. 