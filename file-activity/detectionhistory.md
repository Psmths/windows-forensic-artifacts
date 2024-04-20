# Windows Defender Detection History Files
Detection history files are an artifact present on endpoints with Windows Defender installed and with Real-Time Protection enabled. In the event that defender quarantines/removes a potentially malicious item, a detection history file is created to record the event.

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)

### Analysis Value
 - [x] Execution - Evidence of Execution
 - [x] File - Path
 - [x] File - Hash

## Operating System Availability
 - Windows Defender

## Artifact Location(s)
 - `%ProgramData%\Microsoft\Windows Defender\Scans\History\Service\DetectionHistory\[0-9]*\`

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

## Example
In the following example, an EICAR test file, `eicar.com` was downloaded. This activity generated a Windows Defender Alert, and produced a Detection History file located at `C:\ProgramData\Microsoft\Windows Defender\Scans\History\Service\DetectionHistory\19\BA74399A-42A4-4774-95A0-B0758DE6C5A3`. The creation time of this file corresponds to the time at which Windows Defender scanned and quarantined this file at `2024-04-18 16:24 UTC`.

Parsing the Detection History file with [defender-detectionhistory-parser](https://github.com/jklepsercyber/defender-detectionhistory-parser) yields the following result:

```
{
    "GUID": "ba74399a-42a4-4774-95a0-b0758de6c5a3",
    "Magic.Version": "1.2",
    "Virus": "DOS/EICAR_Test_File",
    "ThreatStatusID": 1,
    "file": "C:\\Users\\user\\Downloads\\eicar.com",
    "ThreatTrackingSha256": "275a021bbfb6489e54d471899f7db9d1663fc695ec2fe2a2c4538aabf651fd0f",
    "ThreatTrackingSigSeq": "0x00000555dc2dddb0",
    "ThreatTrackingId": "B9BB9827-A9BD-4DBB-94CD-73F628E18D97",
    "ThreatTrackingStartTime": "04-18-2024 16:24:49",
    "ThreatTrackingThreatName": "Virus:DOS/EICAR_Test_File",
    "ThreatTrackingSha1": "3395856ce81f2b7382dee72602f798b642f14140",
    "ThreatTrackingSigSha": "72aaf9bab94826d9c67ec38c90de7db8246fb157",
    "ThreatTrackingSize": 68,
    "ThreatTrackingMD5": "44d88612fea8a8f36de82e1278abb02f",
    "ThreatTrackingScanFlags": "",
    "ThreatTrackingIsEsuSig": "",
    "ThreatTrackingThreatId": 2147519003,
    "ThreatTrackingScanSource": "",
    "ThreatTrackingScanType": "",
    "User": "DESKTOP-DR5P34J\\user",
    "SpawningProcessName": "Unknown"
}
```