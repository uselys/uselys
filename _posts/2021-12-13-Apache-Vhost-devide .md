---
layout: post
title: "apache单个虚拟主机划分为多个"
date: 2021-12-13 00:54:40
---



```

RewriteEngine on

DirectoryIndex index.html index.htm index.php default.php index.cgi

RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d

RewriteCond %{HTTP_HOST} www.take168.com
RewriteCond %{REQUEST_URI} !^/take168/www/
RewriteRule ^(.*)$ /take168/www/$1
RewriteCond %{HTTP_HOST} www.take168.com
RewriteRule ^(/)?$ take168/www/index.php [L]

RewriteCond %{HTTP_HOST} ^www.ideatip.com$
RewriteCond %{REQUEST_URI} !^/ideatip/www/
RewriteRule ^(.*)$ /ideatipbbs/$1
RewriteCond %{HTTP_HOST} ^www.ideatip.com$
RewriteRule ^(/)?$ ideatip/www/index.php [L]
 
RewriteCond %{HTTP_HOST} ^www.jobnh.cn$
RewriteCond %{REQUEST_URI} !^/jobnh/www/
RewriteRule ^(.*)$ /jobnh/www/$1
RewriteCond %{HTTP_HOST} ^www.jobnh.cn$
RewriteRule ^(/)?$ jobnh/www/ [L]


```