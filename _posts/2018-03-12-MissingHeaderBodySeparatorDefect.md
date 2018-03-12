---
layout: post
title: 파이썬으로 HTML 파싱 시 MissingHeaderBodySeparatorDefect 에러가 날 때
tags: python, how-to
comments: true
---
  
## 문제
requests.get 등으로 html text를 받아 beautifulsoup으로 파싱하려 할 때 MissingHeaderBodySeparatorDefect 또는 Server : Wow 3.8 와 같은 문구를 포함한 에러가 발생할 때가 있다.
  
## 원인
[파이썬 공식문서](https://docs.python.org/3/library/email.errors.html)에 보면 아래와 같이 설명하고 있다.  
> MissingHeaderBodySeparatorDefect - A line was found while parsing headers that had no leading white space but contained no ‘:’. Parsing continues assuming that the line represents the first line of the body.
  
굳이 좀 번역을 해보자면 이렇다.
> 헤더 바디에 구분자 누락 - 선행 공백이 없고 :을 포함하지 않은 헤더를 파싱하면서 발생합니다. 바디의 첫줄이 그 라인을 나타낸다고 가정하고 계속해서 파싱합니다.
  
## 해결방법
이 메시지는 딱 '에러'라고 표현하기 보단 표준(?) 헤더의 범주에서 좀 벗어난 헤더가 수신되었을 때 발생한다고 보면 된다. 이 메시지가 발생하면 파싱하려는 웹페이지의 인코딩을 살펴보자. UTF-8이 아닌 EUC-KR과 같은 다른 인코딩이 적용되어 있을 가능성이 크다. 그렇다면 requests.get으로 수신한 응답값을 그대로 사용하지 말고 아래와 같이 해당 웹페이지의 인코딩을 적용해주면 문제가 해결된다.
  
~~~
response = requests.get('THE-URL-YOU-WANT-TO-PARSE')
response.encoding = 'EUC-KR'
~~~


