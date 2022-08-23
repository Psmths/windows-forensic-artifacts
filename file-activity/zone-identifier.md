# NTFS Zone Identifiers
Zone Identifiers are an artifact that reside in alternate data streams within an NTFS volume. This artifact is used to determine the origins of a file on a disk, and can differentiate between files from the following origins:

 - Downloaded from the internet (potentially, the source URL as well)
 - Downloaded from an intranet
 - Downloaded from a trusted resource

⚠️ In Windows XP, only executable files will be tagged with a Zone Identifier. Later operating systems will tag all files.

## Operating System Availability
 - Windows 10
 - Windows 8
 - Windows 7
 - Windows Vista
 - Windows XP

## Artifact Location(s)
This artifact is present in an alternate data stream (ADS) of the file of interest for analysis. 

## Artifact Parsers
 - FTK Imager

## Artifact Interpretation

The following Zone IDs are available:

| Zone ID | Description |
| - | - |
| -1 | No Zone |
| 0 | My Computer |
| 1 | Intranet |
| 2 | Trusted |
| 3 | Internet |
| 4 | Untrusted |

As an example, a Zone.Identifier file may look like this:

```
[ZoneTransfer]
ZoneId=3
ReferrerUrl=http://example.com/example.exe
HostUrl=http://example.com/example.exe
```

In this example, in addition to the Zone Identifier being present, the associated URLs are also noted as well. This behavior is dependent on the application used to download the file. Browsers such as Chrome and Microsoft Edge will append this information to the Zone.Identifier file.