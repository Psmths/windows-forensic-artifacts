<a name="readme-top"></a>
![Windows Forensic Artifacts Guide](/media/title.png)
<p align="center">
  <img src="https://img.shields.io/github/license/Psmths/windows-forensic-artifacts.svg">
  <img src="https://www.repostatus.org/badges/latest/wip.svg">
</p>

This repository serves as a guide to a variety of Windows forensic artifacts that may be levereaged during an investigation. It is meant to offer succint information regarding these artifacts, such as their location, parsers that are available for them, and how to interpret the results of a forensic acquisition of these artifacts.

With each new major version of the Windows operating system, forensic artifacts may be added, modified, or removed. Because of this, their applicability to an investigation may depend on what version of windows an endpoint has installed. 

Such cases are identified in this repository as follows:
 - ⚠️ Denotes that a forensic artifact may have certain information available that depends on the operating system version.
 - ❌ Denotes that a forensic artifact is not present on a certain operating system version.

# Contents

 - [Artifacts by Category](#artifacts-by-category)
   * [Execution](#execution)
   * [Account Activity](#account-activity)
   * [File Activity](#file-activity)
 - [Artifact Behavioral Mappings](#artifact-behavioral-mappings) 
   * [TA0002 - Execution](#ta0002-execution)
   * [TA0003 - Persistence](#ta0003-persistence)
   * [TA0008 - Lateral Movement](#ta0008-lateral-movement)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

# Artifacts by Category

The forensic artifacts described in this repository are split into the following categories:

 - [Execution](#execution)
 - [Account Activity](#account-activity)
 - [File Activity](#file-activity)

## Execution
Execution artifacts may provide the following information:

 - Execution - Command Line Options
 - Execution - Count
 - Execution - First Executed
 - Execution - Last Executed
 - Execution - Permissions / Account
 - Execution - Process Tree
 - Execution - Time

| Arifact Type | Artifact | Windows 11 | Windows 10 | Windows 8 | Windows 7 | Windows Vista | Windows XP |
| - | - | - | - | - | - | - | - |
| Filesystem | [Prefetch](execution/prefetch.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Eventlog | [4688: A new process has been created](execution/evtx-process-created.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Registry/Memory | [ShimCache](execution/shimcache.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Registry | [AmCache.hve](execution/amcache.md) | ✅ | ✅ | ✅ | ⚠️ | ❌ | ❌ |

## Account Activity
Account activity artifacts may provide the following information:
 - Account - Creation Time
 - Account - Group Membership
 - Account - Last Login
 - Account - Relative Identifier (RID)
 - Account - Security Identifier (SID)

| Arifact Type | Artifact | Windows 11 | Windows 10 | Windows 8 | Windows 7 | Windows Vista | Windows XP |
| - | - | - | - | - | - | - | - |
| Registry | [SAM Hive](account/sam-hive.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

## File Activity
File activity artifacts may provide the following information:

 - File - Deletion
 - File - Hash
 - File - Last Modified
 - File - Origin
 - File - Path
 - File - Size

| Arifact Type | Artifact | Windows 11 | Windows 10 | Windows 8 | Windows 7 | Windows Vista | Windows XP |
| - | - | - | - | - | - | - | - |
| Filesystem | [Zone.Identifier](file-activity/zone-identifier.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| Filesystem | [USN Journal](file-activity/usn-journal.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
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

| Arifact Type | Artifact | Windows 11 | Windows 10 | Windows 8 | Windows 7 | Windows Vista | Windows XP |
| - | - | - | - | - | - | - | - |
| Filesystem | [Prefetch](execution/prefetch.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Eventlog | [4688: A new process has been created](execution/evtx-process-created.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Registry/Memory | [ShimCache](execution/shimcache.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Registry | [AmCache.hve](execution/amcache.md) | ✅ | ✅ | ✅ | ⚠️ | ❌ | ❌ |

## TA0003 Persistence
The below artifacts are related to [persistence activities](https://attack.mitre.org/tactics/TA0003/). Persistence is defined by MITRE as:

> Persistence consists of techniques that adversaries use to keep access to systems across restarts, changed credentials, and other interruptions that could cut off their access. 

The below artifacts may prove useful in identifying instances of persistence on an endpoint:

| Arifact Type | Artifact | Windows 11 | Windows 10 | Windows 8 | Windows 7 | Windows Vista | Windows XP |
| - | - | - | - | - | - | - | - |
| Registry | [Registry Autostarts](persistence/reg-autostarts.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Eventlog | [TaskScheduler/Operational Log](persistence/task-scheduler-operational-log.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Filesystem | [Scheduled Task Files](persistence/task-scheduler-files.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |

## TA0008 Lateral Movement
The below artifacts are related to [lateral movement activities](https://attack.mitre.org/tactics/TA0008/). Lateral movement is defined by MITRE as:

> techniques that adversaries use to enter and control remote systems on a network.

The below artifacts may prove useful in identifying instances of lateral movement to or from an endpoint:

| Arifact Type | Artifact | Windows 11 | Windows 10 | Windows 8 | Windows 7 | Windows Vista | Windows XP |
| - | - | - | - | - | - | - | - |
| Eventlog | [TaskScheduler/Operational Log](persistence/task-scheduler-operational-log.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Filesystem | [Scheduled Task Files](persistence/task-scheduler-files.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
<p align="right">(<a href="#readme-top">back to top</a>)</p>
