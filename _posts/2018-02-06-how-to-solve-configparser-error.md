---
layout: post
title: MySQL-python을 설치할 때 ConfigParser 관련 에러가 발생하는 경우
tags: how-to, django, python
comments: true
---
  
### 증상
'''pip install MySQL-python''' 을 실행했을 때 **ModuleNotFoundError: No module named 'ConfigParser'** 라는 에러 메시지가 나오면서 설치가 안되는 경우  
  
### 원인
MySQL-python 라이브러리가 python3를 지원하지 않기 때문이다.  
  
### 해결방법
MySQL-python 대신 mysqlclient를 사용하면 된다.  
  
  