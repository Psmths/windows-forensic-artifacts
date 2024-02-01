# Master File Table
The Mast File Table (MFT) is a forensic artifact present on NTFS volumes that maintains a record of every file on the volume (inlcuding itself), providing detailed timestamps, size information, alternate data streams, access control lists, and more. In some cases, the MFT may even include "resident" files that can be extracted directly from the MFT without having the rest of the volume. 

### Analysis Value
 - [x] File - Creation
 - [x] File - Last Modified
 - [x] File - Path
 - [x] File - Size

## Operating System Availability
 - NTFS Volumes

## Artifact Location(s)
 - `$MFT`

## Artifact Parsers
 - Velociraptor
 - jp (TZWorks)
 - MFTEcmd (Eric Zimmerman)
 - KAPE can be used to extract

## Artifact Interpretation

WIP

### Resident Files
Depending on certain filesystem variables, some files may be "resident" within the MFT, meaning the entire file's contents are contained in its MFT entry. The maximum size limit is typically around 600 bytes. 

## Caveats
The MFT when extracted may not show deleted (deallocated) files. Consider searching MFTs located in Volume Shadow Copies for evidence of deleted files, or the USN Journal if the time range is appropriate. 