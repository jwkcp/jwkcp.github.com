---
layout: post
title: 우분투(ubuntu)에서 systemd 간단 사용법
tags: ubuntu systemd
comments: true
---
  
# 개요
[위키피디아의 systemd 설명](https://ko.wikipedia.org/wiki/Systemd)에 따르면 systemd는 일부 리눅스 배포판에서 프로세스를 관리하는 init 프로세스라고 한다. (init 프로세스는 시스템 부팅 과정의 최초 프로세스이자 시스템이 종료될 때 까지 계속 실행되며 모든 프로세스의 부모 노릇을 하는 프로세스다.)   
  
systemd 대표적인 역할은 어떤 서비스를 띄웠을 때 알 수 없는 이유로 프로세스가 죽을 수가 있는데 systemd는 자동으로 프로세스를 재시작하는 등 이런 사고(?)로 부터 프로세스를 관리해준다고 보면 된다. upstart, supervisor 등이 이와 유사한 역할을 한다. 우분투(ubuntu)의 경우 과거 upstart를 사용했으나 데비안 리눅스 배포판 init 프로그램 변경 투표에서 systemd가 최종 선택되며 upstart가 아닌 systemd가 공식적으로 사용되고 있다. 경우에 따라 다르겠지만 처음 시작하며 특별한 이유가 없다면 supervisor 보다는 systemd를 추천하는 글을 꽤 많이 볼 수 있다.   
  
아래는 우분투(ubuntu)를 기준으로 설명한다.  
  
---

# 기본 구조
## 파일 위치
~~~
/etc/systemd/system/나의서비스명.service
~~~
  
## 파일 구성
~~~
[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=myuser
Group=mygroup
EnvironmentFile=/home/myuser/env/systemd_env

# 또는
# Environment=MYVAL="myvalresult"

WorkingDirectory=/home/myuser/myapp
ExecStart=/home/myuser/bin/gunicorn_start
Restart=always

[Install]
WantedBy=multi-user.target
~~~
  
## 알아두기
다른 프로그램도 마찬가지이겠지만 Django를 사용할 경우 SECRET_KEY나 데이터베이스 연결정보, DJANGO_SETTINGS_MODULE, DJANGO_WSGI_MODULE 등의 값을 환경변수에서 불러다 쓰도록 구현할 때가 있다. 이때 .bashrc 등에 export ... 식으로 환경변수를 입력해두고 이를 불러서 쓴다. 하지만 쉘에서 실행할 때는 정상적으로 환경변수를 불러왔더라도 systemd에서 이 환경변수를 제대로 불러오지 못해 문제가 생기는 일을 경험했다면 위 파일 구성에서 **Environment**나 **EnvironmentFile**를 이용하면 된다. 환경변수가 몇 개 되지 않는다면 **Environment**를 이용해도 될 것이고, 환경변수가 많아 별도로 깔끔하게 관리하고 싶다면 **EnvironmentFile**을 이용해보자. **EnvironmentFile**에서 불러오는 파일은 아래와 같이 키-값의 형태로 각 항목은 개행으로 구분해주면 된다.  
  
*예시*
~~~
SECRET_KEY="23j09jf09asdjf923789249fhnakfjaklsdfhas"
DB_NAME=mydb
DB_USER=myuser
~~~
  
---

## 모든 서비스 조회하기
~~~
sudo systemctl list-units

# 또는 상태별 조회하기
# 서비스 상태(failed, running, exited, plugged)
  
sudo systemctl list-units --state=failed
~~~

---

## 활성화 및 시작
~~~
sudo systemctl enable 내서비스이름
sudo systemctl start 내서비스이름
~~~

---

## 종료 및 비활성화
~~~
sudo systemctl stop 내서비스이름
sudo systemctl disable 내서비스이름
~~~

---

## 불필요한 서비스 삭제
~~~
sudo systemctl reset-failed
~~~

---

## 자세한 오류 및 기록 확인
~~~
sudo journalctl | grep 내서비스이름
~~~