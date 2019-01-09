---
layout: post
title: 리덕스(Redux) 상태 모니터링 크롬 확장 프로그램
tags: react redux
comments: true
---

[여기](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd)에 가면 Redux DevTools 이란 크롬 확장 프로그램을 받을 수 있다. 설치 후 아래와 같이 스토어(Store) 생성 시 + window.**REDUX_DEVTOOLS_EXTENSION** && window.**REDUX_DEVTOOLS_EXTENSION**() 를 추가해주면 크롬 개발자 모드에서 리덕스의 상태 변화를 볼 수 있다.

compose를 사용한다던지 추가 기능이 알고 싶다면 아래 사이트를 참고하면 된다.  
[https://github.com/zalmoxisus/redux-devtools-extension](https://github.com/zalmoxisus/redux-devtools-extension)

```
const store = createStore(
   reducer, /* preloadedState, */
+  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
 );
```
