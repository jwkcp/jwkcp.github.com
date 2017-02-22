---
layout: post
title: Django 소셜 로그인 라이브러리(social-app-django) 단계별 값 변화 살펴보기
tags: django
comments: true
---
Django에서 소셜 로그인을 구현할 때 많이 사용하는 [social-app-django]("https://github.com/python-social-auth/social-app-django")에 대한 글이다. 얼마 전까지 [django-social-auth]("https://github.com/omab/django-social-auth")였었으나 더 이상 사용되지 않을 예정(Deprecated)이라고 하니 참고하자. Python Social Auth의 전체 최신 버전은 [여기]("https://github.com/python-social-auth")에서 확인할 수 있다.

***
## 파이프라인 순서

social-app-django는 소셜 로그인 과정에서 사용자가 원하는 작업을 수행할 수 있도록 settings.py에 각 과정을 파이프라인(Pipeline)으로 만들어 놓았다. 'myapp.social.check_pipeline_value'함수는 각 단계별 값을 확인해보기 위해 필자가 넣은 것이다. 
   
{% highlight python %}
SOCIAL_AUTH_PIPELINE = (
   # Pipeline 흐름에 따른 값을 체크해 보기 위해 만든 커스텀 함수(1/3)
   'myapp.social.check_pipeline_value',
    
   'social_core.pipeline.social_auth.social_details',
   'social_core.pipeline.social_auth.social_uid',
   'social_core.pipeline.social_auth.auth_allowed',
    
   # Pipeline 흐름에 따른 값을 체크해 보기 위해 만든 커스텀 함수(2/3)
   'myapp.social.check_pipeline_value',
    
   'myapp.social.show_confirm',
   'social_core.pipeline.social_auth.social_user',
   'social_core.pipeline.user.get_username',
    
   # 사용자의 입력을 받아보기 위해 Partial Pipeline으로 만든 커스텀 함수
   'myapp.social.create_user',
    
   # Pipeline 흐름에 따른 값을 체크해 보기 위해 만든 커스텀 함수(3/3)
   'myapp.social.check_pipeline_value',
    
   'social_core.pipeline.social_auth.associate_user',
   'social_core.pipeline.social_auth.load_extra_data',
   'social_core.pipeline.user.user_details',
)
{% endhighlight %}

***

## 파이프라인에 단계에 따른 값 변화
   
소셜 로그인을 처음 써보는 입장에서 이 값이 어떻게 변화하는지 궁금했는데 관련된 자료를 찾기가 어려웠다. 아래는 필자가 직접 찍어보고 비교한 것을 보기 쉽게 표로 만든 것이다. 나중에 비슷한 경우의 시작하는 개발자가 있다면 도움이 되지 않을까.  
   
[![django_social_auth_pipeline_cf.png](https://s26.postimg.org/oalnypjzt/django_social_auth_pipeline_cf.png)](https://postimg.org/image/hk56p9wtx/)