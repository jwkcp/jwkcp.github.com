---
layout: post
title: 갑자기 장고 커스텀 User 모델의 admin에서 에러가 발생할 때 해결방법
tags: django
comments: true
---
  
---
  
## 문제
django 함수 테스트를 하기 위해 테스트 케이스를 만들고 **python manage.py test app_name** 과 같이 명령을 실행하면 뜬금없이 아래와 같은 에러가 발생하는 경우가 있습니다. 이 에러는 꼭 테스트 케이스를 실행하는 경우에만 발생하지 않으며 다른 경우에도 발생할 수 있습니다. 
  
~~~
django.contrib.admin.sites.NotRegistered: The model User is not registered

# 혹은

django.contrib.admin.sites.AlreadyRegistered: The model User is already registered
~~~
  
## 원인
맞습니다. 사실은 테스트 케이슬 실행 명령과 위 에러는 크게 관련이 없습니다. 이 에러는 settings.py에서 AUTH_USER_MODEL = 'auth.User'와 같이 지정하고 admin.py에서 아래와 같이 User모델을 불러다 쓰는 방법을 쓰는 경우에 INSTALLED_APPS의 나열 순서에 따라 발생합니다.
  
~~~
from django.contrib.auth import get_user_model

admin.site.unregister(get_user_model())
admin.site.register(get_user_model(), UserAdmin)
~~~
  
## 해결방법
get_user_model()을 쓰는 앱보다 django.contrib.auth가 먼저 와야 합니다. 
  
~~~
'accounts.apps.AccountsConfig', # 이 부분을 장고 기본 앱보다 아래 쪽에 배치하도록 합니다.

'django.contrib.admin',
'django.contrib.auth',
'django.contrib.contenttypes',
'django.contrib.sessions',
'django.contrib.messages',
'django.contrib.staticfiles',

# 여기에 'accounts.apps.AccountsConfig'를 위치시킵니다.
~~~
