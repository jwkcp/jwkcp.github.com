---
layout: post
title: ES6 화살표 함수에서 소괄호로 감싸주는 것은 어떤 역할을 하나요?
tags: es6 javascript
comments: true
---
      
ES6의 화살표 함수는 기존 프로그래밍의 함수 구조와 많이 달라 처음 접하면 축약된 표현이 굉장히 낯설고 때론 그 의미가 헷갈린다. 아래와 같이 화살표 다음에 소괄호로 중괄호를 감싸는 구문이 있는데 이것은 중괄호와 리턴을 생략하게 해준다.       
       
예를 들어 아래 구문은 result라는 키와 x, y, z를 더한 값을 가진 사전형(Dictionary) 자료형을 반환한다.
~~~
const foo = (x, y, z) => ({result: x + y + z})
~~~
       
아래와 같이 쓸 수도 있다.     
~~~
const foo = (x, y, z) => {
  return {result: x + y + z}
}
~~~
        
이렇게 쓰는 것도 가능하다.
~~~
function foo(x, y, z) {
  return {result: x + y + z}
}
~~~