---
layout: post
title: 페이지 내 링크를 사용할 때 고정 상단 네비게이션바가 가리는 문제 해결 방법
tags: html
comments: true
---
  
---
  
## 문제
아래와 같은 방법으로 페이지 내 링크를 걸 수 있는데 고정 상단 네비게이션 바를 붙이면 이동한 곳의 상단부 일부가 가려 안보인다.
  
~~~
<a href="#target">여기를 누르면</a>
  
<a name="target">여기로 화면이 스크롤되어 이동합니다.</a>
~~~
  
---
  
## 해결방법
  
**HTML 파일**
  
~~~
<a href="#target">여기를 누르면</a>
  
<a name="target" class="tm">여기로 화면이 스크롤되어 이동합니다.</a>  # 클래스 추가
~~~
  
**CSS 파일**
~~~
.tm {
    padding-top: 60px;  # 자신의 높이에 맞게 조절
}
~~~  

