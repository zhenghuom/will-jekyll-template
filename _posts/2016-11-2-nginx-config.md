---
layout: post
title: "nginx php7 配置"
date: 2016-11-1 18:56:00
image: '/assets/img/'
description: 'ajax上传文件'
tags:
- jekyll 
- template 
categories:
- I love Jekyll
---

#nginx php7 配置Thinkphp

```host
server {
        listen 80;
        listen [::]:80;

        server_name azooo.localhost.com;

        root /home/zhm/www/azooo;

        index index.php index.html index.htm index.nginx-debian.html;
        if (!-e $request_filename) {
                 rewrite ^/index.php(.*)$ /index.php?s=$1 last;
                 rewrite ^(.*)$ /index.php?s=$1 last;
                break;
         }

        location / {
                try_files $uri $uri/ =404;
        }

        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/run/php/php7.0-fpm.sock;
        }

        location ~ /\.ht {
                deny all;
        }
}

```
#nginx php7 配置Laravel

```host
server {
        listen 80;
        listen [::]:80;

        server_name major.localhost.com;

        root /home/zhm/www/azooo-major/public;


        location / {
                try_files $uri $uri/ /index.php?$query_string;
                index index.php  index.html index.htm;

        }

        location = /50x.html {
            root   html;
        }

        location ~ \.php$ {
                #include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/run/php/php7.0-fpm.sock;
                fastcgi_index  index.php;
                fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
                include        fastcgi_params;
        }
}
```


