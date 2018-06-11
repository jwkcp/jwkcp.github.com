---
layout: post
title: 우분투에서 mysqlclient 설치 시 에러가 발생하는 경우
tags: ubuntu mysql
comments: true
---
  
# 증상1 - egg_info 관련 에러
## 에러 메시지

~~~
Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-install-zbw18e9_/mysqlclient/
  
# 또는 
  
/bin/sh: 1: mysql_config: not found
~~~
    
## 원인 및 해결방법
libmysqlclient-dev 를 설치한다. 
  
~~~
sudo apt-get install libmysqlclient-dev
~~~
  
--- 
  
# 증상2 - my_bool 관련 에러
## 에러 메시지

~~~
error: use of undeclared identifier 'my_bool'
my_bool recon = reconnect;
^

error: use of undeclared identifier 'recon'
mysql_options(&self->connection, MYSQL_OPT_RECONNECT, &recon);
~~~
   
# 원인 및 해결방법
mysqlclient가 mysql 8.x 이상의 버전과 호환되지 않아서 발생한다. [여기](https://gist.github.com/vitorbritto/0555879fe4414d18569d)를 참고해 Mysql를 깨끗히 지우고 5.7버전을 설치한다. 아래 명령을 실행하면 stable 버전이 나온다. 5.7이면 brew install mysql 로 설치하면 된다.
   
~~~
brew info mysql 
~~~