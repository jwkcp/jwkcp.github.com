---
layout: post
title: 장고 어드민에서 모델 필드 외 다른 정보를 보여주고 싶을 때(혹은 하이퍼링크를 만들고 싶을 때)
tags: django, how-to, admin
comments: true
---
  
## 문제 상황
장고(django)에서 모델을 가지고 어드민을 구성하다보면 list_display를 통해 어떤 항목을 보이게 할 건지 지정할 수 있다. 그런데 모델에 정의한 필드 외에 다른 정보를 표시할 필요가 있을 땐 어떻게 해야할까? 혹은 장고 어드민에서 특정 필드를 하이퍼링크로 나타내고 싶다면? 검색해보면 굉장히 헷갈리는 정보나 이미 폐기(Deprecated)된 속성을 이용하는 등 잘못된 방법이 많이 나온다. 이런 경험을 했던 사람이 있다면 아래 방법을 참고하자.
  
## 추가 정보를 나타낼 함수를 만들고 불러쓰기
1. 모델에 함수를 만들어 준다.  
~~~
class MyClass:
    # 필드 정의가 여기에 있다.
     
    def file_info(self):
        return "보여주고 싶은 정보"
~~~
  
2. admin.py의 list_display에 함수명을 써주면 끝. 이게 끝이다. 하이퍼링크나 컬럼명을 지정하고 싶은 사람은 아래 내용을 참조.
  
~~~
list_display = ['file_info',]
~~~
  
---
  
## 하이퍼링크 만들기
format_html 라이브러리를 임포트해서 쓴다. 인터넷 검색하면 많이 나오는 **allow_tags는 폐기(Deprecated)**되었으니 시간낭비하지 말자.
  
~~~
from django.utils.html import format_html

class MyClass:
    # 필드 정의가 여기에 있다.
     
    def file_info(self):
        return format_html('<a href="http://naver.com">네이버</a>')
~~~
  
## 어드민 컬럼 제목 정하기
어드민의 컬럼명까지 정하고 싶은 경우 아래와 같이 short_description 속성을 지정하면 된다.
  
~~~
from django.utils.html import format_html

class MyClass:
    # 필드 정의가 여기에 있다.
     
    def file_info(self):
        return format_html('<a href="http://naver.com">네이버</a>')
    
    file_info.short_description = "원하는 컬럼명"
~~~
  