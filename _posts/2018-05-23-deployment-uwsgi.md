---
layout: post
title: Django(장고) 배포 가이드(번역) - 7. uWSGI 사용법
tags: django deployment
comments: true
---
  
> 이 글은 장고(Django) 공식 페이지의 글을 번역한 것입니다. 원문은 [여기](https://docs.djangoproject.com/en/2.0/howto/deployment/wsgi/uwsgi/)를 참고하세요.  
    
[uWSGI](https://uwsgi-docs.readthedocs.io/)는 순수 C코드로 작성되었고 자가 복구 기능이 있으며 개발자/시스템 관리자가 다루기 쉽게 되어 있는 어플리케이션 컨테이너 서버입니다.  
  
> **잠깐**
> uWSGI 문서는 Django, nginx 및 uWSGI(여러 가지 배포 설정 중 하나)를 다루는 [문서](https://uwsgi.readthedocs.io/en/latest/tutorials/Django_and_nginx.html)를 제공합니다. 이 문서는 Django와 uWSGI를 통합하는 방법에 초점을 맞추고 있습니다.  

## 전제조건: uWSGI  
uWSGI의 위키 페이지는 여러 가지 설치 과정을 보여줍니다. 파이썬 패키지 매니저인 pip를 사용하여 단 한 줄의 명령으로 어떤 버전의 uWSGI라도 설치할 수 있습니다. 예를 들어:  
  
~~~
# Install current stable version.
pip install uwsgi

# Or install LTS (long term support).
pip install https://projects.unbit.it/downloads/uwsgi-lts.tar.gz
~~~
  
## uWSGI 모델
uWSGI는 클라이언트-서버 모델로 동작합니다. nginx나 Apache같은 웹 서버는 *django-uwsgi* 의 "워커 *worker*" 프로세스와 통신하며 동적 컨텐츠를 서빙합니다.  
  
---
  
## 장고와 함께 uWSGI를 사용하기 위한 설정 및 시작 요령
uWSGI는 프로세스 설정을 위해 어려 방법을 제공합니다. [uWSGI 설정 문서](https://uwsgi.readthedocs.io/en/latest/Configuration.html)를 더 살펴보세요.  
  
다음은 uWSGI 서버를 구동하는 예제입니다.  
  
~~~
uwsgi --chdir=/path/to/your/project \
    --module=mysite.wsgi:application \
    --env DJANGO_SETTINGS_MODULE=mysite.settings \
    --master --pidfile=/tmp/project-master.pid \
    --socket=127.0.0.1:49152 \      # can also be a file
    --processes=5 \                 # number of worker processes
    --uid=1000 --gid=2000 \         # if root, uwsgi can drop privileges
    --harakiri=20 \                 # respawn processes taking more than 20 seconds
    --max-requests=5000 \           # respawn processes after serving 5000 requests
    --vacuum \                      # clear environment on exit
    --home=/path/to/virtual/env \   # optional path to a virtualenv
    --daemonize=/var/log/uwsgi/yourproject.log      # background the process
~~~
  
위 예제는 여러분이 작성중인 코드의 최상위 프로젝트 패키지 폴더의 이름이 **mysite** 이고 WSGI **application** 객체를 포함하고 있는 모듈이 **mysite/wsgi.py** 에 있다고 가정합니다. 이 구조는 **mysite** 패키지 폴더 아래 위치한 프로젝트 이름이 **mysite** 라고 할 때 최신 Django 버전에서 **django-admin startproject mysite** 명령을 통해 생성된 것입니다. 만약 이 파일이 존재하지 않는다면 생성해야 합니다. 기본적으로 포함되어야 하는 내용과 추가할 수 있는 내용은 'WSGI로 장고 배포하기 [원본](https://docs.djangoproject.com/en/2.0/howto/deployment/wsgi/), [번역](https://jwkcp.github.io/2018/05/20/deployment-wsgi/)' 문서를 참고해주세요.  
  
Django에 특화된 옵션은 다음과 같습니다:  
  
- **chdir** : **mysite** 패키지를 포함하고 있는 디렉토리와 같이 파이썬을 임포트할 때 경로에 있어야 하는 디렉토리 경로  
- **module** : 사용할 WSGI 모듈 - 특별한 경우가 아니면 **[startproject](https://docs.djangoproject.com/en/2.0/ref/django-admin/#django-admin-startproject)** 명령으로 생성된 mysite.wsgi 파일이라고 생각하시면 됩니다.  
- **env** : 최소한 **DJANGO_SETTINGS_MODULE** 을 포함하고 있어야 합니다.
- **home** : virtualenv를 사용하고 있다면 그 경로를 여기에 써주세요.
  
아래는 ini 설정 파일 예제입니다.  
  
~~~
[uwsgi]
chdir=/path/to/your/project
module=mysite.wsgi:application
master=True
pidfile=/tmp/project-master.pid
vacuum=True
max-requests=5000
daemonize=/var/log/uwsgi/yourproject.log
~~~
  
이렇게 생성한 ini 설정 파일은 아래와 같이 사용할 수 있습니다.
  
~~~
uwsgi --ini uwsgi.ini
~~~
  
> **파일 업로드 시 UnicodeEncodeError 해결 방법**
>
> 아스키(ASCII) 문자가 아닌 문자를 포함한 파일명의 파일을 업로드할 때 **UnicodeEncodeError** 에러가 발생한다면 **uwsgi.ini** 설정 파일을 아래와 같이 바꿨는지 확인해야 합니다.  
>    
> env = LANG=en_US.UTF-8
>
> 자세한 내용은 유니코드 레퍼런스 가이드의 [파일 *Files(https://docs.djangoproject.com/en/2.0/ref/unicode/#unicode-files)*] 섹션을 참고하세요.

uWSGI 워커의 시작, 중지, 재시작에 관련된 정보는 uWSGI 문서의 [uWSGI 프로세스 다루기](https://uwsgi-docs.readthedocs.io/en/latest/Management.html) 부분을 참고하세요.
  
