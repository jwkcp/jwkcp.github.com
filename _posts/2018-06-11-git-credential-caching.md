---
layout: post
title: git에서 정한 시간 동안 비밀번호 묻지 않게 하기
tags: git
comments: true
---
  
아래의 명령을 사용하면 된다. 
~~~
git config --global credential.helper cache
~~~

또, --timeout 옵션 다음에 초(seconds)를 인자로 줄 수 있다. 기본값은 900초(15분)이다.
  
~~~
git config --global credential.helper cache --timeout <seconds>
~~~

예를 들어,
      
### 15분 동안 입력된 계정 정보 기억
~~~
git config --global credential.helper cache --timeout 900
~~~

### 24시간 동안 입력된 계정 정보 기억
~~~
git config --global credential.helper cache --timeout 86400
~~~

### 30일 동안 입력된 계정 정보 기억
~~~
git config --global credential.helper cache --timeout 2592000
~~~
   
자세한 내용은 [여기](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Credential-%EC%A0%80%EC%9E%A5%EC%86%8C)를 참고.
  
