---
layout: post
title: vue.js와 django 함께 쓰기
tags: vue.js django
comments: true
---
  
---
  
vue.js를 사용할 때 html 렌더링으로 중괄호 { 와 } 두 개를 쓴다. 그런데 장고의 탬플릿 렌더링 기호도 똑같아서 문제가 생긴다. 이 문제를 가장 쉽게 해결하는 방법은 vue.js의 구획 문자를 다른 것으로 바꿔주는 것이다. [vue.js 공식 예제 페이지](https://kr.vuejs.org/v2/guide/index.html)의 예제를 예로 들면 아래와 같다.  
    
~~~
var app = new Vue({
  delimiters: ['{', '}'],  # 추가. 구획 문자를 원하는 것으로 지정해줄 수 있다.  
  el: '#app',
  data: {
    message: '안녕하세요 Vue!'
  }
})
~~~
  
~~~
<div id="app">
  { message }  # 그럼 이렇게 쓸 수 있다.
</div>
~~~