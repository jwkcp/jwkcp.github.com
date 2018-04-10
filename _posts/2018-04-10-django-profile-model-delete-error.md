---
layout: post
title: Django 사용자 삭제 시 에러('bool' object is not callable)가 날 때 해결방법
tags: django
comments: true
---

---
  
## 문제
Django에서 Profile 모델을 구현 후 사용자(User)를 삭제할 때 아래와 같은 에러가 발생하면서 삭제되지 않는다.
  
~~~
TypeError at /admin/auth/user/
'bool' object is not callable
~~~
  
## 원인
User 모델과 Profile 모델 관계를 설정하면서 잘못 설정했을 수 있다. 아래는 잘못 설정한 예.
  
~~~
user = models.OneToOneField(settings.AUTH_USER_MODEL, on_delete=True)
~~~
  
## 해결방법
아래와 같이 on_delete=True를 on_delete=models.CASCADE 로 수정해준다.
  
~~~
user = models.OneToOneField(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
~~~
  