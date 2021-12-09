---
layout: post
title: "Nginx 反代配置文件"
date: 2021-12-08 00:54:40
---



```

	server {
        listen 80;
        listen 443 ssl http2;
		server_name  ****.net; 
		return 301 https://www.****.net$request_uri;
        ssl_certificate     certfiles/****.net_cert.pem;
        ssl_certificate_key certfiles/****.net_cert.key;
		ssl_stapling on;
		ssl_stapling_verify on;
	}
	server {
        listen 80;
		server_name  www.****.net; 
		return 301 https://www.****.net$request_uri;
	}
	server {
        listen 443 ssl http2;
		server_name www.****.net; 
		
		# if ($geoip_country_code3 != "CHN") {
			# return 301 https://www.uselys.com;
		# }
		# if ($http_referer ~* 'baidu.com|sogou.com|bing.com|so.com') {
		# 	return https://www.****.com/member.php?mod=register;
		# }

        ssl_certificate     certfiles/****.net_cert.pem;
        ssl_certificate_key certfiles/****.net_cert.key;
		ssl_stapling on;
		ssl_stapling_verify on;
		
		client_max_body_size 200m;

		set $backend home.viewsrc.stream;
		
		location / {

			proxy_pass https://$backend;
			proxy_redirect https://$backend /;
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			#proxy_pass_header Set-Cookie;
			
			proxy_cache off;
						
			proxy_cache_bypass $cookie_1i2c_2132_auth $cookie_1i2c_2132_lip $cookie_1i2c_2132_nofavfid;
			proxy_no_cache $cookie_1i2c_2132_auth $cookie_1i2c_2132_lip $cookie_1i2c_2132_nofavfid;
			
			proxy_cache_valid  200 304 301 404 302 720d;

		}
		
		location ~* ^(.*)\.(css|js|ico|gif|jpg|jpeg|png)$ {

			proxy_pass https://$backend;
			proxy_redirect https://$backend /;
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			#proxy_pass_header Set-Cookie;
			
			proxy_cache off;
						
			proxy_cache_bypass $cookie_1i2c_2132_checkpm;

			proxy_cache_valid  200 304 301 404 302 720d;

		}
		location ~* ^/thread-(\d+)-(\d+)-(\d+)\.html$ {

			proxy_pass https://$backend;
			proxy_redirect https://$backend /;
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			#proxy_pass_header Set-Cookie;
			
			proxy_cache off;

			proxy_cache_bypass $cookie_1i2c_2132_checkpm;

			proxy_cache_valid  200 304 301 404 302 720d;

		}
		


		# limit_conn perip 15;
		# limit_conn perserver 256;
		# limit_conn_log_level  error;

		# limit_req zone=peripreq burst=90;
		# limit_req zone=perserverreq burst=960;
		# limit_req_log_level error;
		
		# limit_rate_after 512k;
		# limit_rate 25k;
		
	}

```