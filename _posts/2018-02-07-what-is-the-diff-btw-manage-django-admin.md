---
layout: post
title: manage.py와 django-admin.py의 차이점
tags: django
comments: true
---

### 똑같은거 아니야?
django를 명령을 실행할 때 **python manage.py startproject**나 **python django-admin.py startproject**나 똑같이 django 프로젝트가 생성된다. 몇 가지 다른 명령으로도 테스트해보면 대부분 문제없이 실행되는 경우가 많기 때문에 별 생각없이 그냥 사용할수도 있다. 그러나 둘은 분명히 다르다.
  
### 어떤 점이 다른가?
[공식문서](https://docs.djangoproject.com/en/2.0/ref/django-admin/)를 보면 아래와 같이 manage.py가 가진 특징을 2가지 언급하고 있다.
  
1. It puts your project’s package on sys.path.
2. It sets the DJANGO\_SETTINGS\_MODULE environment variable so that it points to your project’s settings.py file.
  
manage.py를 쓰면 sys.path에 프로젝트를 추가해주기도 하면서 어떤 설정 파일(settings.py)을 쓸지 DJANGO\_SETTINGS\_MODULE 변수에 설정된 값을 이용해 매번 settings.py를 지정하는 불편함을 줄여준다는 거다. 그럼 애초에 manage.py만 쓰도록 하지 왜 django-admin.py랑 같이 쓸 수 있게 해둬서 헷갈리게 하는걸까 하는 의문이 든다. 

### 왜 두 개를 다 쓰게 해둔걸까?
1. manage.py는 django-admin.py로 프로젝트를 생성할 때 만들어진다. 따라서 처음에는 django-admin.py를 써야지 manage.py를 쓸 수 없다.
2. 하나의 소스로 운영 환경이 여럿일 경우 편의를 위해 settings.py도 여러 개로 나누어 쓰는 것이 일반적이다. 이 때, manage.py에 settings.py 경로를 입력해두면 더 편하게 쓸 수 있다. 그러나 별도로 환경변수에 DJANGO\_SETTINGS\_MODULE를 설정해서 쓰는 사람의 경우 django-admin.py를 쓰는 것이 더 편리할 것이다. [공식문서](https://docs.djangoproject.com/en/2.0/ref/django-admin/)에서는 단일 django 프로젝트인 경우 django-admin.py보다 manage.py가 더 편리하다고 언급하고 있다. 자산의 상황에 맞는 사용을 하면 더 편리하다는 뜻!
  
아래 **os.environ.setdefault("DJANGO\_SETTINGS\_MODULE", "django.settings")** 를 보면 변수를 설정해주는 부분이 있는 것을 볼 수 있다. 라이브러리 호출에 따른 예외 처리도 django-admin.py가 가지지 못한 장점이다.
   
~~~
#!/usr/bin/env python
import os
import sys

if __name__ == "__main__":
    os.environ.setdefault("DJANGO_SETTINGS_MODULE", "django.settings")
    try:
        from django.core.management import execute_from_command_line
    except ImportError as exc:
        raise ImportError(
            "Couldn't import Django. Are you sure it's installed and "
            "available on your PYTHONPATH environment variable? Did you "
            "forget to activate a virtual environment?"
        ) from exc
    execute_from_command_line(sys.argv)
~~~