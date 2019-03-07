---
layout: post
title: 워드프레스(wordpress) 설치 시 nginx와 php 설치 후 502 Bad Gateway 에러 해결방법
tags: wordpress nginx php
comments: true
---

## 설치
```
# nginx
sudo apt-get install nginx

# php-fpm
sudo apt-get install php-fpm
```

----

## 설정
nginx 설정은 /etc/nginx/sites-enabled/default 에 있다. default는 sites-availables의 default 파일의 심볼링 링크이며 자기가 원하는 파일을 지정해 사용하면 된다. 워드프레스를 위해 php 부분의 일부 주석을 해제하고 몇 가지 설정을 해준다.    
     
php 설정은 /etc/php/버전번호/fpm/pool.d/www.conf에 있다. 

----

## 502 Bad Gateway 에러
nginx의 설정에 php 소켓 파일 이름이 틀려서 그럴 가능성이 크다. 주석을 해제해주고 php7.2-fpm.sock 부분을 아래에 나오는 php 설정의 이름과 똑같이 맞춰줘야 한다. 

**nginx의 설정 일부**
```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html index.php;

    server_name YOUR-DOMAIN-WITHOUT-HTTP-OR-HTTPS;

    location / {
        try_files $uri $uri/ =404;
    }

    # 워드프레스에서 php를 쓰려면 여기 아래 부분의 주석을 해제
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
    }
    #######################################################
}
```    
    
**php의 설정 일부**
```
listen = /run/php/php7.2-fpm.sock
```
----

## 서비스 재시작
```
# nginx 재시작
sudo service nginx restart
    
# php 재시작
sudo service php7.2-fpm restart
```