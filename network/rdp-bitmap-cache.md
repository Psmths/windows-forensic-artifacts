# RDP Persistent Bitmap Cache
The persistent bitmap cache is a feature of Microsoft Remote Desktop's GDI Acceleration Extension that stores small chunks of images that were present during an RDP session. The goal is to reduce the bandwidth used by a remote desktop connection to increase performance at the cost of storage. As a forensic artifact, these bitmaps are valuable as they provide, to some extent, visual history of RDP sessions that have taken place from a system.

### Behavioral Indications
 - [x] Behavioral - Lateral Movement (TA0008)

### Analysis Value
 - [x] Network - Evidence of Network Activity

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista
 - [x] Windows XP

## Artifact Location(s)
 - `%UserProfile%\AppData\Local\Microsoft\Terminal Server Client\Cache` (Windows Vista and later)
 - `C:\Documents and Settings\{USERNAME}\Local Settings\Application Data\Microsoft\Terminal Server Client\Cache` (Windows XP)

## Artifact Parsers
 - [bmc-tools](https://github.com/ANSSI-FR/bmc-tools)

## Artifact Interpretation
In this artifact's directory, you will find two types of files, each serving a similar purpose, `.bmc` and `.bin`. The `.bmc` files are typically observed on older versions of Microsoft Remote Desktop client, whereas newer versions will store data in the `.bin` files. The [bmc-tools](https://github.com/ANSSI-FR/bmc-tools) utility is able to parse both of these filetypes.

When the bitmap data is extracted, it will provide tiles containing snapshots of activity that has taken place interactively through an RDP session. They will be scrambled in no particular order, so it may take some work to stitch these images back together in a coherent manner.

The presence of these `.bmc` and `.bin` files indicates that the system on which they were found established an RDP connection to another system through RDP.

In modern versions of Microsoft Remote Desktop Client, the `.bmc` file is created when and RDP connection is established, and the `.bin` files are created or modified when that RDP connection is closed. As such, the most recent modification timestamp among these files indicates the last time an RDP connection was established from the system, assuming that `Persistent Bitmap Caching` was enabled for that session. In the case that it was not enabled, neither the `.bmc` nor the `.bin` files will be created.