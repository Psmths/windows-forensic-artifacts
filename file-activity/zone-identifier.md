# NTFS Zone Identifiers
Zone Identifiers, commonly referred to as Mark of the Web (MOTW), are security metadata associated with files that originate from potentially untrusted locations, such as the Internet, email attachments, and network resources.

On NTFS volumes, this information is stored in an alternate data stream (ADS) named Zone.Identifier, attached to the file. Windows Attachment Manager and applications that implement download-origin metadata use this information to identify the security zone from which a file originated.

Windows uses this information to apply additional security protections, including security warnings, Microsoft Defender SmartScreen checks, and Microsoft Office Protected View.

### Analysis Value
 - [x] File - Creation
 - [x] File - Origin

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista
 - [x] Windows XP (SP 2 and Later)

## Artifact Location(s)
This artifact is present in an alternate data stream (ADS) of the file of interest for analysis.

The stream is named:

`<Filename>:Zone.Identifier:$DATA`

For example:

`C:\Users\user1\Downloads\example.exe:Zone.Identifier`

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

A Zone Identifier of 3 indicates that the file was associated with the Internet security zone. This is commonly referred to as Mark of the Web.

A Zone Identifier of 4 indicates that the file was associated with the Restricted Sites security zone.

The following additional metadata may be present, depending on the application responsible for saving the file:

- **ReferrerUrl**: The URL of the webpage from which the download originated or was initiated.
- **HostUrl**: The URL from which the file was downloaded.
- **HostIpAddress**: The IP address associated with the download source, when provided.

As an example, a Zone.Identifier file may look like this:

```
[ZoneTransfer]
ZoneId=3
ReferrerUrl=http://example.com/example.exe
HostUrl=http://example.com/example.exe
```

In this example, the file is associated with the Internet security zone. The additional URL fields provide information about the webpage that initiated the download and the URL of the downloaded resource.

The availability of these additional fields depends on the application used to download or save the file. Modern versions of browsers such as Google Chrome, Microsoft Edge, and Mozilla Firefox can populate this metadata.

> [!IMPORTANT]
> Some software does not assign Zone Identifiers to files that originate from the internet, such as git. Other software, such as 7zip, may or may not assign Zone Identifiers.

## Caveats
- **Application-dependent creation:** Not all applications preserve zone information when saving files. The presence of a Zone.Identifier stream depends on the application, its implementation, and the applicable Windows security policies.
- **NTFS dependency:** Alternate data streams require NTFS. Zone.Identifier streams are not preserved when files are copied to filesystems that do not support ADS, such as FAT32 and exFAT.
- **Archive extraction:** Whether extracted files inherit the Zone.Identifier stream depends on the extraction utility and its configuration. Windows Explorer's built-in ZIP extraction can propagate MOTW.
- **URL reliability:** ReferrerUrl and HostUrl are useful investigative leads but should be corroborated with browser download history and other network artifacts. Their presence and accuracy depend on the application that created the stream.
- **Absence is not proof of local origin:** A missing Zone.Identifier stream does not establish that a file originated locally. The stream may never have been created, may have been removed, or may have been lost during copying, extraction, or transfer.
