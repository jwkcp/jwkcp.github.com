---
layout: post
title: Django(장고)에서 404.html 페이지를 찾을 수 없다고 나올 때 제일 먼저 확인할 사항
tags: django
comments: true
---

---

templates 아래 분명히 404.html을 만들어 두었고 DEBUG도 True로 바꾸었는데 404.html 페이지가 나오지 않는다면 ALLOWED_HOSTS를 확인해보자. DEBUG를 True로 두었으면 ALLOWED_HOSTS에도 허용할 호스트를 나열해 주어야 한다. 예를 들어 mydomain.com 이라면  

~~~
ALLOWED_HOSTS = ['mydomain.com',]
~~~
을 해주고 테스트 환경도 추가해준다.  

~~~
ALLOWED_HOSTS = ['mydomain.com', 'localhost', '127.0.0.1',]
~~~

이제 다시 해보면 404.html이 정상적으로 뜰 것이다.
  
