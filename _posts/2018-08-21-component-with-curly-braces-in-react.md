---
layout: post
title: 리액트에서 import 구문과 함께 쓰이는 중괄호는 뭔가요?
tags: react
comments: true
---

리액트에서 자주 쓰이며 ES6 문법 중 라이브러리를 불러오는 아래와 같은 구문이 있다. 쓸 때 마다 궁금했는데 그래서 좀 찾아봤다.
     
~~~
import React, { Component } from 'react';
~~~
     
앞의 React는 알겠는데 Component는 왜 중괄호를 쳐주는 걸까? 이 중괄호 문법을 사용하지 않는다면 아래와 같이 표현할 수 있다.    
     
~~~
import React from 'react';
const Component = React.Component;
~~~
     
이 컴포넌트를 프로퍼티 형태로 호출해 쓰기 위해서 React 클래스 내부에서는 ReactBaseClasses를 호출한다. 다시 ReactBaseClasses 내부를 들여다 보면 Component에 대한 정의와 함께 export { Component, PureComponent } 와 같이 외부에서 사용할 수 있게 해주고 있는 것을 알 수 있다.     
     
---
     
이런 표현법을 [Syntactic sugar](https://en.wikipedia.org/wiki/Syntactic_sugar)라고 부른다. 일종의 간소화된 표기 방법, 간편 표기법인데 위키피디아에서는 아래와 같이 설명하고 있다.     
       
> In computer science, syntactic sugar is syntax within a programming language that is designed to make things easier to read or to express. It makes the language "sweeter" for human use: things can be expressed more clearly, more concisely, or in an alternative style that some may prefer.
   
> 컴퓨터 사이언스 분야에서 syntactic sugar란 뭔가를 좀 더 읽기 쉽고 표현하기 쉽게 디자인된 프로그래밍 언어에서 사용되는 문법의 일종입니다. 이 syntactic sugar는 사람이 언어를 사용하는데 있어 더 매력적이고 명료한 표현을 가지며, 더 간결하고 때론 각자 선호에 따라 다른 스타일로 대체될 수 있습니다.

---
   
ES6는 (위에서 얘기한 것처럼)더 명료하고 간결한 표현으로 자바스크립트를 매력적으로 만들어 줄 Syntactic sugar를 적극적으로 도입하고 있는 것 같습니다. :)
