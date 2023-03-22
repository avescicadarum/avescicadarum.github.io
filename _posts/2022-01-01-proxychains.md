---
layout: post
title:  "networking - linux - proxychains"
date:   2022-01-01
categories: networking
---


### Configure Proxychains

```bash

┌──(kali㉿Zeus)-[~]
└─$ cat /etc/proxychains.conf | grep 9050
socks5 <your VPN IP> 9050

```

### Proxychains through SSH connection


```bash

ssh -D <your VPN IP>:9050 -i id_rsa root@10.10.10.10

```


### Directory Scan through proxy


```bash

gobuster dir --proxy socks5://<your VPN IP>:9050 --url http://<ip> -w /usr/share/wordlists/dirb/common.txt

```


### Resources

```

https://abrictosecurity.com/how-to-use-proxychains/

```