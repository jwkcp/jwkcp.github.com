---
layout: post
title: Django(장고) 배포 가이드(번역) - 2. WSGI를 이용해 배포하기
tags: django deployment
comments: true
---
  
> 이 글은 장고(Django) 공식 페이지의 글을 번역한 것입니다. 원문은 [여기](https://docs.djangoproject.com/en/2.0/howto/deployment/wsgi/)를 참고하세요.  
  
# WSGI를 이용한 배포 방법
 
장고의 주 배포 플랫폼은 웹 서버와 어플리케이션 개발의 파이썬 표준인 [WSGI](http://wsgi.readthedocs.io/en/latest/)입니다.  
  
장고(Django)의 startproject 명령은 프로젝트 설정과 WSGI 호환 서버를 사용할 수 있도록 하는 기본적이고 심플한 WSGI 환경을 구성합니다.  
  
장고(Django)는 입문자를 위해 아래 WSGI에 관한 문서도 포함하고 있습니다.  
  
- [아파치(Apache) 웹서버와 mod_wsgi 사용법](https://docs.djangoproject.com/en/2.0/howto/deployment/wsgi/modwsgi/)
- [아파치(Apache) 웹서버에서 장고(Django) 사용자 데이터베이스 인증하기](https://docs.djangoproject.com/en/2.0/howto/deployment/wsgi/apache-auth/)
- [Gunicorn 사용법](https://docs.djangoproject.com/en/2.0/howto/deployment/wsgi/gunicorn/)
- [uWSGI 사용법](https://docs.djangoproject.com/en/2.0/howto/deployment/wsgi/uwsgi/)
  
## **어플리케이션** 객체
WSGI를 통해 배포하는 것의 핵심 컨셉은 어플리케이션 서버가 여러분이 작성한 코드와 통신하는데 사용되도록 한다는 '호출 가능한 어플리케이션' 개념입니다. 파이썬 모듈에서는 일반적으로 서버에 접근할 수 있도록 **application** 이란 이름을 사용합니다.  
  
**startproject** 명령은 이 **application** 을 포함하고 있는 **<project_name>/wsgi.py** 파일을 생성합니다.  
  
그리고 이것은 장고(Django)의 개발 서버와 WSGI를 이용한 운영 배포환경 모두에서 사용됩니다.  
  
WSGI 서버는 설정을 읽어 **application** 의 경로를 가져옵니다. 장고에 기본적으로 포함되어 있는 서버, 즉 **[runserver](https://docs.djangoproject.com/en/2.0/ref/django-admin/#django-admin-runserver)** 명령은 [WSGI_APPLICATION](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-WSGI_APPLICATION) 설정에서 경로를 가져옵니다. 기본적으로 **<project_name>/wsgi.py** 에 호출 가능 객체 **application** 을 가리키는 **<project_name>.wsgi.application** 로 설정되어 있습니다.  
  
---
  
## 설정 모듈 구성
WSGI 서버가 여러분이 만든 어플리케이션을 로드할 때, 장고(Django)는 전체 어플리케이션에 관한 항목이 정의된 설정 모듈을 불러오도록 되어 있습니다.  
  
이 때 장고(Django)는 올바른 설정 모듈의 위치를 알아내기 위해 **[DJANGO_SETTINGS_MODULE](https://docs.djangoproject.com/en/2.0/topics/settings/#envvar-DJANGO_SETTINGS_MODULE)** 환경 변수를 이용합니다. 만약 여러분이 개발 환경과 운영 환경에서 각기 다른 설정을 이용하도록 하고 싶다면 이 값을 이용해서 원하는 구성을 만들 수도 있습니다.  
  
만약 이 변수가 설정되어 있지 않다면 기본 **wsgi.py**는 **mysite.settings** 를 가리킵니다.(여기서 **mysite** 는 프로젝트의 이름입니다.) 이게 바로 **runserver** 가 기본적으로 기본 설정 파일을 탐색하는 방법입니다.  
  
> 일러두기
> 환경변수는 프로세스가 동작하는 범위 안에서만 유효하기 때문에 같은 프로세스 안에서 여러 개의 장고 사이트를 돌리면 제대로 동작하지 않습니다. 이런 현상은 mod_wsgi를 사용하다보면 발생하는데요. 이 문제를 피하기 위해 mod_wsgi를 독립된 데몬 프로세스 안에서 각각의 사이트를 데몬(daemon) 모드를 사용해 운영하거나, **wsgi.py** 의 **os.environ["DJANGO_SETTINGS_MODULE"] = "mysite.settings"** 를 강제 지정하는 방법을 환경 변수를 오버라이드하는 방법을 사용합니다.  
  
---
  
## WSGI 미들웨어(middleware) 적용하기
[WSGI middleware](https://www.python.org/dev/peps/pep-3333/#middleware-components-that-play-both-sides)를 적용하려면 application 이름의 어플리케이션 객체를 간단히 감싸주기만 하면 됩니다. 아래와 같이 wsgi.py 파일의 하단에 몇 줄만 추가하는 걸로 끝이죠.  
  
~~~
from helloworld.wsgi import HelloWorldApplication  
application = HelloWorldApplication(application)
~~~
  
장고(Django) 어플리케이션과 다른 프레임워크의 WSGI 어플리케이션을 결합하는 경우, 장고(Django) WSGI 어플리케이션을 나중에 장고(Django) WSGI 어플리케이션에게 위임하는 방법을 사용하는 커스텀 WSGI 어플리케이션으로 교체할 수도 있습니다.
  

  

    
