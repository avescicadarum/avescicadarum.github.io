---
layout: post
title:  "active directory - windows - bloodhound"
date:   2022-01-01
categories: windows
---


## AD - Bloodhound


### Target


```powershell

PS C:\Users\user\Documents> upload /home/kali/privesc/SharpHound.ps1

PS C:\Users\user\Documents> .\SharpHound.ps1

PS C:\Users\user\Documents> Invoke-BloodHound -CollectionMethod All

or 

PS C:\Users\user\Documents> Invoke-BloodHound -CollectionMethod All -Domain DC0.EXAMPLE.LOCAL -ZipFilename dc.zip

```

### Kali


```bash

sudo neo4j console
bloodhound
```