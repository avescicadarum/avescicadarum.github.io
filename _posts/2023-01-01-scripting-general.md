---
layout: post
title:  "scripting - linux"
date:   2022-01-01
categories: web
---

## ping sweep

```bash
for i in {1..255} ;do (ping -c 1 10.11.1.$i | grep "bytes from"|cut -d ' ' -f4|tr -d ':' &);done > ips.txt
```

## extract javascript giles from a log file

```bash
cat access_log.txt | grep '[^.(]*\.js' | awk -F " " '{print $7}' | cut -f 3 -d "/" | sort | uniq -c| sort -nr
```

## zone transfer 

```bash
import dns.zone
import dns.resolver

ns_servers = []
def dns_zone_transfer(address):
    ns_answer = dns.resolver.resolve(address, 'NS')
    for server in ns_answer:
        print("[+] Found NS: {}".format(server))
        ip_answer = dns.resolver.resolve(server.target, 'A')
        for ip in ip_answer:
            print("[+] IP for {} is {}".format(server, ip))
            try:
                zone = dns.zone.from_xfr(dns.query.xfr(str(ip), address))
                for host in zone:
                    print("[+] Found Host: {}".format(host))
            except Exception as e:
                print("[+] NS {} refused zone transfer!".format(server))
                continue

dns_zone_transfer("example.com")
```

## scan specific port open in an ip range

```bash
#!/bin/bash
port=53
for ip in $(seq 1 255); do
  timeout 0.5 bash -c "</dev/tcp/10.10.10.$ip/$port" && echo "[+] 10.10.10.$ip port $port is open"
done 2>/dev/null
```

## enum smtp users

```bash
import socket
import sys

if len(sys.argv) != 2:
        print("Usage: vrfy.py <filename>")
        sys.exit(0)

# Read IPs from file
filename = sys.argv[1]
with open(filename) as users:
  for user in users:
    # Create a Socket
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # Connect to the Server
    connect = s.connect(('10.10.10.10',25))
    # Receive the banner
    banner = s.recv(1024)
    print(banner)
    # VRFY a user
    s.send(('VRFY ' + user.replace('\n', '') + '\r\n').encode())
    result = s.recv(1024)
    print(result)
    # Close the socket
    s.close()
```