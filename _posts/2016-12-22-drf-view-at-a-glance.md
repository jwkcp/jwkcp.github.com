---
layout: post
title: 한눈에 보는 Django Rest Framework 뷰의 진화
tags: django at-a-glance drf
comments: true
---
[![rest_framework_slide1.png](https://s26.postimg.org/untr54a61/rest_framework_slide1.png)](https://postimg.org/image/dajgq9eut/)
[![rest_framework_slide2.png](https://s26.postimg.org/f3mdel01l/rest_framework_slide2.png)](https://postimg.org/image/m6u8u75h1/)

위 그림은 Django 뷰의 구현 방식의 진화를 한 눈에 살펴보기 위해 그려본 것이다. Generic class-based view를 보면 SnippetList와 SnippetDetail에 중복된 코드가 있다. 이를 더 간결하고 쉽게 사용하기 위해 django-rest-framework에서는 **Viewset**을 제공하고 있다.

[django-rest-framework 공식 튜토리얼](http://www.django-rest-framework.org/tutorial/6-viewsets-and-routers/ "django-rest-framework 공식 튜토리얼")에서는 아래와 같이 뷰(view)와 뷰셋(viewset)을 사용할 때의 트레이드-오프에 대해 언급하고 있다. 뷰셋은 유용한 추상화를 제공하며 일관된 API 접근, 코드 작성의 최소화, 일일이 URL conf를 작성하는 대신 상호작용과 표현에 더 집중할 수 있도록 도와주지만 마치 클래스 기반 뷰와 함수 기반 뷰를 선택할 때와 비슷한 트레이드 오프가 있으며, 직접 뷰를 작성할 때보다 덜 명료하다는 것. 그러니 뷰셋이 항상 좋은 선택이 아니라 상황에 따라 판단을 해야한다고.

>**“Trade-offs between views vs viewsets”**
>   
>Using viewsets can be a really useful abstraction. It helps ensure that URL conventions will be consistent across your API, minimizes the amount of code you need to write, and allows you to concentrate on the interactions and representations your API provides rather than the specifics of the URL conf.
>
>That doesn’t mean it’s always the right approach to take. There’s a similar set of trade-offs to consider as when using class-based views instead of function based views. Using viewsets is less explicit than building your views individually.