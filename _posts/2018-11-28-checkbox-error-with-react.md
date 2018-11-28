---
layout: post
title: 리액트에서 체크박스 사용 시 에러가 발생할 때
tags: react
comments: true
---

## 문제

리액트(React)에서 체크박스(Checkbox) 사용 시 아래와 같은 에러가 발생할 때가 있다.

> index.js:1452 Warning: Failed prop type: You provided a `checked` prop to a form field without an `onChange` handler. This will render a read-only field. If the field should be mutable use `defaultChecked`. Otherwise, set either `onChange` or `readOnly`.

input type으로 checkbox를 쓸 때 onClick 핸들러를 제공하고 checked 값을 설정하는 식으로 코딩하면 이런 에러 메시지가 발생한다.

## 해결방법

1. onClick 핸들러를 없애고 onChange 핸들러를 사용한다.
2. onClick 핸들러를 그대로 두고 싶으면 **readonly 키워드**를 붙이거나 **checked 속성 대신 defaultChecked를 사용**한다.
