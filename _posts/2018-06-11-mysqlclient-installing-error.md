---
layout: post
title: 우분투에서 mysqlclient 설치 시 에러가 발생하는 경우
tags: ubuntu mysql
comments: true
---
  
## 증상
아래와 같은 에러가 발생하며 mysqlclient가 설치되지 않는다.  

~~~
Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-install-zbw18e9_/mysqlclient/
~~~
  
또는 
  
~~~
/bin/sh: 1: mysql_config: not found
~~~
  
---
   
## 해결방법
libmysqlclient-dev 를 설치한다. 
  
~~~
sudo apt-get install libmysqlclient-dev
~~~