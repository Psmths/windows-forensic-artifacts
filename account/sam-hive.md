# SAM Hive
The SAM hive contains a wealth of information that can be used to profile an endpoint's accounts. For domain-joined endpoints, the SAM hive will be present on the domain controller. Non-domain joined endpoints will have a resident SAM hive. 

### Behavioral Indications
 - [x] Behavioral - Persistence (TA0003)

### Analysis Value
 - [x] Account - Creation Time
 - [x] Account - Group Membership
 - [x] Account - Last Login
 - [x] Account - Relative Identifier (RID)

## Operating System Availability
 - [x] Windows 11
 - [x] Windows 10
 - [x] Windows 8
 - [x] Windows 7
 - [x] Windows Vista
 - [x] Windows XP

## Artifact Location(s)
- File: `%SystemRoot%\System32\config\SAM`

## Artifact Parsers
 - RegistryExplorer (Eric Zimmerman)

## Artifact Interpretation
Within the SAM hive, the registry key located at `SAM\Domains\Accounts\Users` will contain the following values for each account:

 - Relative Identifier (RID)
 - CreatedOn time (Time the account was created)
 - Logon Count
 - Username
 - Password reset questions
 - Password Hints
 - Last Login Time
 - Last Failed Login Time
 - Last Password Change Time

> [!NOTE]
> If the account in question is authenticating using Microsoft Live, the Logon Count will be 0.