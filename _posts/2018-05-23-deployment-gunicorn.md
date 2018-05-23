---
layout: post
title: Django(장고) 배포 가이드(번역) - 6. Gunicorn 사용법
tags: django deployment
comments: true
---
  
> 이 글은 장고(Django) 공식 페이지의 글을 번역한 것입니다. 원문은 [여기](https://docs.djangoproject.com/en/2.0/howto/deployment/wsgi/gunicorn/)를 참고하세요.  
    
# Gunicorn 사용법
  
[Gunicorn('Green Unicorn')](http://gunicorn.org/)은 UNIX 시스템에서 구동되는 순수 파이썬 WSGI 서버입니다. 별도로 요구되는 의존성도 없고 설치하기도 사용하기도 쉽습니다.   
  
## Gunicorn 설치하기
설치는 **pip install gunicorn** 로 끝입니다. 매우 쉽죠? 더 자세한 내용이 궁금하다면 [Gunicorn 공식 문서](http://docs.gunicorn.org/en/latest/install.html)를 참고하세요.  
  
---
  
## Gunicorn에서 제네릭 WSGI 응용 어플리케이션으로 장고(Django) 실행하기
Gunicorn이 설치되면 **gunicorn** 명령으로 Gunicorn 서버 프로세스를 실행할 수 있게 됩니다. 정말 간단하게도 gunicorn을 호출하기 위해선 *application* 이라는 이름을 가진 WSGI 어플리케이션 객체가 있는 위치만 알려주면 됩니다. 이렇게요.  
  
~~~
gunicorn myproject.wsgi
~~~
  
이렇게 하면 하나의 스레드가 **127.0.0.1:8000** 주소로 리스닝을 시작하며 하나의 프로세스가 실행됩니다. 이 명령은 파이썬 경로에 있어야 하는데 이를 쉽게 확인하는 방법은 **manage.py** 파일이 있는 디렉토리에서 실행하는 것입니다.  
  
더 많은 팁을 배우고 싶다면 [Gunicorn 배포 문서](http://docs.gunicorn.org/en/latest/deploy.html)를 참고하세요.  
