---
layout: post
title: Django와 함께 Mysql 사용 준비하기
tags: mysql
comments: true
---
  
## mysql 다운로드
- [공식 다운로드 사이트](https://dev.mysql.com/downloads/)
- [공식 다운로드 사이트 - 커뮤니티 버전 5.7](https://dev.mysql.com/downloads/mysql/5.7.html#downloads)
  
> **알아두기**  
2018.05.23 현재 Django에서 mysql을 쓰기 위해 사용하는 mysqlclient 패키지가 mysql 8.0 버전에서는 'my_bool' 에러가 발생하면 정상적으로 동작하지 않는다. 따라서 이 오류가 발생하고 mysqlclient 라이브러리도 mysql 8.0에 대응하지 않고 있다면 8.0 이상이 아닌 5.7 이하를 설치해야 이 오류를 피할 수 있다.

---
  
## 데이터베이스 생성
~~~
create database testdb;
~~~
  
---
  
## 데이터베이스 접속
~~~
mysql -u root -p
~~~
  
---
  
## 사용자 생성
~~~
create user 'testuser'@'localhost' identified by 'mypassword';
~~~
  
## 사용자 보기
~~~
select user, host from user;
~~~
  
## 사용자 삭제
~~~
drop user 'testuser'@'localhost';
~~~
  
---
  
## 권한 부여
~~~
grant all privileges on testdb.* to 'testuser'@'localhost';
~~~
  
## 권한 보기
~~~
show grants for 'testuser'@'localhost';
~~~
   
---
  
## 인코딩 설정 확인
~~~
show variables like 'char%';
  
or
  
status
~~~
  
---
  
## 인코딩 변경
~~~
set character_set_client = utf8;
~~~
  
> **일러두기**  
Django에서 *python manage.py makemigrations* 명령을 실행하기 전에 인코딩을 변경해야 한다. 그렇지 않으면 한글 등 아스키 문자가 아닌 문자가 입력될 경우 Django 에러가 발생할 수 있다.  
