---
layout: post
title: 파이썬 테스트 케이스 작성 시 순서와 상관없이 같은지 비교하기
tags: python
comments: true
---
  
---
  
파이썬에서 테스트 케이스 작성 시 assertEqual 함수를 이용해 두 대상의 일치 여부를 비교할 수 있다. 하지만 set 자료형과 같이 그때 그때 순서가 바뀌는 경우면 assertEqual 함수는 적합하지 않다. 어떨 때는 통과했다가, 어떨 때는 실패하기 때문이다. 그럴 때는 아래 함수를 이용하면 원소의 순서와 상관없이 대상의 일치 여부를 비교할 수 있다.
  
~~~
self.assertCountEqual(Source, Target)
~~~
  