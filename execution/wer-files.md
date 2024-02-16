# Windows Error Reporting Files (.WER)
Windows Error Reporting is a component of Windows that allows for users to send crash reports to Microsoft. Windows Error Reporting files provide information about the crash and are useful to a forensic analysis to provide evidence of execution.

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)
 
### Analysis Value
 - [x] Execution - Evidence of Execution
 - [x] File - Path

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Server 2019
 - [x] Windows Server 2016
 - [x] Windows Server 2012 R2
 - [x] Windows Server 2012
 - [x] Windows Server 2008 R2
 - [x] Windows Server 2008

## Artifact Location(s)
 - `%ProgramData%\Microsoft\Windows\WER\ReportArchive`
 - `%ProgramData%\Microsoft\Windows\WER\ReportQueue`
 - `%UserProfile%\AppData\Local\Microsoft\Windows\WER\ReportArchive`
 - `%UserProfile%\AppData\Local\Microsoft\Windows\WER\ReportQueue`

## Artifact Interpretation
Within the aforementioned directories, folders containing .WER files may be found. 

These are created when either:

 - A user-mode applications crashes (AppCrash_ ...)
 - A user-mode application hangs (AppHang_ ...)
 - A kernel crash occurs (Kernel_ ...)

For evidence of execution, the AppCrash and AppHang folders are most interesting. The folders contain the application name, for example, `AppHang_Bginfo64.exe_cf919d50e71d613a2bddb1a116ff8eebb4e5c_140e09f3_c2b585e7-ac5d-4cf9-bd79-7b6d4fe6075c`.

Each folder represents one instance of an application crashing or hanging, and may contain a variety of files apart from the .WER file, such as minidumps. The .WER files contain information about crash reports that occurred and mirror closely the information represented in the Windows Reliability History control panel page.

The .WER file will contain a wealth of information, such as:

 - The full path to the application that crashed or froze
 - The modules that the application loaded
 - Application metadata, such as version, name, etc.
 - OS metadata such as OS version, architecture, etc.

## Example
The following example shows the result of an application, `WinSCP.exe`, experiencing a fault and crashing (it has been reduced to include the most interesting information from this artifact):

Note the `EventTime` is a Windows Filetime timestamp. In this example, it translates to `Sat 18 February 2023 05:14:17 UTC`, which corresponds to the creation time of this .WER file. From this we can confirm that the application located at `C:\Program Files (x86)\WinSCP\WinSCP.exe` was executed sometime before it crashed at `2023-02-18T05:14:17.000Z`.

```
Version=1
EventType=APPCRASH
EventTime=133211708571634483
NsAppName=WinSCP.exe
OriginalFilename=winscp.exe
Sig[0].Name=Application Name
Sig[0].Value=WinSCP.exe
Sig[1].Name=Application Version
Sig[1].Value=5.21.5.12858
Sig[2].Name=Application Timestamp
Sig[2].Value=00000000
Sig[3].Name=Fault Module Name
Sig[3].Value=MSHTML.dll
Sig[4].Name=Fault Module Version
Sig[4].Value=11.0.19041.2604
Sig[5].Name=Fault Module Timestamp
Sig[5].Value=709ac760
Sig[6].Name=Exception Code
Sig[6].Value=c0000005
Sig[7].Name=Exception Offset
Sig[7].Value=0035c570
DynamicSig[1].Name=OS Version
DynamicSig[1].Value=10.0.19044.2.0.0.256.48
UI[2]=C:\Program Files (x86)\WinSCP\WinSCP.exe
LoadedModule[0]=C:\Program Files (x86)\WinSCP\WinSCP.exe
LoadedModule[1]=C:\Windows\SYSTEM32\ntdll.dll
LoadedModule[2]=C:\Windows\System32\KERNEL32.DLL
LoadedModule[3]=C:\Windows\System32\KERNELBASE.dll
LoadedModule[4]=C:\Windows\System32\WS2_32.DLL
LoadedModule[5]=C:\Windows\System32\RPCRT4.dll
LoadedModule[6]=C:\Windows\System32\CRYPT32.DLL
LoadedModule[7]=C:\Windows\System32\ucrtbase.dll
LoadedModule[8]=C:\Windows\System32\SHLWAPI.DLL
OsInfo[29].Key=osver
OsInfo[29].Value=10.0.19041.2604.amd64fre.vb_release.191206-1406
OsInfo[31].Key=edition
OsInfo[31].Value=Professional
FriendlyEventName=Stopped working
ConsentKey=APPCRASH
AppName=WinSCP: SFTP, FTP, WebDAV, S3 and SCP client
AppPath=C:\Program Files (x86)\WinSCP\WinSCP.exe
ApplicationIdentity=45B3D2BB4FABCE88A748DFBEF7254C79
```
<sup><sub>This example was produced on Windows 10, Version 10.0.19044 Build 19044</sub></sup>