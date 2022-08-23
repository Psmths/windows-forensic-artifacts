<p align="center">
  <h1 align="center">Windows Forensic Artifacts Guide</h2>
</p>
<p align="center">
  <img src="https://img.shields.io/github/license/Psmths/windows-forensic-artifacts.svg">
  <img src="https://www.repostatus.org/badges/latest/wip.svg">
</p>
<hr>

This repository serves as a guide to a variety of Windows forensic artifacts that may be levereaged during an investigation. It is meant to offer succint information regarding these artifacts, such as their location, parsers that are available for them, and how to interpret the results of a forensic acquisition of these artifacts.

With each new major version of the Windows operating system, forensic artifacts may be added, modified, or removed. Because of this, their applicability to an investigation may depend on what version of windows an endpoint has installed. 

Such cases are identified in this repository as follows:
 - ⚠️ Denotes that a forensic artifact may have certain information available that depends on the operating system version.
 - ❌ Denotes that a forensic artifact is not present on a certain operating system version.

## Contents
The forensic artifacts described in this repository are split into the following categories:

 - [Execution](#execution)
 - [Account Usage and Modification](#account-usage-and-modification)
 - [Browser Activity](#browser-activity)
 - [File Activity](#file-activity)
 - [Persistence](#persistence)

## Execution
| Arifact Type | Artifact | Windows 11 | Windows 10 | Windows 8 | Windows 7 | Windows Vista | Windows XP |
| - | - | - | - | - | - | - | - |
| Filesystem | [Prefetch](execution/prefetch.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Eventlog | [4688: A new process has been created](execution/evtx-process-created.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Registry/Memory | [ShimCache](execution/shimcache.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Registry | [AmCache.hve](execution/amcache.md) | ✅ | ✅ | ✅ | ⚠️ | ❌ | ❌ |

## Account Usage and Modification
| Arifact Type | Artifact | Windows 11 | Windows 10 | Windows 8 | Windows 7 | Windows Vista | Windows XP |
| - | - | - | - | - | - | - | - |
| Registry | [SAM Hive](account/sam-hive.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

## Browser Activity

## File Activity
| Arifact Type | Artifact | Windows 11 | Windows 10 | Windows 8 | Windows 7 | Windows Vista | Windows XP |
| - | - | - | - | - | - | - | - |
| Filesystem | [Zone.Identifier](file-activity/zone-identifier.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ |

## Persistence
| Arifact Type | Artifact | Windows 11 | Windows 10 | Windows 8 | Windows 7 | Windows Vista | Windows XP |
| - | - | - | - | - | - | - | - |
| Registry | [Registry Autostarts](persistence/reg-autostarts.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |