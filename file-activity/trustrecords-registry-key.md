# Microsoft Office TrustRecords Registry Keys
For documents meant to be edited in Microsoft Office Word or Excel, if the document has been marked as originating from a non-secure origin such as the Internet (by means of and NTFS [Zone.Identifier](file-activity/zone-identifier.md)), the user is prompted to enable editing and potentially enable macros for the document. This can be a valuable indicator in situations where malware was deployed through malicious Office macros.

### Analysis Value
 - [x] File - Path

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista

## Artifact Location(s)
🔋 Live System:

 - `HKEY_CURRENT_USER\Software\Microsoft\Office\{OFFICE_VERSION}\Word\Security\Trusted Documents\TrustRecords` for Microsoft Office Word
 - `HKEY_CURRENT_USER\Software\Microsoft\Office\{OFFICE_VERSION}\Excel\Security\Trusted Documents\TrustRecords` for Microsoft Office Excel
  - `HKEY_CURRENT_USER\Software\Microsoft\Office\{OFFICE_VERSION}\PowerPoint\Security\Trusted Documents\TrustRecords` for Microsoft Office PowerPoint

🔌 Offline system:
 - File: `%UserProfile%\NTUSER.DAT`
 - `Software\Microsoft\Office\{OFFICE_VERSION}\Word\Security\Trusted Documents\TrustRecords` for Microsoft Office Word
 - `Software\Microsoft\Office\{OFFICE_VERSION}\Excel\Security\Trusted Documents\TrustRecords` for Microsoft Office Excel
  - `Software\Microsoft\Office\{OFFICE_VERSION}\PowerPoint\Security\Trusted Documents\TrustRecords` for Microsoft Office PowerPoint

> [!NOTE]
> On a live system, the HKEY_CURRENT_USER registry hive is the loaded NTUSER.dat hive for the currently active user.

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
The `Trusted Documents\TrustRecords` registry keys are created when a user choses to enable editing for the first time for each particular Microsoft Office application. Within the `TrustRecords` registry key there will be `REG_BINARY` values corresponding to each document for which the user has opted to enable editing/macros. The name of this value is the path of the file in question, and the data contains some information regarding what action(s) were taken.

The last 4 bytes of the data for each value may indicate:

| Last 4 Bytes | Interpretation |
| ------------ | -------------- |
| `01 00 00 00` | The user has enabled editing for this file |
| `FF FF FF 7F` | The user has enabled macros for this file |

Essentially, this artifact provides evidence that a certain user opened a particular Microsoft Office file, and may also indicate that they enabled editing/macro content. The registry entries for these documents persists even in the event that the file was deleted from the filesystem.

## Example
In this example, a file containing macro content was downloaded from the internet. When the user opened the file, they were first prompted to enable editing. When they pressed this, a new registry key was created:

```
Path: HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\16.0\Word\Security\Trusted Documents\TrustRecords\
Key Name: %USERPROFILE%/Downloads/Word-Macro.docm
Key Value: 5A AC 70 BD C9 95 DA 01 00 28 A1 53 C5 FF FF FF A9 DE B3 01 01 00 00 00
```

Afterwards, the prompt then asked the user to enable contents (enabling macros), and when the user clicked to allow, the registry key was updates as follows (note the change to the last 4 bytes of the value):

```
Path: HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\16.0\Word\Security\Trusted Documents\TrustRecords\
Key Name: %USERPROFILE%/Downloads/Word-Macro.docm
Key Value: 5A AC 70 BD C9 95 DA 01 00 28 A1 53 C5 FF FF FF AB DE B3 01 FF FF FF 7F
```