---
layout: post
title: 10분만에 익히는 supervisor 설치와 사용법
tags: supervisor usage
comments: true
---
우분투 환경에서 supervisor를 처음 사용해봤던 경험을 토대로 설치 및 사용법에 대해 간략히 남겨보고자 한다. supervisor가 무엇인지, 어디서부터 시작해야하는지 찾고 있는 분이 있다면 분명 도움이 될 것이다. 이 글은 supervisor의 모든 것을 다루고 있지 않으며 일정 부분의 오류와 지식의 한계를 포함하고 있다. 새로 알게되거나 발견된 오류는 지속적으로 이 글에 업데이트 될 예정이다.

---

## 정의

[supervisord.org](http://supervisord.org/, "supervisord.org")에서는 supervisor를 이렇게 정의하고 있다.

> “유닉스 계열의 시스템에서 동작하는 프로세스를 모니터링하고 관리하기 위한 클라이언트/서버 프로그램”

서비스를 운영하다 보면 프로세스가 잘 돌고 있는지, 죽지 않았는지 계속해서 신경써야 한다. 죽은 프로세스를 언제나 빠르게 재구동하는 것도 쉬운 일은 아니다. 프로세스는 24시간 일을 하지만 관리자가 항상 시스템에 접속할 수 있진 않으니까. supervisor는 이런 어려움을 해결하고자 태어났다. 손쉽게 프로세스 상태를 보여주고, 죽은 프로세스도 자동으로 살려준다.

## 용어

* supervisor: 이 제품의 이름이다.
* supervisor**d**: supervisor 백그라운드 데몬 프로세스
* supervisor**ctl**: supervisor로 구동되는 프로세스를 관리하기 위한 명령어

## 설치

apt-get과 pip, 2가지 방법을 통해 설치할 수 있다. service 명령을 통해 구동할 수 있을 뿐만 아니라 가상환경의 pip를 들락 날락하고 싶지 않아서다. 두 가지 방법 모두 사용해봤는데 개인적으로 apt-get을 통해 설치하는 것을 선호한다.

1. apt-get으로 설치: apt-get install supervisor
2. pip로 설치: pip install supervisor

## 설정

/etc/supervisor 디렉토리 아래 supervisord.conf 설정 파일을 찾는다. 아래는 gunicorn을 관리하는 프로세스로 지정한 경우인데 [program:프로그램명] 으로 기재 후 실행 스크립트 경로를 아래 command 속성에 넣어준다.

{% highlight bash shell scripts %}
[program:myapp]
command=/home/user/gunicorn_start.sh
{% endhighlight %}

잠시 gunicorn 실행 스크립트를 살펴보자. 아래와 같이 작성했다. 이 방법은 정석이 아니며, 더 좋은 방법이 있을 수 있다. 이 글은 supervisor를 설명하는 글이므로 다른 곳에 시간을 들이지 않았으면 하는 마음에 덧붙여 보았다.

{% highlight bash shell scripts %}
#!/bin/bash

source /home/user/.virtualenvs/my_venv/bin/activate     ; 파이썬 가상환경 활성화
cd /home/user/myapp     ; 프로그램 디렉터리로 이동
exec /home/user/.virtualenvs/my_venv/bin/gunicorn -b 0.0.0.0:8081 myapp.wsgi:application     ; 가상환경에 설치된 gunicorn 실행
{% endhighlight %}

## 구동

1. apt-get 명령으로 설치했을 경우: service supervisor restart
2. pip로 설치했을 경우: supervisord -c '설정파일위치'

## 관리

앞서 보았던 /etc/supervisor 디렉토리 하위에 supervisord.conf 설정 파일 중 아래 항목을 주석 해제 후 적절한 포트와 아이피, 접속 허용할 계정정보를 입력한다. 그러면 웹으로도 프로세스를 관리할 수 있다.

{% highlight bash shell scripts %}
[inet_http_server] ; inet (TCP) server disabled by default
port=0.0.0.0:9001
username=admin
password=1234
{% endhighlight %}

[![supervisor_screenshot.png](https://s26.postimg.org/o7pdsrr09/supervisor_screenshot.png)](https://postimg.org/image/tvvojnvcl/)

위에 보이는 restart, stop 등의 명령은 쉘에서 supervisorctl로도 똑같이 할 수 있다.

## 요구사항

**지원**: 리눅스, 맥os, 솔라리스, FreeBSD 등 대부분의 유닉스 기반 운영체제, 파이썬 2.4 ~ 2.7
**미지원**: 모든 윈도우 버전, 파이썬 3 ~

현재 supervisor 3.3까지 릴리즈 되었으며 python3를 지원하지 않는다. 여기를 방문하면 supervisor의 github를 통해 개발 진행 상황을 볼 수 있다. master 브랜치의 readme.rst를 보면 master 브랜치를 통해 현재 python3를 지원하기 위한 개발을 진행하고 있는 것을 알 수 있다. (2016년 11월 현재)

하지만 우분투에 python2.7이 깔려있고 apt-get을 통해 supervisor를 설치하면 문제없이 사용할 수 있다. 프로그램은 어차피 가상환경을 통해 python3로 개발할 수 있으니까.

---

## 더 볼만한 글
당연히 [supervisord.org](http://supervisord.org/ "supervisord.org")에 훨씬 방대한 내용의 정보가 있다. [DigitalOcean같은 사이트](https://www.digitalocean.com/community/tutorials/how-to-install-and-manage-supervisor-on-ubuntu-and-debian-vps "DigitalOcean같은 사이트")에서도 supervisor 사용법에 대해 다루고 있으니 참고해보면 좋겠다. supervisor 말고도 훌륭한 프로세스 모니터링 기능을 제공하는 [systemd](https://www.freedesktop.org/wiki/Software/systemd/ "systemd"), [circus](http://circus.readthedocs.io/en/latest/ "circus"), [upstart](http://upstart.ubuntu.com/ "upstart"), [runit](http://smarden.org/runit/ "runit"), [god](http://godrb.com/ "god") 등도 있다.
