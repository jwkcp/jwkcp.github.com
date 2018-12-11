---
layout: post
title: 리액트 실행 시 default.a.createContext is not a function 에러가 발생하는 경우
tags: react
comments: true
---

## 문제

아래의 에러가 발생하면서 리액트 실행 시 에러가 발생한다.

> Uncaught TypeError: **WEBPACK*IMPORTED_MODULE_0_react***default.a.createContext is not a function

## 원인

정확히 이것이 원인이다라고 확답할 순 없지만 react 버전을 최신으로 변경하니 발생하던 에러가 없어졌다. 아래의 명령으로 리액트의 버전을 최신으로 올릴 수 있다. 각자 사용하는 라이브러리에 따라 의존성이 있을 수 있으므로 이 부분도 미리 고려하자.

```
yarn upgrade react@latest
```

아래의 명령으로 의존성 에러를 체크할 수 있다.

```
yarn check
```

---

[여기](https://reactjs.org/)에 가면 우측 상단에 최신 리액트 버전이 몇 인지 확인할 수 있다. yarn에서 사용할 수 있는 CLI 명령어는 [여기](https://yarnpkg.com/en/docs/cli/)에서 확인 할 수 있다.
