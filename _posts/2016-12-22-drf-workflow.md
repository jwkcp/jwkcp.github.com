---
layout: post
title: 단 한장의 그림으로 Django Rest Framework(DRF) 구조 이해하기
tags: django drf at-a-glance
comments: true
---
Django를 이용해 RESTful 서비스를 구현하기 위해선 django-rest-framework라는 훌륭한 패키지가 있다. 이 패키지는 여기에 가면 즉시 구현해볼 수 있는 예제와 함께 자세한 사항이 기록돼 있다. 아래 그림은 이 패키지의 문서를 보면서 전체 구조를 한번에 그려보기 위해 [QuickStart 문서](http://www.django-rest-framework.org/#quickstart "QuickStart 문서")를 보고 나름대로 정리해 그려본 것이다. 아래 그림이 이 포스팅에서 말하고자 하는 전부다.

[![DRF_WORKFLOW.png](https://s26.postimg.org/v34ir82nt/DRF_WORKFLOW.png)](https://postimg.org/image/vsnb3l379/)

---

사용자는 처음 웹브라우저와 같은 클라이언트를 통해 url request를 보내고 urls.py가 이를 처음 맞는다.

다른 언어/프레임워크에서 흔히 MVC(**M**odel-**V**iew-**C**ontroller)라고 부르는 패턴을 Django는 MTV(Model-Template-View)라고 부른다. 이름에서 알 수 있듯이 views.py가 모든 로직을 관장하는 Controller의 역할을 하고 있다. 아래 그림에서도 보면 views.py가 권한(permissions.py), 데이터베이스(models.py), 시리얼라이제이션(serializers.py) – 대부분의 도서가 직렬화/역직렬화로 표현 – 등을 총괄하는 것을 볼 수 있다.

viewset은 코드 반복 최소화를 위해 제네릭뷰를 사용하고 있다. settings.py에서 정한 권한(permission)은 기본 퍼미션으로 코드 상에서 개별 지정한 권한(permission)이 이보다 우선한다. 초록색 화살표를 따라가면서 흐름을 보자. 그리고 [django-rest-framework.org의 문서](http://www.django-rest-framework.org/ "django-rest-framework.org의 문서")를 보면 이해에 큰 도움이 될 것이다.

