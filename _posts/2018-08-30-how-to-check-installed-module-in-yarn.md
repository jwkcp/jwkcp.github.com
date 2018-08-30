---
layout: post
title: yarn에서 모듈 설치 여부 확인하기
tags: yarn js
comments: true
---

# 설치여부 조회하기
~~~
yarn list 여기에모듈이름
~~~
     
그러나 위와 같이 사용하는 방법은 곧 폐기(Deprecated) 될 예정이다. 아래와 같이 pattern 옵션을 사용하자.
      
# 설치여부 조회하기 (권장)
~~~
yarn list --pattern 여기에모듈이름
~~~
      
---
     
더 많은 명령은 여기([기본적인 명령](https://yarnpkg.com/en/docs/usage), [기타 다른 CLI 명령](https://yarnpkg.com/en/docs/cli/))를 참고할 것.   