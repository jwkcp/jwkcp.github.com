---
layout: post
title: es6 문법을 쓸 때 jshint 경고 표시 끄는 방법
tags: javascript
comments: true
---
           
jshint를 설치하고 나면 import 와 같은 구문이 물결 표시되면서 ES6에서만 쓸 수 있다는 경고가 뜨는 것을 볼 수 있다. 아래와 같이 파일을 만들어 프로젝트 최상위 폴더에 위치해두면 이 경고가 사라진다.    
      
.jshintrc 파일을 아래 내용으로 생성하여 프로젝트 최상위 폴더에 위치    
    
~~~
{
  "esversion": 6
}
~~~
     