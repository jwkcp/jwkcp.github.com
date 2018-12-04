---
layout: post
title: 리액트 내 클래스에서 화살표 함수 사용 시 에러가 발생하는 경우 해결 방법
tags: react webpack babel
comments: true
---

## 문제

리액트(React)의 클래스(class) 내에서 화살표 함수(arrow function)을 사용하려 할 때 아래와 같은 에러가 발생하는 경우가 있다.

```
./src/app.js
Module build failed (from ./node_modules/babel-loader/lib/index.js):
SyntaxError: Unexpected token (9:12)

   7 |   }
   8 |
>  9 |   getNumber = () => {
     |             ^
  10 |     console.log("getNumber function");
  11 |   };
```

## 해결방법

바벨(babel) 플러그인 [babel-plugin-transform-class-properties](https://www.npmjs.com/package/babel-plugin-transform-class-properties)을 설치해야 한다. 이후 .babelrc에 아래와 같이 해준다.

```
{
  "plugins": ["transform-class-properties"]
}
```
