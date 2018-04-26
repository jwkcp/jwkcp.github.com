---
layout: post
title: 네이버 오픈 API 사용 시 Not Exist Client ID 에러가 발생하는 경우
tags: api
comments: true
---
  
---
  
## 문제
분명히 애플리케이션 등록 하고 허용 기능도 잘 설정했는데 아래와 같은 에러가 발생할 때가 있다.  
  
~~~
Not Exist Client ID : Authentication failed. (인증에 실패했습니다.)
~~~
  
--- 
    
## 원인
http header로 삽입하는 X-Naver-Client-Id 나 X-Naver-Client-Secret 값에 공백이 포함되면 그렇다. 무슨 말인지 아래 코드를 비교해보자.  

**실패 예시**  
~~~
X-Naver-Client-Id : {애플리케이션 등록 시 발급받은 client id 값}
X-Naver-Client-Secret : {애플리케이션 등록 시 발급받은 client secret 값}
~~~
  
**성공 예시**  
~~~
X-Naver-Client-Id: {애플리케이션 등록 시 발급받은 client id 값}
X-Naver-Client-Secret: {애플리케이션 등록 시 발급받은 client secret 값}
~~~
  
두 코드의 차이는 X-Naver-Client-Id와 :(콜론) 사이에 공백 유무다. 그럼 이 공백이 왜 들어갔나 생각하고 테스트 해보니 [네이버 api 예시 사이트](https://developers.naver.com/docs/search/news/)에서 X-Naver-Client-Id 부분을 붙여 넣으면 이상하게 공백이 포함된다. 직접 소스 코딩을 할 경우는 그렇지 않겠지만 Postman이나 Paw와 같은 REST API 툴을 이용하면 그렇더라. request raw 문자열을 보다가 발견했다. 이 공백이 포함되니 네이버는 유효한 헤더가 들어오지 않았다고 판단해 'Not Exist Client ID : Authentication failed. (인증에 실패했습니다.)' 이 에러를 리턴하는 것이다.   
  
--- 
      
## 해결 방법
위의 **성공 예시**처럼 공백을 제거해주면 된다. 복사해서 붙여넣기하면 공백이 들어간다. 손으로 직접 타이핑해보자.
  
--- 
    
## 부가 정보
REST API 호출 툴에서 네이버 api 호출 테스트를 할 경우 '비로그인 오픈API 서비스 환경 > 웹 서비스 URL' http://127.0.0.1 이나 http://localhost를 넣으면 된다.
  