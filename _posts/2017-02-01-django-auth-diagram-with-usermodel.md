---
layout: post
title: Django User 모델과 클래스형 뷰를 이용한 회원가입 과정 다이어그램
tags: django diagram at-a-glance
comments: true
---

제네릭 뷰와 폼, Django의 인증 모델은 사용의 편리함을 주지만 많은 부분들이 추상화되어 있어 요청부터 응답까지의 전체 과정이 머릿속에 쉽게 그려지지 않았다. 
 
아래는 이 과정을 스스로 정리해보기 위해 Django에서 User 모델과 클래스형 뷰를 이용한 회원가입 과정을 간단히 그려본 것이다.

[![django_auth_diagram_with_usermodel.png](https://s30.postimg.org/f5f9ezmz5/django_auth_diagram_with_usermodel.png)](https://postimg.org/image/w5y5no00d/)