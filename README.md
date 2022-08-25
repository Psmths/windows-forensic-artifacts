<a name="readme-top"></a>
![Windows Forensic Artifacts Guide](/media/title-2.png)
<p align="center">
  <img src="https://img.shields.io/github/license/Psmths/windows-forensic-artifacts.svg">
  <img src="https://www.repostatus.org/badges/latest/wip.svg">
</p>

This repository serves as a guide to a variety of Windows forensic artifacts that may be levereaged during an investigation. It is meant to offer succint information regarding these artifacts, such as their location, parsers that are available for them, and how to interpret the results of a forensic acquisition of these artifacts.

With each new major version of the Windows operating system, forensic artifacts may be added, modified, or removed. Because of this, their applicability to an investigation may depend on what version of windows an endpoint has installed. 

Such cases are identified in this repository as follows:
| Icon | Interpretation |
| - | - |
| ✅ | Denotes that the forensic artifact is fully supported on this operating system version |
| ⚠️ | Denotes that a forensic artifact may have certain information available that depends on the operating system version |
| ❌ | Denotes that a forensic artifact is not present on a certain operating system version |

# Contents
 - [Types of Windows Artifacts](#types-of-windows-artifacts)
 - [Artifacts by Category](#artifacts-by-category)
   * [Execution](#execution)
   * [Account Activity](#account-activity)
   * [File Activity](#file-activity)
   * [Network Activity](#network-activity)
 - [Artifact Behavioral Mappings](#artifact-behavioral-mappings) 
   * [TA0002 - Execution](#ta0002-execution)
   * [TA0003 - Persistence](#ta0003-persistence)
   * [TA0008 - Lateral Movement](#ta0008-lateral-movement)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

# Types of Windows Artifacts
Forensic artifacts on the Windows operatying system can generally be split into four main categories:

 1. Registry 
 2. Filesystem
 3. Event Log
 4. Memory

**Registry** artifacts are found in the Windows registry, which is loaded into memory while a system is in operation and written to disk during shutdown. The registry stores low-level configuration settings for the operating system and contains a wealth of forensic artifacts of interest to an analyst.

**Filesystem** artifacts are artifacts that arise due to the operation of Windows' filesystem - NTFS (New Technology File System).

**Event log** artifacts are found in the Windows event log and consist primarily of audit logs from the operating system and its applications. 

**Memory** artifacts are those artifacts found in the endpoint's memory while it is operational. These artifacts must be collected from a live system, and are generally not applicable to dead disk forensics with certain exceptions such as page files and hibernation files that consist of memory that has been written to the disk. 

A complete forensic analysis of a Windows endpoint will consist of one or all of these artifacts. They may be collected and parsed individually at the analyst's discretion, or consolidated into "super timelines" with forensic software such as [log2timeline](https://github.com/log2timeline).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

# Artifacts by Category

The forensic artifacts described in this repository are split into the following categories:

 - [Execution](#execution)
 - [Account Activity](#account-activity)
 - [File Activity](#file-activity)
 - [Network Activity](#network-activity)

## Execution
Execution artifacts may provide the following information:

 - Execution - Command Line Options
   - What command line was used to spawn this process?
 - Execution - Count
   - How many times was this executable run?
 - Execution - First Executed
   - When was this executable furst run?
 - Execution - Last Executed
   - When was the last time this executable was run?
 - Execution - Permissions / Account
   - What permissions does the process have?
   - What account launced the process?
 - Execution - Process Tree
   - How did this process come to be? What spawned this process?
 - Execution - Time
   - When was this process spawned?

| Arifact Type | Artifact | 11 | 10 | 8 | 7 | Vista | XP |
| - | - | - | - | - | - | - | - |
| Filesystem | [Prefetch](execution/prefetch.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Eventlog | [Security/4688: A new process has been created](execution/evtx-4688-process-created.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Registry/Memory | [ShimCache](execution/shimcache.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Registry | [AmCache.hve](execution/amcache.md) | ✅ | ✅ | ✅ | ⚠️ | ❌ | ❌ |
| Filesystem | [Scheduled Task Files](persistence/task-scheduler-files.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| Eventlog | [TaskScheduler/Operational Log](persistence/task-scheduler-operational-log.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |

## Account Activity
Account activity artifacts may provide the following information:
 - Account - Creation Time
   - When was this account created?
 - Account - Group Membership
   - What groups is the account a member of?
 - Account - Last Login
   - When did this account last log in?
 - Account - Login History
 - Account - Logon ID
   - Certain activity can be tied to login sessions by means of a `Logon ID`
 - Account - Relative Identifier (RID)
   - What is the account's [Relative Identifier](https://en.wikipedia.org/wiki/Relative_identifier)?
 - Account - Security Identifier (SID)
   - What is the account's [Security Identifier](https://en.wikipedia.org/wiki/Security_Identifier)?

| Arifact Type | Artifact | 11 | 10 | 8 | 7 | Vista | XP |
| - | - | - | - | - | - | - | - |
| Registry | [SAM Hive](account/sam-hive.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Eventlog | [Security/4720: A user account was created](account/evtx-4720-account-created.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| Eventlog | [Security/4648: Logon using explicit credentials](account/evtx-4648-explicit-credentials.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| Eventlog | [Security/4624: An account was successfully logged on](account/evtx-4624-successful-logon.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ |

## File Activity
File activity artifacts may provide the following information:

 - File - Creation
   - When was the file created?
 - File - Deletion
   - When was the file deleted?
 - File - Hash
   - What is the hash of this file?
 - File - Last Modified
   - When was the file last modified?
 - File - Origin
   - Where did the file come from?
 - File - Path
   - Where is the file located?
 - File - Size
   - What is the file's size on disk?

| Arifact Type | Artifact | 11 | 10 | 8 | 7 | Vista | XP |
| - | - | - | - | - | - | - | - |
| Filesystem | [Zone.Identifier](file-activity/zone-identifier.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| Filesystem | [USN Journal](file-activity/usn-journal.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Network Activity
Network activity artifacts may provide the following information:

 - Network Activity - Destination Identification
   - Can the destination for this activity be identified?
 - Network Activity - Source Identification
   - Can the source of this activity be identified?

| Arifact Type | Artifact | 11 | 10 | 8 | 7 | Vista | XP |
| - | - | - | - | - | - | - | - |
| Eventlog | [Security/4778: Session reconnected](network/evtx-4778-session-reconnected.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| Eventlog | [TerminalServices-RDPClient/Operational/1102](network/evtx-1102-rdp-activex.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| Filesystem | [Scheduled Task Files](persistence/task-scheduler-files.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| Eventlog | [TaskScheduler/Operational Log](persistence/task-scheduler-operational-log.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |

# Artifact Behavioral Mappings

Additionally, these artifacts may be roughly mapped to the MITRE ATT&CK framework to perform analysis on a behavioral basis:

 - [TA0002 - Execution](#ta0002-execution)
 - [TA0003 - Persistence](#ta0003-persistence)
 - [TA0008 - Lateral Movement](#ta0008-lateral-movement)

## TA0002 Execution
The below artifacts are related to [execution](https://attack.mitre.org/tactics/TA0002/). Execution is defined by MITRE as:

> ...techniques that result in adversary-controlled code running on a local or remote system. 

The below artifacts may prove useful in identifying instances of execution on an endpoint:

| Arifact Type | Artifact | 11 | 10 | 8 | 7 | Vista | XP |
| - | - | - | - | - | - | - | - |
| Filesystem | [Prefetch](execution/prefetch.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Eventlog | [Security/4688: A new process has been created](execution/evtx-4688-process-created.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Registry/Memory | [ShimCache](execution/shimcache.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Registry | [AmCache.hve](execution/amcache.md) | ✅ | ✅ | ✅ | ⚠️ | ❌ | ❌ |
| Filesystem | [Scheduled Task Files](persistence/task-scheduler-files.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| Eventlog | [TaskScheduler/Operational Log](persistence/task-scheduler-operational-log.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |

## TA0003 Persistence
The below artifacts are related to [persistence activities](https://attack.mitre.org/tactics/TA0003/). Persistence is defined by MITRE as:

> Persistence consists of techniques that adversaries use to keep access to systems across restarts, changed credentials, and other interruptions that could cut off their access. 

The below artifacts may prove useful in identifying instances of persistence on an endpoint:

| Arifact Type | Artifact | 11 | 10 | 8 | 7 | Vista | XP |
| - | - | - | - | - | - | - | - |
| Registry | [Registry Autostarts](persistence/reg-autostarts.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Eventlog | [TaskScheduler/Operational Log](persistence/task-scheduler-operational-log.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Filesystem | [Scheduled Task Files](persistence/task-scheduler-files.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| Eventlog | [Security/4720: A user account was created](account/evtx-4720-account-created.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ |

## TA0008 Lateral Movement
The below artifacts are related to [lateral movement activities](https://attack.mitre.org/tactics/TA0008/). Lateral movement is defined by MITRE as:

> techniques that adversaries use to enter and control remote systems on a network.

The below artifacts may prove useful in identifying instances of lateral movement to or from an endpoint:

| Arifact Type | Artifact | 11 | 10 | 8 | 7 | Vista | XP |
| - | - | - | - | - | - | - | - |
| Eventlog | [TaskScheduler/Operational Log](persistence/task-scheduler-operational-log.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Filesystem | [Scheduled Task Files](persistence/task-scheduler-files.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| Eventlog | [Security/4778: Session reconnected](network/evtx-4778-session-reconnected.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| Eventlog | [TerminalServices-RDPClient/Operational/1102](network/evtx-1102-rdp-activex.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| Eventlog | [Security/4648: Logon using explicit credentials](account/evtx-4648-explicit-credentials.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| Eventlog | [Security/4624: An account was successfully logged on](account/evtx-4624-successful-logon.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ |


<p align="right">(<a href="#readme-top">back to top</a>)</p>
