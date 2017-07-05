---
layout: post
title: Django User 모델을 확장하는 4가지 방법
tags: how-to, django, how-to
comments: true
---

Django에서 기본으로 제공해주는 User 모델 이외에 추가 정보를 함께 저장해야 할 경우가 있다. 아래 4가지 방법은 이때 사용하는 방법이다.

---
1. 프록시 모델 사용하기
2. Profile을 활용한 One-To-One 관계 활용하기
3. AbstractBaseUser를 확장하여 User 모델 커스터마이징하기
4. AbstractUser를 확장하여 User 모델 커스터마이징하기

이 4가지 방법에 대한 자세한 설명은 [여기](https://simpleisbetterthancomplex.com/tutorial/2016/07/22/how-to-extend-django-user-model.html#onetoone)에 방문하면 된다.
   
