---
layout: post
title:  "powershell - host discovery"
date:   2022-01-01
categories: windows
---

## Powershell - Host Discovery


```powershell

1..254 | ForEach-Object {Get-WmiObject Win32_PingStatus -Filter "Address='10.10.10.$_' and Timeout=200 and ResolveAddressNames='true' and StatusCode=0" | select ProtocolAddress*}
```