---
layout: post
title: "Nginx 基本配置文件 Debian适用"
date: 2021-12-09 10:53:40
---



```

user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;
#worker_shutdown_timeout 300s;

events {
	worker_connections 4096;
	multi_accept on;
	use epoll;
	worker_aio_requests 8192;
}


http {

	##
	# Basic Settings
	##
	
    include       mime.types;
    default_type  application/octet-stream;

	sendfile on;
	tcp_nopush on;
	tcp_nodelay off;
	keepalive_timeout 75;
	keepalive_requests 100; 
	types_hash_max_size 2048;
	server_tokens off;
	
	server_names_hash_bucket_size 512;
	server_name_in_redirect on;

	resolver 1.1.1.1 1.0.0.1 valid=60s ipv6=off;
	
    map $http_upgrade $connection_upgrade {
        default upgrade;
        ''      close;
    }
	
	##
	# Set FireWall
	##
	
	limit_conn_zone $binary_remote_addr zone=perip:12m;
	limit_conn_zone $server_name zone=perserver:12m;
	limit_req_zone $binary_remote_addr zone=peripreq:12m rate=5r/s;
	limit_req_zone $server_name zone=perserverreq:12m rate=5120r/s;

	##
	# Set GEO
	##
	
	geoip_country         /usr/share/GeoIP/GeoIP.dat;
	geoip_city            /usr/share/GeoIP/GeoIPCity.dat;

	##
	# Cloudflare IP list
	##
	set_real_ip_from 103.21.244.0/22;
	set_real_ip_from 103.22.200.0/22;
	set_real_ip_from 103.31.4.0/22;
	set_real_ip_from 104.16.0.0/12;
	set_real_ip_from 108.162.192.0/18;
	set_real_ip_from 131.0.72.0/22;
	set_real_ip_from 141.101.64.0/18;
	set_real_ip_from 162.158.0.0/15;
	set_real_ip_from 172.64.0.0/13;
	set_real_ip_from 173.245.48.0/20;
	set_real_ip_from 188.114.96.0/20;
	set_real_ip_from 190.93.240.0/20;
	set_real_ip_from 197.234.240.0/22;
	set_real_ip_from 198.41.128.0/17;
	set_real_ip_from 2400:cb00::/32;
	set_real_ip_from 2606:4700::/32;
	set_real_ip_from 2803:f800::/32;
	set_real_ip_from 2405:b500::/32;
	set_real_ip_from 2405:8100::/32;
	set_real_ip_from 2c0f:f248::/32;
	set_real_ip_from 2a06:98c0::/29;
	set_real_ip_from 144.34.170.221;

	# use any of the following two
	#real_ip_header CF-Connecting-IP;
	real_ip_header X-Forwarded-For;
	

	##
	# Proxy Settings
	##
	
    proxy_cache_path /var/cache/nginx/ levels=1:2 keys_zone=uselys:60m inactive=720d max_size=16G;
	proxy_cache_key $scheme://$host$uri$is_args$args;
	
	proxy_buffering on;
	proxy_buffer_size 512k;
	proxy_buffers 32 512k;
	proxy_busy_buffers_size 512k;
	proxy_request_buffering on;

	proxy_connect_timeout 120s;
	proxy_read_timeout 120s;
	proxy_send_timeout 120s;
	
	proxy_set_header X-Real-IP $remote_addr;
	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	proxy_set_header Accept-Encoding "";
	
	proxy_intercept_errors off;
	proxy_ignore_headers X-Accel-Expires Expires Cache-Control Set-Cookie Vary;
	
	# proxy_hide_header Cache-Control;
	# proxy_hide_header Set-Cookie;
	# proxy_hide_header Expires;
	# proxy_hide_header X-Accel-Expires;
	# proxy_hide_header Vary;

	proxy_cache_lock on;
	proxy_cache_lock_timeout 60s;
	proxy_cache_lock_age 300s;
	proxy_cache_use_stale updating error timeout invalid_header http_404 http_500 http_502 http_503 http_504;
	proxy_cache_revalidate on;
	proxy_cache_valid  200 304 301 404 302 2h;
	proxy_cache_valid any 30m;
	proxy_cache_min_uses 1;
	
	expires 15m;
	
	proxy_pass_header Set-Cookie;

	##
	# SSL Settings
	##

	ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
	ssl_prefer_server_ciphers on;

	##
	# Logging Settings
	##

	log_format main '[$time_local] - $status - $remote_addr - $http_host "$request" "$http_referer" "$http_user_agent" "$http_x_forwarded_for"';
	
    log_format  uselys  '$remote_addr - $remote_user [$time_local] $http_host "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
	map $http_user_agent $google {
		default 0;
		~*Googlebot   1;
	}
	map $http_user_agent $baidu {
		default 0;
		~*BaiduSpider   1;
	}

	access_log  /root/baidu.txt  main if=$baidu;
	access_log  /root/google.txt  main if=$google;
	
	#access_log off;
	error_log /dev/null crit;
	access_log /var/log/nginx/access.log main;
	error_log /var/log/nginx/error.log;

	##
	# Gzip Settings
	##

	gzip off;
	gzip_disable "msie6";

	# gzip_vary on;
	# gzip_proxied any;
	# gzip_comp_level 6;
	# gzip_buffers 16 8k;
	# gzip_http_version 1.1;
	# gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

	##
	# Virtual Host Configs
	##


    server {
        listen       80;
		listen       443 ssl http2;
        server_name  localhost;
        ssl_certificate     certfiles/viewsrc.stream_cert.pem;
        ssl_certificate_key certfiles/viewsrc.stream_cert.key;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

		if ($http_user_agent ~* "python") { return 444; }

        error_page  404              /404.html;
		location = /404.html { alias /etc/nginx/incfiles/404.html; }

        location / {
		
			if (!-e $request_filename) {
				return 404;
			}
			
            root   /var/www/robots/;
            index  index.html index.htm;

        }
		
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
		


        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }

	include sites-enabled/*.conf;

    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}


```