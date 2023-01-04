---
layout: post
title:  "Linux Host  Discovery"
date:   2022-01-01
categories: linux
---


## Host Discovery

```bash

for i in {1..255} ;do (ping -c 10.10.1.$i | grep "bytes from"|cut -d ' ' -f4|tr -d ':' &);done

```