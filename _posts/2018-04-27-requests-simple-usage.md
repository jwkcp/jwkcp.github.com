---
layout: post
title: python http 라이브러리 requests 초간단 사용법
tags: python
comments: true
---
  
---
  
> 아래 내용은 [공식 사이트](http://docs.python-requests.org/)에서 일부 발췌, 번역한 것입니다. 
  
# 간단 사용법
아래 예제는 [공식 사이트](http://docs.python-requests.org/en/master/)에서 발췌했다. [여기](https://gist.github.com/kennethreitz/973705)에서 urllib2를 쓸 때와 비교해 얼마나 더 간결하게 코딩할 수 있는지 확인할 수 있다.  
    
~~~
import requests

# 요청 (한 줄이면 끝!)
r = requests.get('https://api.github.com/user', auth=('user', 'pass'))
  
# 응답 (어떻게 쓸까?)
> r.status_code
200

> r.headers['content-type']
'application/json; charset=utf8'

> r.encoding
'utf-8'

> r.text
u'{"type":"User"...'

> r.json()
{u'private_gists': 419, u'total_private_repos': 77, ...}
~~~

---

# 상세 설명
  
**GET 메소드 사용**
[requests.get(url, params=None, **kwargs)](http://docs.python-requests.org/en/master/api/#requests.get)
~~~
def get(url, params=None, **kwargs):
    r"""Sends a GET request.

    :param url: URL for the new :class:`Request` object.
    :param params: (optional) Dictionary or bytes to be sent in the query string for the :class:`Request`.
    :param \*\*kwargs: Optional arguments that ``request`` takes.
    :return: :class:`Response <Response>` object
    :rtype: requests.Response
    """

    kwargs.setdefault('allow_redirects', True)
    return request('get', url, params=params, **kwargs)
~~~
  
**POST 메소드 사용**
[requests.post(url, data=None, json=None, **kwargs)](http://docs.python-requests.org/en/master/api/#requests.post)
~~~
def post(url, data=None, json=None, **kwargs):
    r"""Sends a POST request.

    :param url: URL for the new :class:`Request` object.
    :param data: (optional) Dictionary (will be form-encoded), bytes, or file-like object to send in the body of the :class:`Request`.
    :param json: (optional) json data to send in the body of the :class:`Request`.
    :param \*\*kwargs: Optional arguments that ``request`` takes.
    :return: :class:`Response <Response>` object
    :rtype: requests.Response
    """

    return request('post', url, data=data, json=json, **kwargs)
~~~
  
**requests 메소드 사용**  
위의 get, post의 구현을 보면 결국 request메소드를 쓰기 쉽게 랩핑(Wrapping)한 것임을 알 수 있다.  
  
~~~
import requests
req = requests.request('GET', 'http://httpbin.org/get')

<Response [200]>
~~~

---

## 그래서 리턴 값을 어떻게 활용할 수 있나?
[requests.Response](http://docs.python-requests.org/en/master/api/#requests.Response)의 객체를 리턴한다. 다양한 매소드와 속성이 있지만 몇 개만 나열해본다. 자세한 사항은 링크를 참고.  
  
> headers - 대소문자를 구별하지 않는다. 응답 객체의 헤더 정보가 포홤되어 있다.
  
> json() - json으로 인코딩된 응답 내용을 리턴한다.  
   
> status_code - 404, 200 같은 http 응답 상태 코드.  
   
> text - 유니코드로 인코딩된 응답 내용.  
  
> contents - 바이트로 인코딩된 응답 내용.
  
