---
layout: post
title: "apache批量虚拟主机"
date: 2021-12-13 00:54:40
---



```

UseCanonicalName Off
<VirtualHost *:80>
        VirtualDocumentRoot /www/web/%2/%1
</VirtualHost>


```