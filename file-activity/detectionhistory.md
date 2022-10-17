# Windows Defender Detection History Files
Detection history files are an artifact present on endpoints with Windows Defender installed and with Real-Time Protection enabled. In the event that defender quarantines/removes a potentially malicious item, a detection history file is created to record the event.

### Analysis Value
 - [x] File - Path
 - [x] File - Hash
 - [x] Execution - Full Path
 - [x] Execution - Evidence of Execution

## Operating System Availability
 - Windows Defender

## Artifact Location(s)
 - `C:\ProgramData\Microsoft\Windows Defender\Scans\History\Service\DetectionHistory\[0-9]*\`

## Artifact Parsers
 - KAPE (Extraction and Parsing)
 - [defender-detectionhistory-parser](https://github.com/jklepsercyber/defender-detectionhistory-parser)

## Artifact Interpretation
The presence of this artifact indicates that the file the artifact was created for existed on the disk, and that it was flagged as malicious and quarantined/removed by Windows Defender. When parsed, it provides basic information such as what class of threat (Trojan, RAT, etc.) the file is, as well as additional metadata such as:

 - The file's SHA-256 and MD5 hash
 - The user responsible for the file's creation
 - The process responsible for the file's creation

In the event that the malicious item is a registry key, the registry key will be present in this artifact.

More detailed information regarding the structure of this artifact can be found [here](https://github.com/libyal/dtformats/blob/main/documentation/Windows%20Defender%20scan%20DetectionHistory%20file%20format.asciidoc).