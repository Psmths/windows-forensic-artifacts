# NetworkCards Registry Key
The `NetworkCards` registry key will provide the name and interface GUID of the system's attached network interface adatpers.

### Analysis Value
 - [x] Endpoint - Enumeration

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
 - [x] Windows Server 2008
 - [x] Windows Server 2003 R2
 - [x] Windows Server 2003

## Artifact Location(s)
🔋 Live System:
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkCards\*`

🔌 Offline system:
- File: `%SystemRoot%\System32\config\SOFTWARE`
- Key: `SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkCards\*`


## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
Each network interface adapter will have its own subkey with the following values:

 - `Description`: The name of the network adapter
 - `ServiceName`: The GUID of the network adapter

## Examples

```
Description     REG_SZ      Microsoft Hyper-V Network Adapter
ServiceName     REG_SZ      {4C7FB48D-33EB-4277-A3FB-37D5EF39C990}
```

<sup><sub>This example was produced on Windows Server 2019 Standard, Version 10.0.17763 Build 17763</sub></sup>


```
Description     REG_SZ      Realtek USB GbE Family Controller
ServiceName     REG_SZ      {737FB1FE-88FF-4F5E-943A-A46C057E68B9}
```

<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>

