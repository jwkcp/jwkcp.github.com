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
# 쉘 프롬프트에서 실행
sudo -u postgres createdb mydbname

또는

# psql 접속 후 실행
create database mydbname;
~~~

## 데이터베이스 목록 조회
~~~
# psql 접속 후 실행
\list 또는 \l
~~~
  
## 데이터베이스 선택, 연결
~~~
# psql 접속 후 실행
\connect 데이터베이스명 또는 \c 데이터베이스명
~~~
  
---
  
# 사용자 다루기
## 사용자 생성
~~~
# psql 접속 후 실행
create user 사용자명 password 'mypassword';
~~~

## 사용자 롤(role) 또는 비밀번호 변경
~~~
# psql 접속 후 실행
alter user 사용자명 with password 'mypassword';
alter user 사용자명 with superuser;
alter user 사용자명 with createrole;
~~~
    
## 사용자 권한주기
~~~
# psql 접속 후 실행
grant all privileges on database 데이터베이스명 to 사용자명;
~~~
  
## 현재 사용자 조회
~~~
# psql 접속 후 실행
select current_user;
~~~
  
## 모든 사용자 조회
~~~
# psql 접속 후 실행
\du 또는 \du+
~~~
    
---
  
# 기타
## 현재 연결 정보 보기
~~~
# psql 접속 후 실행
\conninfo
~~~
  
## 로케일, 인코딩(Encoding) (한글 깨짐 해결하기)
**기본 인코딩 설정 위치**
~~~
sudo vi /etc/postgresql/9.5/main

에서 client_encoding = 'ko_KR.UTF-8'
lc_messages = 'ko_KR.UTF-8'
lc_monetary = 'ko_KR.UTF-8'
lc_numeric = 'ko_KR.UTF-8'
lc_time = 'ko_KR.UTF-8'
~~~
  
## Collate, Ctype 변경
> 필요한 경우에만 변경하세요. 경우에 따라서 sudo -u postgres psql 을 통해 로그인 시 아래와 같은 메시지의 에러가 발생할 수 있습니다.
  
> psql: FATAL:  database locale is incompatible with operating system
DETAIL:  The database was initialized with LC_COLLATE "ko_KR.UTF-8",  which is not recognized by setlocale().
HINT:  Recreate the database with another locale or install the missing locale.

**Collate, Ctype 변경하기**
~~~
update pg_database
set datcollate='ko_KR.UTF-8', datctype='ko_KR.UTF-8'
where datname=바꾸려는 데이터베이스 이름;
~~~
