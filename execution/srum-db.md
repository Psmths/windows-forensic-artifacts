# System Resource Usage Monitor (SRUM)
The SRUM database is a forensic artifact that provides evidence of execution and network activity. It is used by Windows to provide telemetry regarding applications that run on an endpoint. 

It provides 30-60 days of resolution. It is present in both the **registry** as well as the **filesystem**, as an ESE database.

### Behavioral Indications
 - [x] Behavioral - TA0002 - Execution

### Analysis Value
 - [x] Execution - Permissions/Account
 - [x] Execution - Evidence of Execution
 - [x] File - Path
 - [x] Account - Security Identifier (SID)

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8

## Artifact Location(s)
 - `%SystemRoot%\System32\sru\SRUDB.dat` (Filesystem Location)
 - `SOFTWARE\Microsoft\Windows NT\Current Version\SRUM\Extensions` (Registry Location)

## Artifact Parsers
 - [srum-dump](https://github.com/MarkBaggett/srum-dump) (Recommended!)
 - ESEDatabaseView
 - Registry Explorer

## Artifact Interpretation
The recommended method to interpret this artifact is to use the srum-dump parser. It will allow you to specify the path the the SRUDB.dat file, the SOFTWARE registry hive, and easily convert the information into a presentable format in an Excel spreadsheet.

The SRUM database records information from a number of providers. If you are parsing this manually, you will encounter the following IDs:

| Key | Provider |
| - | - |
| `{973F5D5C-1D90-4944-BE8E-24B94231A174}` | Network Data Usage Monitor |
| `{D10CA2FE-6FCF-4F6D-848E-B2E99266FA86}` | Push Notification Provider |
| `{D10CA2FE-6FCF-4F6D-848E-B2E99266FA89}` | Application Resource Usage Provider |
| `{DD6636C4-8929-4683-974E-22C046A43763}` | Network Connectivity Usage Monitor |
| `{FEE4E14F-02A9-4550-B5CE-5FA2DA202E37}` | Energy Usage Provider |

The following information is available from these providers:

 - Network Data Usage Monitor
   - *Tracks network usage on a per-application basis*
   - Application ID
   - User SID
   - Type of interface network traffic traversed (i.e., Ethernet, loopback, IEEE 802.11 wireless, etc.)
   - Bytes sent and received
 - Push Notification Provider
   - *Tracks push notification (WPM) activity on a per-application basis*
   - Application Name
   - User SID
   - Push notification payload size
 - Application Resource Usage
   - Application Name
   - User SID
   - Performance metrics such as CPU time, disk write/read bytes, etc.
 - Network Connectivity Usage Monitor
   - *Tracks each network the endpoint has been connected to.*
   - Time of first connection
   - Duration of connection
   - Type of interface network traffic traversed (i.e., Ethernet, loopback, IEEE 802.11 wireless, etc.)

The srum-dump utility can be used to extract and parse valuable information from the SRUM ESE database. However, manual analysis of this artifact can be an arduous task, as it requires a comprehensive cross-referencing of various sections of the SRUM ESE database. For this reason, it is not recommended to employ manual analysis on this type of artifact.

This artifact is useful for identifying potential data exfiltration events from Windows systems, as it captures network utilization over time, providing insight into the magnitude of the data transfer.

## Caveats
Data collected is written to the SRUM database on the filesystem once per hour to reflect what is stored in the registry, or during system shutdown/reboot events. 

In the event that a proper shutdown was not conducted, the SRUM filesystem database may need to be repaired using a utility such as `esentutl`.

## Examples
![Example Image](/media/examples/srum_dump_output.png)
*Example srum-dump output showing network usage*