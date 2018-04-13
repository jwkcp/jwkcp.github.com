---
layout: post
title: 파이썬에서 CERTIFICATE_VERIFY_FAILED 에러가 발생하는 경우
tags: django python
comments: true
---
  
---
  
## 문제
파이썬이나 혹은 장고(django)로 https로 된 사이트에 요청을 보낼 때 아래와 같은 에러가 발생하는 경우가 있다.
  
~~~
<urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:777)>
~~~

## 원인
개정된 [PEP 467](https://www.python.org/dev/peps/pep-0476/)에 따라 모든 https 통신은 필요한 인증서와 호스트명을 기본으로 체크하도록 되어 있어서 그렇다. [영향을 받는 라이브러리는 urllib, urllib2, http, httplib](https://www.python.org/dev/peps/pep-0476/#id24) 이다.

## 해결방법
ssl._create_unverified_context()를 urllib.request.urlopen의 context 파라메터로 넘겨주면 된다. 그러면 에러가 발생하지 않고 https 주소로 요청을 보내고 응답을 받을 수 있다.

~~~
# 예제
  
context = ssl._create_unverified_context()
response = urllib.request.urlopen(requests, data=data.encode('utf-8'), context=context)
~~~

함수명에 _(언더스코어)가 있다는 것은 private이고 내부적으로 쓰이는 함수라 이걸 이런 식으로 호출해서 써도 되나 안되나 고민이 되었는데 [파이썬 공식 사이트](https://docs.python.org/2/library/httplib.html#httplib.HTTPSConnection)에 보면 아래와 같이 나와 있어 안심하고 써도 될 것 같다. 가장 좋은 방법은 필요한 인증서와 호스트명을 넣고 올바른 SSL 통신을 하는 것이지만. : )
  
> This class now performs all the necessary certificate and hostname checks by default. To revert to the previous, unverified, behavior ssl._create_unverified_context() can be passed to the context parameter.
  
(번역)
> 이 클래스는 이제 모든 필요한 인증서와 호스트명을 기본으로 체크하도록 동작합니다. 예전처럼 유효성 검증을 하지 않는 채 동작하도록 하기 위해서는 context 파라메터로 ssl._create_unverified_context()를 전달하면 됩니다.
  
## 추가로 알아두면 좋은 정보
만약 urllib나 urllib2가 아닌 requests 라이브러리를 별도로 설치해서 통신하고 있다면 아래와 같이 하면 된다.
  
~~~
import requests
  
response = requests.get('https://mydomain.com', verify=False)
~~~
