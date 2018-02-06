---
layout: post
title: 도커(docker)에 mysql 설치 후 콘솔에서 mysql 연결이 안될 때
tags: how-to, docker, mysql
comments: true
---
  
### 증상
도커(docker)에 mysql 설치 후 콘솔에서 mysql을 연결하려고 하면 'Access Denied' 메시지가 발생하면서 연결되지 않음
  
### 원인 및 체크사항
여러 가지 원인이 있을 수 있지만 아래 사항을 우선 확인해보자. 
  
***
  
1. 도커에서 mysql을 서비스하고 있는 포트 확인
   보통 mysql은 3306 포트로 서비스하지만 도커에서 서비스하는 포트는 다를 수 있다. 도커에서 **'Published IP:PORT'**라고 되어 있는 부분의 포트를 사용하면 된다.
  
2. localhost말고 127.0.0.1 사용
   '''mysql -h localhost -P 32770 -u root -p''' 라고 콘솔에서 실행하면 'Access Denied' 에러가 나오는데 localhost 대신 127.0.0.1를 입력하면 정상적으로 mysql에 접속이 된다.
   
   