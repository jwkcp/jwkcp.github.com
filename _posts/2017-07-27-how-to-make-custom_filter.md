---
layout: post
title: 장고(Django)에서templatetags를 이용한 커스텀 필터 만들기 
tags: django 
comments: true
---

### 1. 폴더구조
- myproject
  - myproject
    - settings.py
    - \__init__.py
    - tests.py
    - urls.py
    - wsgi.py
    - **templatetags**
  - manage.py

### 2. 설정 
settings.py에 INSTALLED_APPS에 메인 폴더의 이름을 등록해준다.

### 3. templatetags 폴더 생성
메인 폴더, 예를 들어 myproject라는 Django 프로젝트를 만들었으면 그 안에 같은 이름으로 myproject 폴더가 생기고 여기에 settings.py, wsgi.py 등이 생기게 된다. 여기에 templatetags 폴더를 만든다. 

### 4. 코드 작성
그리고 templatetags 폴더 아래 \__init__.py(빈 파일)과 커스텀 필터 코드를 작성할 파일(원하는이름.py)을 아무 이름으로나 만든다. 여기서는 my_filter.py라고 짓겠다.

{% highlight python %}
from django import template


register = template.Library()

@register.filter
def my_lovely_filter(value):
    return value
{% endhighlight %}
     

### 5. 사용하기
아래와 같이 템플릿 태그를 사용할 때 필터를 적용할 수 있다. (아래 소괄호 두 개는 중괄호 두 개로 바꿔야 한다.)
     
html 상단에 커스텀 필터를 로드해주고
{% highlight python %}
(( load my_filter.py ))
{% endhighlight %}
      
아래와 같이 사용하면 된다.
{% highlight python %}
(( value|my_lovely_filter )) 
{% endhighlight %}
