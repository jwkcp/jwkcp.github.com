---
layout: post
title: postgresql을 처음 시작할 때 필요한 간단 명령어
tags: postgresql db
comments: true
---
       
# 초기 설정
## postgresql 위치 확인
이 명령으로 경로가 나오지 않으면 [여기](http://postgresapp.com/documentation/cli-tools.html)를 참고하여 쉘에 path를 지정해주거나 .bash_profile 또는 zsh을 쓰고 있다면 .zshrc에 경로를 추가해줘야 한다. 프로파일을 즉시 반영하는 방법은 [여기](https://jwkcp.github.io/2018/05/24/zsh-profile/) 참고.  
~~~
which psql
~~~
  
## DB 연결
~~~
sudo -u postgres psql
~~~

## DB 연결 끊기, postgresql 쉘에서 빠져 나가기
~~~
\q
~~~
  
---
  
# 데이터베이스 다루기
## 데이터베이스 생성
~~~
create database 데이터베이스명;
~~~

## 데이터베이스 목록 조회
~~~
\list 또는 \l
~~~
  
## 데이터베이스 선택, 연결
~~~
\connect 데이터베이스명 또는 \c 데이터베이스명
~~~
  
---
  
# 사용자 다루기
## 사용자, 롤(role) 생성
~~~
create role 사용자명;

또는

create role 사용자명 password 'mypassword';
create role 사용자명 login password 'mypassword';
~~~

## 사용자, 롤(role) 비밀번호 변경
~~~
alter user 사용자명 with password 'mypassword';
~~~
  
## 사용자, 롤(role) 선택
~~~
set role 사용자명;
~~~
  
## 사용자, 롤(role) 권한주기
~~~
grant all privileges on database 데이터베이스명 to 사용자명;
~~~
  
---
  
# 기타
## 현재 연결 정보 보기
~~~
\conninfo
~~~
  
## 현재 사용자 조회
~~~
select current_user;
~~~
