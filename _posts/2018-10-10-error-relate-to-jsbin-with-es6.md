---
layout: post
title: jsbin에서 es6 문법에서 오류 표시가 나타나는 경우
tags: tools
comments: true
---
      
[jsbin](https://jsbin.com)을 사용하다보면 문법 경고 또는 에러가 표시될 때가 있다. 이 때 이 에러를 조치하는 방법이다.   

---
        
**ES6 문법이 적용되지 않을 때**     
jsbin 에디터 상단에 JavaScript 라고 되어 있는 드롭다운 박스를 누르면 여러 설정값들이 나온다. 여기서 ES6/Babel을 선택해주면 된다.
      
**use esnext option jsbin** 이란 에러 메시지가 나올 때
이 메시지와 함께 const, 화살표 함수 등에 경고가 표시될 때가 있는 소스 코드 상단에 아래와 같이 써주면 된다.

~~~~
//jshint esnext:true
~~~~

