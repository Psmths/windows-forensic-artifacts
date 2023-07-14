# WordWheelQuery Registry Key
The WordWheelQuery registry key contains values that represent the history of searches performed using the Explorer search bar.

### Analysis Value
 - [x] User Activity

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7

## Artifact Location(s)
🔋 Live System:
- `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery`

🔌 Offline system:
- File: `%UserProfile%\NTUSER.DAT`
- Key: `SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
Within this registry key will be values with numerical names and an additional value `MRUListEx`. Each numerically named value represents one search term, and the `MRUListEx` value indicates the order in which the terms were searched. 

The Last Write Timestamp of the `WordWheelQuery` registry key indicates the time at which the most recent search term was used to perform a search within Explorer. 

## Example
Within the `WordWheelQuery` registry key, the data of the value `MRUListEx` is as follows:

```
00000000  3f 00 00 00  |?...|
00000004  3e 00 00 00  |>...|
00000008  3d 00 00 00  |=...|
0000000c  3c 00 00 00  |<...|
00000010  3b 00 00 00  |;...|
00000014  3a 00 00 00  |:...|
00000018  39 00 00 00  |9...|
0000001c  38 00 00 00  |8...|
00000020  37 00 00 00  |7...|
00000024  31 00 00 00  |1...|
00000028  36 00 00 00  |6...|
0000002c  35 00 00 00  |5...|
00000030  2f 00 00 00  |/...|
00000034  34 00 00 00  |4...|
00000038  33 00 00 00  |3...|
0000003c  32 00 00 00  |2...|
00000040  30 00 00 00  |0...|
00000044  2e 00 00 00  |....|
00000048  2d 00 00 00  |-...|
0000004c  2c 00 00 00  |,...|
00000050  2b 00 00 00  |+...|
00000054  2a 00 00 00  |*...|
00000058  29 00 00 00  |)...|
0000005c  28 00 00 00  |(...|
00000060  27 00 00 00  |'...|
00000064  26 00 00 00  |&...|
00000068  25 00 00 00  |%...|
0000006c  24 00 00 00  |$...|
00000070  23 00 00 00  |#...|
00000074  22 00 00 00  |"...|
00000078  21 00 00 00  |!...|
0000007c  20 00 00 00  | ...|
00000080  1f 00 00 00  |....|
00000084  1e 00 00 00  |....|
00000088  1d 00 00 00  |....|
0000008c  1c 00 00 00  |....|
00000090  1b 00 00 00  |....|
00000094  1a 00 00 00  |....|
00000098  19 00 00 00  |....|
0000009c  18 00 00 00  |....|
000000a0  17 00 00 00  |....|
000000a4  16 00 00 00  |....|
000000a8  15 00 00 00  |....|
000000ac  13 00 00 00  |....|
000000b0  14 00 00 00  |....|
000000b4  12 00 00 00  |....|
000000b8  11 00 00 00  |....|
000000bc  10 00 00 00  |....|
000000c0  0f 00 00 00  |....|
000000c4  0e 00 00 00  |....|
000000c8  0d 00 00 00  |....|
000000cc  0c 00 00 00  |....|
000000d0  0b 00 00 00  |....|
000000d4  05 00 00 00  |....|
000000d8  0a 00 00 00  |....|
000000dc  09 00 00 00  |....|
000000e0  00 00 00 00  |....|
000000e4  08 00 00 00  |....|
000000e8  07 00 00 00  |....|
000000ec  06 00 00 00  |....|
000000f0  03 00 00 00  |....|
000000f4  04 00 00 00  |....|
000000f8  02 00 00 00  |....|
000000fc  01 00 00 00  |....|
00000100  ff ff ff ff  |ÿÿÿÿ|
```

From this information, we can determine that the most recently searched term exists in the value named `3f 00 00 00`, or `63` in decimal. The data of this value is `74 00 65 00 73 00 74 00 00 00`, which when translated from UTF-16LE is `test`. Because this is the most recent term as indicated by the `MRUListEx` value, the Last Write Timestamp will correspond to the time that this term was searched.

The earliest known search term is in the value named `01 00 00 00`, or `1`.