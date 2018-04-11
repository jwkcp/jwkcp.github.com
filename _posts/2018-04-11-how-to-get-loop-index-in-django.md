---
layout: post
title: Django(장고) 템플릿 태그에서 반복문 돌 때 인덱스 얻는 방법
tags: django
comments: true
---
  
---
  
아래 < 와 > 는 { 과 } 으로 바꿔써야 한다.
  
~~~
<% for item in items %>
    << forloop.counter >>
<% endfor %>
~~~
  
참고링크: [파이썬에서 반복문 돌 때 인덱스를 얻기 위해 권장되는 방법](https://jwkcp.github.io/2018/02/21/how-to-get-loop-index-in-python/)

  