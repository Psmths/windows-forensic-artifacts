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
 - [x] Windows Server 2019
 - [x] Windows Server 2016
 - [x] Windows Server 2012 R2
 - [x] Windows Server 2012
 - [x] Windows Server 2008 R2
 - [x] Windows Server 2003 R2
 - [x] Windows Server 2003

## Artifact Location(s)
🔋 Live System:
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services`

🔌 Offline system:
- File: `%SystemRoot%\system32\config\SYSTEM`
- Key: `SYSTEM\{CURRENT_CONTROL_SET}\Services`

> [!NOTE]
> More information on [{CURRENT_CONTROL_SET}](/enumeration/select.md)

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
Within the `Services` key you will find subkeys, one for each service installed on an endpoint. 

The values within this key may be interpreted as follows:

| Value | Interpretation |
| --- | --- |
| DisplayName | The name of the service as it would appear in `services.msc` |
| Description | The description of the service as it would appear in `services.msc` |
| ImagePath | The path to the executable for this service |
| Start | Start mode of the service |
| Type | Type of service |

The Last Write Timestamp for each service key represents the time at which the service was installed or modified.

Additionally, for each service there may be an optional `Parameters` subkey. This key may contain any options that are passed to the executable when the service is started. Certain service installers such as NSSM (Non-Sucking Service Manager) will show the "true" executable for the service under this `Parameters` key.  

### Interpreting the ```Start``` Value
| Value | Interpretation |
| - | - |
| 0 | **Boot** - Service is a device driver |
| 1 | **System** - Service is a device driver |
| 2 | **Automatic** - Service and all of its dependency services is started on boot by the OS |
| 3 | **Manual** - Service is started manually by user interaction |
| 4 | **Disabled** - Service is disabled and cannot be started automatically or manually |

