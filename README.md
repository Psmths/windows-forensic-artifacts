
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