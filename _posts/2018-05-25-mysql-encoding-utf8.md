---
layout: post
title: 우분투에서 mysql 인코딩을 latin1에서 utf8로 바꿔주기
tags: mysql db
comments: true
---
       
## 초기 접속하기
~~~
mysql -u root -p [엔터]

만약 안된다면
 
sudo mysql -u root -p [엔터]
~~~
  
초기 접속이 안된다면 mysql이 동작 중(active)인지 확인하자.  
> sudo systemctl status mysqld
  
---  
  
## 인코딩 상태 확인하기
mysql 접속 후 mysql> 프롬프트 상에서 아래 명령을 입력한다.  
~~~
status
~~~

---
  
## 변경하기
/etc/mysql/my.cnf의 하단에 아래 내용을 붙여넣는다.  
  
~~~
[client]
default-character-set=utf8

[mysql]
default-character-set=utf8


[mysqld]
collation-server = utf8_unicode_ci
init-connect='SET NAMES utf8'
character-set-server = utf8
~~~
  
---
  
## 반영하기
아래 명령으로 mysql을 재시작해준다.
  
~~~
sudo systemctl restart mysqld
~~~
