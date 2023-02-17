<a name="readme-top"></a>
![Windows Forensic Artifacts Guide](/media/title-2.png)
<p align="center">
  <img src="https://img.shields.io/github/license/Psmths/windows-forensic-artifacts.svg">
  <img src="https://www.repostatus.org/badges/latest/wip.svg">
</p>

This repository provides an in-depth guide to the various Windows forensic artifacts that can be utilized when conducting an investigation. Detailed information is provided for each artifact, including its location, available parsing tools, and instructions for interpreting the results of a forensic data extraction. Furthermore, the repository seeks to provide a comprehensive resource for those seeking to expand their understanding of Windows forensics artifacts and how to properly leverage them during a forensic investigation.

# Contents
 - [Types of Windows Artifacts](#types-of-windows-artifacts)
 - [How to Use this Guide](#how-to-use-this-guide)
 - [Artifacts by Category](#artifacts-by-category)
   * [Execution](#execution)
     - [Command Line Options / Full Path](#execution---command-line-options)
     - [First Executed](#execution---first-executed)
     - [Last Executed](#execution---last-executed)
     - [Permissions/Account](#execution---permissions--account)
     - [Process Geneaology/Tree](#execution---process-tree)
     - [Execution Time](#execution---time)
     - [Evidence of Execution](#execution---evidence-of-execution)
   * [Account Activity](#account-activity)
     - [Account Creation Time](#account---creation-time)
     - [Group Membership](#account---group-membership)
     - [Last Login](#account---last-login)
     - [Login History](#account---login-history)
     - [Logon ID](#account---logon-id)
     - [Relative Identifier (RID)](#account---relative-identifier-rid)
     - [Security Identifier (SID)](#account---security-identifier-sid)
     - [Username](#account---username)
   * [File Activity](#file-activity)
     - [Creation](#file---creation)
     - [Deletion](#file---deletion)
     - [Hash](#file---hash)
     - [Last Modified](#file---last-modified)
     - [Origin](#file---origin)
     - [Path](#file---path)
     - [Size](#file---size)
   * [Network Activity](#network-activity)
     - [Destination Identification](#network-activity---destination-identification)
     - [Source Identification](#network-activity---source-identification)
     - [Browser Activity](#network-activity---browser-activity)
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

# How to Use this Guide

This guide was created to classify the numerous Windows forensic artifacts and provide a concise list of what information they respectively provide. While it may be used as a general reference, it shines when it comes time to tie separate artifacts together based on mutual/shared datapoints.

For instance, if it is known that an attacker has logged into an endpoint around a certain time, an analyst may want to determine what activity on the endpoint can be attributed to this session. For this, the analyst might begin by looking at 4624 Login events and pull the `Logon ID` from this artifact. This guide provides a list of every artifact that has the `Logon ID` field present, providing a quick way to correlate logon activity with other activity on the endpoint filed under the section [Logon ID](#account---logon-id).

As another example, say for instance you are aware that an endpoint may have a malicious file on it. Maybe you want to see [when the file was created](#file---creation), or [when it was first executed](#execution---first-executed). What about determining [what Logon ID is associated with the execution with 4688 events?](#account---logon-id)

Building a visual map in your mind of the relationships between all the artifacts present in Windows is necessary to allow for an analyst to efficiently pivot their focus during an investigation, this guide simply lays it all out and provides useful analysis tips collected during years of forensic experience while doing so.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

# Artifacts by Category

The forensic artifacts described in this repository are split into the following categories:

 - [Execution](#execution)
 - [Account Activity](#account-activity)
 - [File Activity](#file-activity)
 - [Network Activity](#network-activity)

## Execution
Execution artifacts may provide the following information:

### Execution - Command Line Options / Full Path
>What command line was used to spawn this process?

- [Security/4688: A new process has been created](execution/evtx-4688-process-created.md)
- [Scheduled Task Files](persistence/task-scheduler-files.md)
- [TaskScheduler/Operational Log](persistence/task-scheduler-operational-log.md)
- [Detection History Files](file-activity/detectionhistory.md)

### Execution - First Executed
>When was this executable furst run?
 - [Prefetch](execution/prefetch.md)
 - [AmCache.hve](execution/amcache.md)
 - [Scheduled Task Files](persistence/task-scheduler-files.md)
 - [TaskScheduler/Operational Log](persistence/task-scheduler-operational-log.md)

### Execution - Last Executed
>When was the last time this executable was run?
 - [Prefetch](execution/prefetch.md)
 - [Scheduled Task Files](persistence/task-scheduler-files.md)
 - [TaskScheduler/Operational Log](persistence/task-scheduler-operational-log.md)
 - [Background Activity Montitor](execution/bam-dam.md)
 - [Program Compatibility Assistant (PCA)](execution/program-compatibility-assistant.md)

### Execution - Permissions / Account
>What permissions does the process have?
>What account launced the process?
 - [Security/4688: A new process has been created](execution/evtx-4688-process-created.md)
 - [Scheduled Task Files](persistence/task-scheduler-files.md)
 - [Background Activity Montitor](execution/bam-dam.md)
 - [SRUM Database](execution/srum-db.md)

### Execution - Process Tree
>How did this process come to be? What spawned this process?
 - [Security/4688: A new process has been created](execution/evtx-4688-process-created.md)

### Execution - Time
>When was this process spawned?
 - [Security/4688: A new process has been created](execution/evtx-4688-process-created.md)
 - [Scheduled Task Files](persistence/task-scheduler-files.md)
 - [TaskScheduler/Operational Log](persistence/task-scheduler-operational-log.md)

### Execution - Evidence of Execution
>Was a process spawned?
 - [AmCache.hve](execution/amcache.md)
 - [Background Activity Montitor](execution/bam-dam.md)
 - [Security/4688: A new process has been created](execution/evtx-4688-process-created.md)
 - [Prefetch](execution/prefetch.md)
 - [ShimCache](execution/shimcache.md)
 - [SRUM Database](execution/srum-db.md)
 - [Detection History Files](file-activity/detectionhistory.md)
 - [Scheduled Task Files](persistence/task-scheduler-files.md)
 - [TaskScheduler/Operational Log](persistence/task-scheduler-operational-log.md)
 - [Program Compatibility Assistant (PCA)](execution/program-compatibility-assistant.md)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Account Activity
Account activity artifacts may provide the following information:
### Account - Creation Time
> When was this account created?
 - [SAM Hive](account/sam-hive.md)
 - [Security/4720: A user account was created](account/evtx-4720-account-created.md)

### Account - Group Membership
> What groups is the account a member of?
 - [SAM Hive](account/sam-hive.md)
 - [Group Membership Registry Key](account/group-membership-key.md)

### Account - Last Login
> When did this account last log in?
 - [SAM Hive](account/sam-hive.md)

### Account - Login History
> Identification of specific instances of account logins
 - [Security/4778: Session reconnected](network/evtx-4778-session-reconnected.md)
 - [Security/4648: Logon using explicit credentials](account/evtx-4648-explicit-credentials.md)
 - [Security/4624: An account was successfully logged on](account/evtx-4624-successful-logon.md)
 - [Security/4625: An account failed to log on](account/evtx-4625-failed-logon.md)

### Account - Logon ID
> Certain activity can be tied to login sessions by means of a `Logon ID`
 - [Security/4688: A new process has been created](execution/evtx-4688-process-created.md)
 - [Security/4720: A user account was created](account/evtx-4720-account-created.md)
 - [Security/4778: Session reconnected](network/evtx-4778-session-reconnected.md)
 - [TerminalServices-RDPClient/Operational/1102](network/evtx-1102-rdp-activex.md)
 - [Security/4648: Logon using explicit credentials](account/evtx-4648-explicit-credentials.md)
 - [Security/4624: An account was successfully logged on](account/evtx-4624-successful-logon.md)

### Account - Relative Identifier (RID)
> What is the account's [Relative Identifier](https://en.wikipedia.org/wiki/Relative_identifier)?
 - [SAM Hive](account/sam-hive.md)

### Account - Security Identifier (SID)
> What is the account's [Security Identifier](https://en.wikipedia.org/wiki/Security_Identifier)?
 - [SAM Hive](account/sam-hive.md)
 - [Security/4720: A user account was created](account/evtx-4720-account-created.md)
 - [Security/4778: Session reconnected](network/evtx-4778-session-reconnected.md)
 - [TerminalServices-RDPClient/Operational/1102](network/evtx-1102-rdp-activex.md)
 - [Security/4648: Logon using explicit credentials](account/evtx-4648-explicit-credentials.md)
 - [Security/4624: An account was successfully logged on](account/evtx-4624-successful-logon.md)
 - [Security/4625: An account failed to log on](account/evtx-4625-failed-logon.md)
 - [WMI-Activity/Operational/5861: New WMI Event Consumer](persistence/evtx-5861-event-consumer-created.md)
 - [SRUM Database](execution/srum-db.md)
 - [ProfileList](account/profile-list-key.md)

### Account - Username
> Determining the username attached to a particular SID
 - [ProfileList](account/profile-list-key.md)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## File Activity
File activity artifacts may provide the following information:

### File - Creation
> When was the file created?
 - [Zone.Identifier](file-activity/zone-identifier.md)
 - [USN Journal](file-activity/usn-journal.md)

### File - Deletion
> When was the file deleted?
 - [USN Journal](file-activity/usn-journal.md)

### File - Hash
> What is the hash of this file?
 - [AmCache.hve](execution/amcache.md)
 - [Detection History Files](file-activity/detectionhistory.md)

### File - Last Modified
> When was the file last modified?
 - [ShimCache](execution/shimcache.md)
 - [AmCache.hve](execution/amcache.md)
 - [USN Journal](file-activity/usn-journal.md)

### File - Origin
> Where did the file come from?
 - [Zone.Identifier](file-activity/zone-identifier.md)

### File - Path
> Where is the file located?
 - [ShimCache](execution/shimcache.md)
 - [AmCache.hve](execution/amcache.md)
 - [Run/RunOnce Keys](persistence/reg-run-runonce.md)
 - [USN Journal](file-activity/usn-journal.md)
 - [Scheduled Task Files](persistence/task-scheduler-files.md)
 - [TaskScheduler/Operational Log](persistence/task-scheduler-operational-log.md)
 - [Background Activity Montitor](execution/bam-dam.md)
 - [SRUM Database](execution/srum-db.md)
 - [Detection History Files](file-activity/detectionhistory.md)
 - [Image File Execution Options](persistence/image-file-execution-options.md)
 - [Program Compatibility Assistant (PCA)](execution/program-compatibility-assistant.md)

### File - Size
> What is the file's size on disk?
 - [ShimCache](execution/shimcache.md)
 - [USN Journal](file-activity/usn-journal.md)


<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Network Activity
Network activity artifacts may provide the following information:

### Network Activity - Destination Identification
> Can the destination for this activity be identified?
 - [TerminalServices-RDPClient/Operational/1102](network/evtx-1102-rdp-activex.md)
 - [Security/4648: Logon using explicit credentials](account/evtx-4648-explicit-credentials.md)

### Network Activity - Source Identification
> Can the source of this activity be identified?
 - [TaskScheduler/Operational Log](persistence/task-scheduler-operational-log.md)
 - [Scheduled Task Files](persistence/task-scheduler-files.md)
 - [Security/4778: Session reconnected](network/evtx-4778-session-reconnected.md)
 - [Security/4624: An account was successfully logged on](account/evtx-4624-successful-logon.md)
 - [Security/4625: An account failed to log on](account/evtx-4625-failed-logon.md)

### Network Activity - Browser Activity
> Artifacts supporting general forensic analysis for browser activity on an endpoint
 - [places.sqlite](network/places-sqlite.md)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

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
| Registry/Filesystem | [SRUM Database](execution/srum-db.md) | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Filesystem | [Program Compatibility Assistant (PCA)](execution/program-compatibility-assistant.md) | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |

## TA0003 Persistence
The below artifacts are related to [persistence activities](https://attack.mitre.org/tactics/TA0003/). Persistence is defined by MITRE as:

> Persistence consists of techniques that adversaries use to keep access to systems across restarts, changed credentials, and other interruptions that could cut off their access. 

The below artifacts may prove useful in identifying instances of persistence on an endpoint:

| Arifact Type | Artifact | 11 | 10 | 8 | 7 | Vista | XP |
| - | - | - | - | - | - | - | - |
| Registry | [Run/RunOnce Keys](persistence/reg-run-runonce.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Eventlog | [TaskScheduler/Operational Log](persistence/task-scheduler-operational-log.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Filesystem | [Scheduled Task Files](persistence/task-scheduler-files.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| Eventlog | [Security/4720: A user account was created](account/evtx-4720-account-created.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| Eventlog | [WMI-Activity/Operational/5861: New WMI Event Consumer](persistence/evtx-5861-event-consumer-created.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| Registry | [Image File Execution Options](persistence/image-file-execution-options.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

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
| Eventlog | [Security/4625: An account failed to log on](account/evtx-4625-failed-logon.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ |



<p align="right">(<a href="#readme-top">back to top</a>)</p>
