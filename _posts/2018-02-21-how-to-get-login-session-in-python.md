---
layout: post
title: 파이썬으로 로그인 인증이 필요한 웹사이트 크롤링할 때 로그인 세션 얻는 방법
tags: python, how-to
comments: true
---
  
파이썬으로 웹사이트를 크롤링할 때 로그인이 필요한 경우가 있다. 이때는 requests에 Session()을 사용하면 전달할 데이터, SSL검증, 리다이렉트 등의 동작을 편하게 컨트롤 할 수 있으니 알아두자.
아래의 예는 특정 url로 post메시지를 보내 302메시지를 보내고 쿠키를 생성한 후 진짜 로그인하는 페이지로 이동하는 웹사이트에 로그인 세션을 얻는 방법이다.
  
## 로그인에 사용할 데이터 준비
아래 LOGIN_DATA는 사전형(Dictionary) 자료구조다. 키 값을 보면 'id', 'pass' 이렇게 있는데 이건 정해진 값이 아니라 크롤링하려는 웹사이트에서 각자 찾아야 한다. html 소스보기를 해보면 로그인 아이디, 비밀번호를 입력하는 텍스트 박스에 id값이 있을 것이다. 이 값을 여기에 넣으면 된다.
~~~
LOGIN_URL = 'http://example.com/login.do'
LOGIN_DATA = {
    'id': 'myid',
    'pass': 'mypassword'
}
~~~

## requests의 Session() 사용하기
아래에서 with를 사용하면 중간에 세션이 분실되는 경우를 방지해주고, 모든 작업이 끝나 with문을 벗어나면 자동으로 세션을 종료해주기 때문에 편리하다.  
verify를 False로 해주면 SSL 인증서로 인해 발생하는 에러를 무시한다.  
allow_redirects를 False로 하면 강제 리다이렉트를 해제할 수 있다.  
  
아래에 보면 확보한 헤더에서 쿠키(Set-Cookie)와 주소(Location)을 가져오는 것을 알 수 있다. 이 값을 생성한 세션 인스턴스를 통해 get, post 등 요청을 보낼 때 헤더에 넣어서 보내면 된다.
   
~~~
with requests.Session() as s:
    res = s.post(LOGIN_URL, data=LOGIN_INFO, verify=False, allow_redirects=False)

    # 쿠키와 헤더에 포함된 302 Location 값을 가져온다.
    # 이때, 헤더에 설정된 쿠키와 함께 Location으로 Get Request 를 보내면 된다.
    redirect_cookie = res.headers['Set-Cookie']
    redirect_url = res.headers['Location']
    headers = {"Cookie": redirect_cookie}

    # Location 주소로 Get Request 호출
    s.get(redirect_url, headers=headers)
~~~
  
## 추가 팁! BeautifulSoup 빠르게 살펴보기
웹사이트 HTML DOM을 파싱할 때 너무 유명한 라이브러리라 따로 설명이 필요할지 모르겠다. pip로 다운로드 받으려면 beautifulsoup4로 검색해야 한다.
  
~~~
pip install beautifulsoup4
~~~
  
이렇게 임포트하고
~~~
from bs4 import BeautifulSoup as bs
~~~
  
아래와 같이 세션 함수로 얻은 html문서를 bs()생성자에 넣으면 soup 객체를 얻을 수 있다. select 함수의 메개변수로 들어가는 CSS 선택자 경로는 크롬에서 개발자 도구를 활성화한 다음 Elements 탭에서 마우스 오른쪽 버튼을 눌러 'Copy > Copy selector'를 누르면 쉽게 경로를 딸 수 있으니 참고!
~~~
html = s.get(url, headers=headers)
soup = bs(html.text, 'html.parser')
elements = soup.select('dt > a')
elements.get('href') # 링크를 가져오고 싶다면!
~~~
  