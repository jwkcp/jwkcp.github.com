---
layout: post
title: Django(장고) 템플릿 태그에서 모듈로 연산 사용하기
tags: django
comments: true
---
  
---
  
아래와 같이 forloop.counter 혹은 forloop.counter0를 이용해 divisibleby 필터를 이용하면 된다.  
   
~~~
{% if forloop.counter|divisibleby:5 %}
~~~
  
이 구문은 if x % 5 == 0: 과 같다.
  