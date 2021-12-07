---
layout: post
title: "NGINX 缓存区分访客与已登录用户"
date: 2020-04-11 01:24:13
---

```
proxy_cache_bypass $cookie_d596ab410ed23232309f9f3602f2cbdd__typecho_authCode;
proxy_no_cache $cookie_d596ab410ed23232309f9f3602f2cbdd__typecho_authCode;
```

$cookie_ 后面的字符串就是登陆后的cookie名称。

https和http的Cookie名称不同。

如果开了cdn，cdn的缓存要清空。