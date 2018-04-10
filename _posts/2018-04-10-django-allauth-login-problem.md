---
layout: post
title: django-allauth로 로그인할 때 자꾸 signup 주소로 리다이렉트 될 때 해결방법
tags: django
comments: true
---
  
---
  
## 문제
[django-allauth](https://github.com/pennersr/django-allauth)를 이용해 소셜 로그인 구현 시 자꾸 accounts/social/signup 주소로 이동한다. 원래는 로그인한 소셜 계정으로 User 모델에 값이 들어가고 profile 주소로 이동해야 하는데 말이다.
  
## 원인
소셜 로그인에 이메일 계정이 이미 User 모델에 사용되고 있기 때문이다. django-allauth가 소셜 로그인 사용자를 만들고 주소를 리다이렉트하면서 문제가 생긴다.

## 해결
소셜 로그인 이메일과 같은 이메일이 있는지 User모델에서 확인 후 변경 또는 삭제해준다.
  
  