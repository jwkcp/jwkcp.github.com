---
layout: post
title: 파이어베이스(firebase) 사용 시 "It looks like you're using..." 과 같은 에러가 발생하는 경우
tags: firebase
comments: true
---

## 문제

파이어베이스(firebase)를 사용하다보면 정상적으로 동작하는데도 불구하고 아래와 같이 콘솔창에 경고가 발생하는 경우가 있다.

```
It looks like you're using the development build of the Firebase JS SDK.
When deploying Firebase apps to production, it is advisable to only import
the individual SDK components you intend to use.

For the module builds, these are available in the following manner
(replace <PACKAGE> with the name of a component - i.e. auth, database, etc):

CommonJS Modules:
const firebase = require('firebase/app');
require('firebase/<PACKAGE>');

ES Modules:
import firebase from 'firebase/app';
import 'firebase/<PACKAGE>';

Typescript:
import * as firebase from 'firebase/app';
import 'firebase/<PACKAGE>';
```

## 원인

파이어베이스 모듈을 전체 통체로 로드해 사용해서 그렇다. 아래와 같이 변경하여 필요한 모듈만 로드하도록 수정하면 경고가 사라진다. 개발이나 테스트 할때는 큰 상관없지만 배포환경에서는 수정 후 배포하도록 하자.

## 해결방법

### AS-IS (BAD)

```
import * as firebase from "firebase";
```

### TO-BE (GOOD)

```
import * as firebase from "firebase/app";
import "firebase/auth";
import "firebase/database";
```
