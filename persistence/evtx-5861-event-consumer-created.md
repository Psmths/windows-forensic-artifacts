# WMI-Activity/Operational/5861: New WMI Event Consumer
This event, logged to the WMI-Activity/Operational channel, is logged when a new WMI event consumer is registered on the system.

### Behavioral Indications
 - [x] Behavioral - Persistence (TA0003)

### Analysis Value
 - [x] Account - Security Identifier (SID)

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista

## Artifact Location(s)
- `%SystemRoot%\System32\Winevt\Logs\Microsoft-Windows-WMI-Activity%4Operational.evtx`

## Artifact Interpretation
Because new WMI event consumers on Windows enpoints are rather rare, this artifact provides a high-fidelity indicator of persistence activity. The information that you find in this event may vary depending on specific attacker techniques. In general, suspicious WMI event consumers will be of the following types, which are indicated in the event data for event ID `5861`:

 - `CommandLineEventConsumer`
 - `ActiveScriptEventConsumer`

## Analysis Tips
### Proximate Execution
Depending on the method an attacker has used to install a WMI event consumer, they will either have run `mofcomp.exe` or powershell. Consider cross-referencing this finding with artifacts that provide evidence of execution and searching for such activity. 

### Live System Collection
Evidence of WMI event consumers may also be queried on a live system through Powershell's Get-WMI module as follows:

`Get-WMIObject -Namespace root/Subscription -Class CommandLineEventConsumer`

## Example
![Example Image](/media/examples/wmi.png) 