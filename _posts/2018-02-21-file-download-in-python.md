---
layout: post
title: 파이썬에서 파일 다운로드 하기
tags: python, how-to
comments: true
---
  
다운로드 url이 있을 때, 배치 스크립트를 통한 wget을 쓰지 않고 파이썬으로 다운로드 할 수 있을까? 있다. 몇 가지 방법을 소개한다.
  
## 비추천
파이썬 파일 다운로드로 검색을 해보면 많이 등장하는 함수가 **urlretrieve()**다. 하지만 이 함수는 python2 시절 함수로 사용이 권장되지 않는다. 아래 내용만 봐도 언제 사라질지 모른다고 되어 있다. 그러니 가급적 이 함수를 이용해 다운로드를 구현하는 것음 삼가자.

> The following functions and classes are ported from the Python 2 module urllib (as opposed to urllib2). They might become deprecated at some point in the future. (출처: [python.org](https://docs.python.org/3.4/library/urllib.request.html))
  
## 추천
### Session() 함수를 사용하는 경우
[로그인 세션을 얻은 다음 파일을 다운로드 해야 하는 경우](https://jwkcp.github.io/2018/02/21/how-to-get-login-session-in-python/)에는 Session()함수를 쓰는 것이 더 편할 것이다. 아래와 같이 사용할 수 있다.

~~~
import requests
  
# 세션으로 get 요청을 보내면 다운로드 할 데이터가 반환된다. 
res = session.get(url, headers=headers)
  
# 
with open('./download_file.jpg', 'wb') as f:
    f.write(res.content)
~~~

### urlopen() 함수를 사용하는 경우
위에 Session()함수를 이용할 때와 거의 동일하다. 다만 urlopen()으로 리턴받은 응답(response)객체는 read() 함수로 값을 읽어줘야 한다.
~~~
from urllib.request import urlopen

with urlopen(url) as res:
    res_data = res.read()

    with open('./download_file.jpg', 'wb') as f:
        f.write(res_data)
~~~
  