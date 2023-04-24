# Program Compatibility Assistant (PCA)
Program Compatibility Assistant (PCA) is a feature introduced since Windows 8 intended for application compatibility purposes. New to Windows 11 (22H2 Update) is the ability to extract detailed evidence of execution from it. 

### Behavioral Indications
 - [x] Behavioral - Execution (TA0002)

### Analysis Value
 - [x] Execution - Evidence of Execution
 - [x] Execution - Last Executed
 - [x] File - Path

## Operating System Availability
 - [x] Windows 11

## Artifact Location(s)
- `%SystemRoot%\AppCompat\pca\PcaAppLaunchDic.txt`

## Artifact Interpretation - PcaAppLaunchDic.txt
This is a pipe-delimited text file that contains the following information:

`{Executable Full Path}|{Most Recent Execution Timestamp}`
