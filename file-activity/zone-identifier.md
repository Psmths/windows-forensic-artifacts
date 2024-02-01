# NTFS Zone Identifiers
Zone Identifiers are an artifact that reside in alternate data streams within an NTFS volume. This artifact is used to determine the origins of a file on a disk.

> [!NOTE]
> In Windows XP, only executable files will be tagged with a Zone Identifier. Later operating systems will tag all files.

### Analysis Value
 - [x] File - Creation
 - [x] File - Origin

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista
 - [x] Windows XP

## Artifact Location(s)
This artifact is present in an alternate data stream (ADS) of the file of interest for analysis. 

## Artifact Parsers
 - FTK Imager
 - KAPE
 - MFTEcmd

## Artifact Interpretation
The following Zone IDs are available:

| Zone ID | Description |
| - | - |
| -1 | No Zone |
| 0 | My Computer |
| 1 | Intranet |
| 2 | Trusted |
| 3 | Internet (AKA Mark of the Web) |
| 4 | Untrusted |

A Zone Identifier of 4 indicates that Microsoft SmartScreen has determined a file is suspicious.

As an example, a Zone.Identifier file may look like this:

```
[ZoneTransfer]
ZoneId=3
ReferrerUrl=http://example.com/example.exe
HostUrl=http://example.com/example.exe
```

In this example, in addition to the Zone Identifier being present, the associated URLs are also noted as well. This behavior is dependent on the application used to download the file. Browsers such as Chrome and Microsoft Edge will append this information to the Zone.Identifier file.

> [!IMPORTANT]
> Some software does not assign Zone Identifiers to files that originate from the internet, such as git. Other software, such as 7zip, may or may not assign Zone Identifiers. 