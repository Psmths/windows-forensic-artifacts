# SAM Hive
The SAM hive contains a wealth of information that can be used to profile an endpoint's accounts and collect data such as:

 - When an account was created
 - Account Last successful / failed login time
 - Account last password change time
 - Account group membership

For domain-joined endpoints, the SAM hive will be present on the domain controller. Non-domain joined endpoints will have a resident SAM hive. 

## Operating System Availability
 - Windows 10
 - Windows 8
 - Windows 7
 - Windows Vista
 - Windows XP

## Artifact Location(s)
- `C:\Windows\System32\config\SAM`

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

## Caveats
If the account in question is authenticating using Microsoft Live, the Logon Count will be 0.