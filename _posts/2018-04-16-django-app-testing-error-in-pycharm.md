---
layout: post
title: 파이참을 이용해 장고(django) 테스트 케이스 실행할 때 에러가 나는 경우 해결방법
tags: django
comments: true
---
  
---
  
## 문제 
파이참에서 테스트 케이스 만들고 실행하면 아래와 같은 에러가 나올 때가 있다.
  
~~~
"django.core.exceptions.AppRegistryNotReady"
~~~
  
~~~
ModuleNotFoundError: No module named 'mysite.polls'
~~~

~~~
RuntimeError: Model class myapp.models.person doesn't declare an explicit app_label and isn't in an application in INSTALLED_APPS.
~~~

---

## 원인
파이참에서 테스트 케이스 실행환경이 잘 설정되어 있지 않아서 그렇다. 일반적으로 settings.py 기본값으로 사용할 경우 발생하지 않을 수도 있는데 settings.py를 settings 폴더에 넣고 dev.py와 prod.py로 나눠서 사용하는 사람의 경우 발생할 가능성이 높다.
  
---

## 해결방법
1. 파이참에서 'Run > Edit Configurations...'에 들어간 후
2. 'Django tests'에서 작성한 테스트 케이스 선택 or 이후에 작성한 테스트 케이스도 동일하게 만들고 싶으면 'Defaults' 이용
3. 'Custom settings'에 자신이 개발환경에서 사용하는 settings.py가 잘 들어있는지 확인
4. OK 누르고 다시 테스트를 돌려보면 결과가 잘 나온다. (이 때, 'Run > Run...'을 눌러 ittest.~ 이 아닌 Test: ~ 로 시작하는 테스트를 실행하도록 하자.)  
  
## 부연설명
테스트 클래스를 만들 때 TestCase를 상속받는데 아래와 같이 2가지 라이이브러리에서 불러올 수 있다.  
여기에서는 **from django.test import TestCase**를 사용했다.

~~~
from unittest import TestCase
~~~
  
~~~
from django.test import TestCase
~~~
  
