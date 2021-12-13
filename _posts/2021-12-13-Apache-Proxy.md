---
layout: post
title: "apache反向代理"
date: 2021-12-13 00:54:40
---



```

<VirtualHost *:80>
    ServerName www.ylqx.info
        DocumentRoot /www/html/ylqx/www
        <IfModule mod_proxy.c>
                ProxyRequests Off
                <Proxy *>
                        Order deny,allow
                        Deny from all
                        Allow from all
                </Proxy>
                ProxyPass /1069/ http://www.so1069.com/
                ProxyPassReverse /1069/ http://www.so1069.com/
                ProxyVia Full
                <IfModule mod_disk_cache.c>
                        CacheEnable disk /
                        CacheRoot "/var/cache/mod_proxy"
                        CacheMaxFileSize 20000
                        CacheMinFileSize 64
                </IfModule>
        </IfModule>
        ErrorDocument 404 /index.html
</VirtualHost>

```