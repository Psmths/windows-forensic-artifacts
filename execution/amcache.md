# Amcache.hve
The Amcache hive stores metadata regarding executables/installed programs present on an endpoint. Typically, only those that have been executed (or executables associated with installed software) will appear in this registry hive. This artifact has seen numerous revisions, and it is therefore important to first gather information regarding the specific version of Windows that you are analyzing before proceeding with Amcache analysis. You can find a system's OS version and build number information in the [CurrentVersion](/enumeration/current-version.md) registry key, using the `CurrentMajorVersionNumber`, `CurrentMinorVersionNumber`, and `CurrentBuildNumber` values.

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)

### Analysis Value
 - [x] Execution - First Executed
 - [x] Execution - Evidence of Execution
 - [x] File - Hash
 - [x] File - Path

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7 ⚠️
 - [x] Windows Server 2019
 - [x] Windows Server 2016
 - [x] Windows Server 2012 R2
 - [x] Windows Server 2012
 - [x] Windows Server 2008 R2

> [!IMPORTANT]  
> Windows 7 requires update KB2952664 for the Amcache hive to be present. Amcache is available on Windows Server starting from Windows Server 2008 R2.
 
## Artifact Location(s)
- `%SystemRoot%\AppCompat\Programs\Amcache.hve`

Additional LOG files:
- `%SystemRoot%\AppCompat\Programs\Amcache.hve.*LOG1`
- `%SystemRoot%\AppCompat\Programs\Amcache.hve.*LOG2`

## Artifact Parsers
 - amcacheparser.exe (Eric Zimmerman)
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
Within the Amcache hive there are multiple registry keys, each containing different information. The most common keys to analyze are:

 - InventoryApplication
 - InventoryApplicationFile
 - InventoryDriverBinary
 - InventoryApplicationShortcut

### InventoryApplication (Windows 10 Build 10.0.14393 +)
The `InventoryApplication` key stores information about installed software on the system. This key contains a value named `LastScanTime` that corresponds to the last time the Microsoft Compatibility Appraiser has run. This is a scheduled task that executes the `compattelrunner.exe` binary. **The information contained in this key should only be updated when this task is executed. Software installed since this task has last run may not appear in this key!** This value is in Windows FileTime format.

This key contains subkeys for each installed software, the key names being the software's `ProgramId`. It contains the following values of interest:

| Value | Description |
| ----- | ----------- |
| ProgramId | The installed software's ProgramId |
| Name | Name of the installed software |
| Version | Version of the installed software |
| Publisher | The installed software's publisher |
| Source | `AddRemoveProgram` or `Msi` or `File` or `AppXPackage` |
| InstallDate | The date the software was installed. This seems to only populate for AddRemoveProgram/Msi software installations |
| RootDirPath | The path to the root directory of the software |
| RegistryKeyPath | The path to the `Uninstall` registry key in the SOFTWARE hive |

The `Source` value can give information regarding how software was installed on the system:

 - AddRemoveProgram: Software installed via an executable
 - Msi: Software installed via a .msi file using the Windows Installer service
 - AppXPackage: Software installed via the Windows Store of the `Get-AppxPackage` PowerShell command

### InventoryApplicationFile (Windows 10 Build 10.0.14393 +)
This registry key contains information about the executables tied to installed software, as well as executables that have run on the system. A single software installation may drop multiple executables to a system, and they should all be tracked here. Like the `InventoryApplication` key, this key also only updates when the Microsoft Compatibility Appraiser task has run.

The subkeys will contain the executable name, and a hash separated by a `|` character. The most interesting values to analyze within this key are:

| Value | Description |
| ----- | ----------- |
| ProgramId | The ProgramId that this executable is tied to, which can be found in `InventoryApplication`. If the executable was not installed as part of a software installation, this ProgramId will not be found in `InventoryApplication` |
| FileId | Stripping the four leading 0s, the SHA-1 hash of the executable |
| LowerCaseLongPath | The path to the executable |
| Name | The filename of the executable |
| BinaryType | 32/64bit indicator |
| Size | The size, in bytes, of the executable |

> [!WARNING]
> **There is a limit to the size of the data that gets hashed to produce this artifact's SHA-1 hash in the `FileId` value.** If the size of the binary exceeds approximately 30MB in size, only the first 30MB will be hashed. The result is that the SHA-1 hash will not be valid for that binary. ⚠️

## Example
Installing a new software, CrystalDiskMark on a system and manually running `compattelrunner.exe` updated the Amcache Hive with the following key (named `00001d78ebb0f68947e39952c24983d564390000ffff`) under `InventoryApplication`:

```
[
    {
        "Data": "00001d78ebb0f68947e39952c24983d564390000ffff",
        "ValueName": "ProgramId",
        "ValueType": "RegSz",
    },
    {
        "Data": "00006141a84b1e5f3b60561c7be664657764da598522",
        "ValueName": "ProgramInstanceId",
        "ValueType": "RegSz",
    },
    {
        "Data": "CrystalDiskMark 8.0.4c",
        "ValueName": "Name",
        "ValueType": "RegSz",
    },
    {
        "Data": "8.0.4c",
        "ValueName": "Version",
        "ValueType": "RegSz",
    },
    {
        "Data": "Crystal Dew World",
        "ValueName": "Publisher",
        "ValueType": "RegSz",
    },
    {
        "Data": "65535",
        "ValueName": "Language",
        "ValueType": "RegDword",
    },
    {
        "Data": "AddRemoveProgram",
        "ValueName": "Source",
        "ValueType": "RegSz",
    },
    {
        "Data": "Application",
        "ValueName": "Type",
        "ValueType": "RegSz",
    },
    {
        "Data": "",
        "ValueName": "StoreAppType",
        "ValueType": "RegSz",
    },
    {
        "Data": "",
        "ValueName": "MsiPackageCode",
        "ValueType": "RegSz",
    },
    {
        "Data": "",
        "ValueName": "MsiProductCode",
        "ValueType": "RegSz",
    },
    {
        "Data": "0",
        "ValueName": "HiddenArp",
        "ValueType": "RegDword",
    },
    {
        "Data": "0",
        "ValueName": "InboxModernApp",
        "ValueType": "RegDword",
    },
    {
        "Data": "10.0.0.19044",
        "ValueName": "OSVersionAtInstallTime",
        "ValueType": "RegSz",
    },
    {
        "Data": "10/18/2023 00:00:00",
        "ValueName": "InstallDate",
        "ValueType": "RegSz",
    },
    {
        "Data": "",
        "ValueName": "PackageFullName",
        "ValueType": "RegSz",
    },
    {
        "Data": "",
        "ValueName": "ManifestPath",
        "ValueType": "RegSz",
    },
    {
        "Data": "",
        "ValueName": "BundleManifestPath",
        "ValueType": "RegSz",
    },
    {
        "Data": "C:\\Program Files\\CrystalDiskMark8\\",
        "ValueName": "RootDirPath",
        "ValueType": "RegSz",
    },
    {
        "Data": "\"C:\\Program Files\\CrystalDiskMark8\\unins000.exe\"",
        "ValueName": "UninstallString",
        "ValueType": "RegSz",
    },
    {
        "Data": "HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Uninstall\\CrystalDiskMark8_is1",
        "ValueName": "RegistryKeyPath",
        "ValueType": "RegSz",
    },
    {
        "Data": "0",
        "ValueName": "SentDetailedInv",
        "ValueType": "RegDword",
    }
]
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>

The software was installed via an executable, leading the `Source` value to be `AddRemoveProgram`. Note the `ProgramId` value of `00001d78ebb0f68947e39952c24983d564390000ffff`.

Additionally, several keys were created under `InventoryApplicationFile`, one example (`diskmark32.exe|51ddc7c2637fbb8d`):

```
[
    {
        "Data": "00001d78ebb0f68947e39952c24983d564390000ffff",
        "ValueName": "ProgramId",
        "ValueType": "RegSz",
    },
    {
        "Data": "00009d1e062ff187c9a920a3fcc511911d4fc0e820ce",
        "ValueName": "FileId",
        "ValueType": "RegSz",
    },
    {
        "Data": "c:\program files\crystaldiskmark8\diskmark32.exe",
        "ValueName": "LowerCaseLongPath",
        "ValueType": "RegSz",
    },
    {
        "Data": "diskmark32.exe|51ddc7c2637fbb8d",
        "ValueName": "LongPathHash",
        "ValueType": "RegSz",
    },
    {
        "Data": "DiskMark32.exe",
        "ValueName": "Name",
        "ValueType": "RegSz",
    },
    {
        "Data": "crystal dew world",
        "ValueName": "Publisher",
        "ValueType": "RegSz",
    },
    {
        "Data": "pe32_i386",
        "ValueName": "BinaryType",
        "ValueType": "RegSz",
    },
    {
        "Data": "crystaldiskmark8",
        "ValueName": "ProductName",
        "ValueType": "RegSz",
    },
    {
        "Data": "8.0.4.0",
        "ValueName": "ProductVersion",
        "ValueType": "RegSz",
    },
    {
        "Data": "07/11/2021 06:58:40",
        "ValueName": "LinkDate",
        "ValueType": "RegSz",
    },
    {
        "Data": "8.0.4.0",
        "ValueName": "BinProductVersion",
        "ValueType": "RegSz",
    },
    {
        "Data": "698912",
        "ValueName": "Size",
        "ValueType": "RegQword",
    },
    {
        "Data": "1041",
        "ValueName": "Language",
        "ValueType": "RegDword",
    },
    {
        "Data": "60189208",
        "ValueName": "Usn",
        "ValueType": "RegQword",
    }
]
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>

From this example, we can see that the `ProgramId` between the two Amcache keys correspond to each other. 
