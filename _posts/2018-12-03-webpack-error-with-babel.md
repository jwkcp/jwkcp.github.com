---
layout: post
title: 웹팩 로더(Webpack loader)에서 바벨(babel) 관련 에러가 발생하는 경우
tags: react webpack babel
comments: true
---

## 문제

아래와 같이 웹팩 빌드 시 바벨 관련 에러가 발생한다.

```
ERROR in ./src/app.js
Module build failed (from ./node_modules/babel-loader/lib/index.js):
Error: Cannot find module '@babel/core'
 babel-loader@8 requires Babel 7.x (the package '@babel/core'). If you'd like to use Babel 6.x ('babel-core'), you should install 'babel-loader@7'
```

## 원인

babel-core와 babel-loader가 서로 요구하는 버전이 맞지 않아서 그렇다.

## 해결책

babel-core와 babel-loader의 요구 버전을 맞춰준다.
  
babel-loader 버전8 을 사용하고 있다면 babel-core는 7.x 대를,  
babel-loader 버전7 을 사용하고 있다면 babel-core는 6.x 대를 사용하면 된다.
