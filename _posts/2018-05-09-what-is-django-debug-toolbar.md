---
layout: post
title: django-debug-toolbar란 무엇인가.
tags: django
comments: true
---

---

## 개요  
Django(장고)를 배울 때 잊을만하면 눈에 띄는 라이브러리가 있다. **[django-debug-toolbar](https://github.com/jazzband/django-debug-toolbar)** 가 그것이다. Django의 버전부터, cpu, 설정, 헤더, 요청, sql, 정적 파일, 템플릿, 캐시, 신호(signal), 로깅과 리다이렉트 가로채기(Intercept redirect)가 보인다.  

자세하고 순차적인 사용법은 [공식 설명서](https://django-debug-toolbar.readthedocs.io/en/stable/installation.html#getting-the-code)를 참고하면 된다. 아래는 간단히 정리한 사용법이다.  

---

## 사용법  
**1/3)설치**   
~~~
pip install django-debug-toolbar
~~~

**2/3)설정(settings.py)**   
~~~
## 설치 앱에 추가 (django.contrib.staticfiles는 기본으로 있을 수도 있다.)
INSTALLED_APPS = [
    # ...
    'django.contrib.staticfiles',
    # ...
    'debug_toolbar',
]

# 미들웨어에 추가
MIDDLEWARE = [
    # ...
    'debug_toolbar.middleware.DebugToolbarMiddleware',
    # ...
]

STATIC_URL = '/static/'

# 이 아이피에서만 디버그 툴바가 보인다.
INTERNAL_IPS = ('127.0.0.1',)
~~~

**3/3)프로젝트 레벨 URL 설정(urls.py)**    
~~~
from django.conf import settings
from django.conf.urls import include, url

if settings.DEBUG:
    import debug_toolbar
    urlpatterns = [
        url(r'^__debug__/', include(debug_toolbar.urls)),
    ] + urlpatterns
~~~

이제 브라우저에서 실행시켜보면 우측에 디버그 툴바가 보일 것이다.
