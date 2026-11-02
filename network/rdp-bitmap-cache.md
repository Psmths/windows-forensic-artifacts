# RDP Bitmap Cache
The Remote Desktop Protocol (RDP) persistent bitmap cache is a client-side feature designed to reduce network bandwidth and improve performance by storing bitmap images received from a remote system. Rather than retransmitting graphical elements that have already been cached, the RDP client can reuse previously received bitmap data.

The cache is stored on the system initiating the RDP connection, within the profile of the user running the Remote Desktop client. It can contain small fragments of the remote desktop's graphical display, which may be extracted and examined to recover portions of what was displayed during RDP sessions.

As a forensic artifact, the persistent bitmap cache can provide visual evidence of remote interactive activity, potentially revealing applications, command prompts, file names, documents, administrative tools, and other information displayed on the remote system. However, it is not a continuous screen recording, and recovered fragments may be incomplete or difficult to associate with a particular session.

### Behavioral Indications
 - [x] Behavioral - Lateral Movement (TA0008)

### Analysis Value
 - [x] Network - Evidence of Network Activity

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista
 - [x] Windows XP

## Artifact Location(s)
 - `%UserProfile%\AppData\Local\Microsoft\Terminal Server Client\Cache` (Windows Vista and later)
 - `C:\Documents and Settings\{USERNAME}\Local Settings\Application Data\Microsoft\Terminal Server Client\Cache` (Windows XP)

## Artifact Parsers
 - [bmc-tools](https://github.com/ANSSI-FR/bmc-tools)
 - [RdpCacheStitcher](https://github.com/BSI-Bund/RdpCacheStitcher)

## Artifact Interpretation
The RDP bitmap cache directory typically contains two types of files:

- .bmc files — Legacy bitmap cache files, commonly named bcache*.bmc.
- .bin files — The newer persistent bitmap cache format, commonly named Cache0000.bin, Cache0001.bin, and so on.

These formats are associated with different generations of the RDP client. The file extension alone should not be used to determine the Windows version, as cache formats can vary with the RDP client implementation. The bmc-tools utility supports both formats and can extract their bitmap data.

The extracted data consists of individual bitmap fragments, typically small tiles representing portions of the remote desktop display. These fragments are not necessarily stored in screen-position order and do not inherently provide the coordinates required to reconstruct the original desktop. Consequently, reconstructing a coherent image may require manual examination and correlation. Tools such as RdpCacheStitcher can assist with this process, although automated reconstruction may produce imperfect results.

The presence of these cache files is evidence consistent with RDP client activity under the associated user profile. Because the cache is maintained on the client system, it can provide evidence of remote desktop activity initiated from that system. However, the files do not inherently identify the remote host, the user authenticated to that host, or the exact time of a particular session. Correlate the cache with other RDP artifacts to establish additional context.

In modern versions of Microsoft Remote Desktop Client, the `.bmc` file is created when and RDP connection is established, and the `.bin` files are created or modified when that RDP connection is closed. As such, the most recent modification timestamp among these files indicates the last time an RDP connection was established from the system, assuming that `Persistent Bitmap Caching` was enabled for that session. In the case that it was not enabled, neither the `.bmc` nor the `.bin` files will be created.

## Caveats
- **Caching can be disabled:** Persistent bitmap caching is configurable in the RDP client. If disabled, the client may not create or update persistent bitmap cache files. The absence of cache files does not establish that RDP was not used.
- **Incomplete visual evidence:** The cache contains graphical fragments rather than a complete recording. Not everything displayed during a session will necessarily be recoverable.
- **Session attribution:** The cache does not inherently identify the remote computer or associate individual bitmap fragments with a specific RDP session.
- **Temporal attribution:** Filesystem timestamps are not reliable substitutes for session start or end timestamps.
- **Cache reuse:** Bitmap fragments may persist and be reused across multiple RDP sessions, making it difficult to determine which session originally introduced a particular fragment.
