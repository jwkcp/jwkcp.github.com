---
layout: post
title: Django(장고) 배포 가이드(번역) - 1. 시작하기
tags: django deployment
comments: true
---
  
> 이 글은 장고(Django) 공식 페이지의 글을 번역한 것입니다. 원문은 [여기](https://docs.djangoproject.com/en/2.0/howto/deployment/)를 참고하세요.  
    
# 장고 배포하기
장고는 웹 개발자들의 삶을 편하게 만들어주는 도구로 가득하지만 사이트 배포 자체가 어렵다면 이런 것들은 아무 의미가 없죠. 쉬운 배포는 장고(Django) 개발이 시작된 후 항상 주된 목표였습니다.  
  
- WSGI를 이용해 배포하는 방법 [원문](How to deploy with WSGI) | [번역](#)
- 배포 체크리스트 [원문](https://docs.djangoproject.com/en/2.0/howto/deployment/checklist/) | [번역](https://jwkcp.github.io/2018/05/11/deployment-checklist-for-django/)
  
만약 여러분이 장고나 파이썬 입문자라면 우선  [mod_wsgi](https://docs.djangoproject.com/en/2.0/howto/deployment/wsgi/modwsgi/)를 사용해보길 권합니다. 대부분의 경우에 mod_wsgi는 가장 쉽고 빠르며 가장 안정적인 배포 방법이기 때문이죠.  
  
